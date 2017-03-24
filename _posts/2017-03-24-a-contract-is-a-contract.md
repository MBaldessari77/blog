---
title: "A contrat IS a contract"
layout: post
date: 2017-03-24
categories: architecture programming c#
---

The concept of contract is not new, starting from abstract c++ class, or going to .NET interface, there is nothing new under the sun.

We can quibble about correctness, clarity and honesty of a contract, but a contract IS a contract.

The contract in software defines how two systems interact, exchange information and/or provide service among them.

## Rule of thumb

When a class implements IDisposable tells [_"Hey... when you use me you have to call Dispose explicitly at the end!"_](https://msdn.microsoft.com/en-us/library/system.idisposable)

### Simple example

For example, [Entity Framework](https://en.wikipedia.org/wiki/Entity_Framework) [DbContext](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext) class implements IDisposable then we want to dispose always when terminate to use it, so is better to include in an using statement, wich grants that Dispose() is __deterministically__ called.

```csharp
public class MyDbContext : DbContext
{
}

using (var ctx = new MyDbContext())
{
    //Using ctx...
}
```

> Is then __better__ to  __deterministically__ call Dispose() on Entity Framework context object? [YES](http://stackoverflow.com/questions/21875816/is-disposing-of-entity-framework-context-object-required)

### Aggregating example

In some cases when we must to implemente a specific pattern (for example [Unit Of Work](https://martinfowler.com/eaaCatalog/unitOfWork.html)), we need to [aggregating](https://en.wikipedia.org/wiki/Class_diagram#Aggregation) a disposable object: one best way to do it is to implement IDisposable and then call Dispose() on wrapped Disposable object.

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
