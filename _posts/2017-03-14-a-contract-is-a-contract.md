---
title: "A contrat IS a contract"
layout: post
date: 2017-03-14
categories: architecture programming c#
---

The concept of contract is not new, starting from abstract c++ class, going into interface in .NET, there is noting new under the sun.

We can discquisire about correctness, clarity and onesty of a contract, but a contract IS a contract.

The contract in software define how two systems interact, exchange information and/or provide service among them.

## Rule of thumb

When a class implements IDisposable is tell [_"Hey... when you use me you have to call Dispose explicitily at the end!"_](https://msdn.microsoft.com/en-us/library/system.idisposable)

### Simple example

For example EntityFramework DbContext implements IDisposable then we need disposing always when terminate to use it, better to include in an using statment, who grants us that Dispose() is __deterministically__ called

```csharp
public class MyDbContext : DbContext
{
}

using (var ctx = new MyDbContext())
{
    //Using ctx...
}
```

> Is __better__ to  __deterministically__ call Dispose() on Entity Framework context object? [YES](http://stackoverflow.com/questions/21875816/is-disposing-of-entity-framework-context-object-required)

### Aggregating example

In some cases when we must to implemente a specific pattern (for example [Unit Of Work](https://martinfowler.com/eaaCatalog/unitOfWork.html)), whe need to [aggregating](https://en.wikipedia.org/wiki/Class_diagram#Aggregation) a disposable object: one best way to do it is to implement IDisposable and then call Dispose on wrapped Disposable object.

```csharp
public class UnitOfWork : IDisposable
{
    readonly MyDbContext _context = new MyDbContext();

    public void Dispose()
    {
        _context?.Dispose();
    }
}
```
