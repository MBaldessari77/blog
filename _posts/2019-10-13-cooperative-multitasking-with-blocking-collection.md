---
title: "Cooperative Multitasking: example using BlockingCollection"
layout: post
date: 2019-10-13
categories: tip multitasking code-snippet net.core c#
---

# Introduction

Write performance and scalable multi-tasking application using a cooperative approach instead of a preemptive approach.

## Problem

How to better use multi CPU cores resource in case of single many task wich different CPU resources needed.

## Example

We want to calculate primes count from 2 to N using a brute force algorithm. The latter is an interesting problem to resolve in a multi-tasking way because the duration of single tasks depends on N magnitude.

Using a standard multi-task algorithm, for example, clustering tasks based on how many cores are available, lead to an unoptimized use of computing resources. An alternative is using a cooperative multitasking approach to better use computation resources.

## Practically

Not optimized brute force alghoritm.

```csharp
        private static bool IsPrime(int n)
        {
            for (int y = 2; y < n; y++)
            {
                if (n % y != 0)
                    continue;
                return false;
            }
            return true;
        }
```

Count prime from 2..N where N = 1.000.000 using Parallel Linq Extensions.

```csharp
        private static void LoadPreemptiveMultitaskingEngineTest()
        {
            List<Action> tasks = new List<Action>();

            int primeCount = 0;

            for (int x = 2; x < OneMillion; x++)
            {
                int num = x;
                tasks.Add(() =>
                {
                    if (IsPrime(num))
                        Interlocked.Increment(ref primeCount);
                });
            }

            tasks.AsParallel().ForAll(t => t());

            //See https://primes.utm.edu/howmany.html
            Debug.Assert(primeCount == 78498);
        }
```

Count prime from 2..N where N = 1.000.000 using a Cooperative Engine.

```csharp
        private static void LoadCooperativeMultitaskingEngineTest()
        {
            List<Action> tasks = new List<Action>();

            int primeCount = 0;

            for (int x = 2; x < OneMillion; x++)
            {
                int num = x;
                tasks.Add(() =>
                {
                    if (IsPrime(num))
                        Interlocked.Increment(ref primeCount);
                });
            }

            var engine = new CooperativeEngine();

            foreach (var task in tasks)
                engine.AddAction(task);

            engine.Start();
            engine.WaitForCompletion();

            //See https://primes.utm.edu/howmany.html
            Debug.Assert(primeCount == 78498);
        }
```

Cooperative engine

```csharp
using System.Collections.Concurrent;
...

    public class CooperativeEngine
    {
        private readonly BlockingCollection<Action> _actions = new BlockingCollection<Action>();
        private readonly ManualResetEventSlim _finished = new ManualResetEventSlim();

        private int _runningThreadCount;
        private bool _finishing;

        public void AddAction(Action action)
        {
            _actions.Add(action);
        }

        public void Start()
        {
            for (int x = 0; x < Environment.ProcessorCount; x++)
            {
                Interlocked.Increment(ref _runningThreadCount);
                var t = new Thread(Engine);
                t.IsBackground = true;
                t.Start();
            }
        }

        public void WaitForCompletion(int millisecondsTimeout = -1)
        {
            _finishing = true;
            _actions.CompleteAdding();
            _finished.Wait(millisecondsTimeout);
        }

        private void Engine()
        {
            try
            {
                while (true)
                {
                    bool taken = _actions.TryTake(out Action action, -1);

                    if (taken)
                    {
                        action();
                        continue;
                    }

                    if (_finishing)
                        return;
                }
            }
            finally
            {
                int count = Interlocked.Decrement(ref _runningThreadCount);
                if (count == 0)
                    _finished.Set();
            }
        }
    }
```

## Result

LoadPreemptiveMultitaskingEngineTest:  14.82s \*

LoadCooperativeMultitaskingEngineTest: 10.30s (-30%) \*

> \* Tested on I7-7800X 6 cores 12 logic cores

## Conclusion

Modern CPUs will have more and more cores, and we must take this into account if we want to develop algorithms that make the most of these resources.
