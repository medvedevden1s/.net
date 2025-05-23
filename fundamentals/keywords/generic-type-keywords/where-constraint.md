# Where constraint

### Introduction

The `where` keyword in generics lets you specify conditions that generic types or methods must meet. Constraints tell the compiler exactly what types it can accept, ensuring they provide specific behaviors or capabilities.

Think of generic constraints like rules for entry: they ensure the type you use meets certain criteria (like having a specific base class, implementing an interface, or being a struct).

Let's explore clearly how and when to use these constraints effectively.

### Interface Constraint

Use the interface constraint when the type must implement a specific interface:

```csharp
// T must implement the IComparable<T> interface
public class SortedCollection<T> where T : IComparable<T>
{
    public void Add(T item) { /* Add logic using CompareTo */ }
}
```

> **Why this constraint?** To ensure that the type supports comparison operations (like sorting).

### Base Class Constraint

The base class constraint ensures the generic type inherits from or matches a specific class. It's especially useful for reusing logic or enforcing shared behavior:

```csharp
public class LoggerBase
{
    public void Log(string message) => Console.WriteLine($"Log: {message}");
}

public class Logger<T> where T : LoggerBase
{
    private T _logger;

    public Logger(T logger) => _logger = logger;

    public void LogMessage(string msg) => _logger.Log(msg);
}
```

When you have shared logic (like logging or validation) defined in a base class, and you want your generic type to always support it.

{% hint style="warning" %}
You **can't use** `Object`, `Array`, or `ValueType` as a base class constraint.
{% endhint %}

### Struct and Class Constraints

The `struct` and `class` constraints specify whether the type argument must be a value type (`struct`) or a reference type (`class`):

```csharp
// T must be a reference type (class)
public class ReferenceStore<T> where T : class { }

// U must be a value type (struct)
public class ValueStore<U> where U : struct { }
```

> Use `class?` if you want to allow both nullable and non-nullable reference types.

### Notnull Constraint

The `notnull` constraint ensures the type parameter cannot be nullable:

```csharp
public class NotNullCollection<T> where T : notnull
{
    private readonly List<T> _items = new();

    public void Add(T item) => _items.Add(item);
}
```

{% hint style="warning" %}
`notnull` generates a compiler warning (not an error) if a nullable type is used.
{% endhint %}

### Unmanaged Constraint

The `unmanaged` constraint is for types that don't need garbage collection (primitive types, structs without reference fields, etc.). It's typically used in low-level operations like interop:

```csharp
// T must be an unmanaged type (int, float, simple structs, etc.)
public class Buffer<T> where T : unmanaged
{
    private unsafe T* _buffer;
}
```

{% hint style="warning" %}
You **can't combine** `unmanaged` with the `class` or `struct` constraints (though implicitly it applies only to structs).
{% endhint %}

Constructor (`new()`) Constraint

The `new()` constraint ensures the type parameter has a public parameterless constructor, allowing you to instantiate the type:

```csharp
public class Factory<T> where T : new()
{
    public T CreateInstance() => new T();
}
```

{% hint style="success" %}
Combine `new()` with other constraints. For example, ensure a type is comparable **and** easily created:
{% endhint %}

```csharp
public class ComparableFactory<T> where T : IComparable<T>, new()
{
    public T CreateAndCompare(T other)
    {
        var instance = new T();
        return instance.CompareTo(other) >= 0 ? instance : other;
    }
}
```

### Allows Ref Struct Anti-Constraint

The `allows ref struct` anti-constraint permits using ref struct types as generic parameters:

```csharp
public class RefStructHolder<T> where T : allows ref struct
{
    public void Handle(scoped T parameter) { }
}
```

{% hint style="warning" %}
`allows ref struct` can't be combined with `class` or `class?` constraints.
{% endhint %}

### Default Constraint (Advanced)

The `default` constraint solves ambiguity when overriding methods with nullable parameters:

```csharp
public abstract class BaseClass
{
    public void Method<T>(T? item) where T : struct { }
    public abstract void Method<T>(T? item);
}

public class DerivedClass : BaseClass
{
    // Explicitly override abstract method without struct constraint
    public override void Method<T>(T? item) where T : default { }
}
```

{% hint style="info" %}
Use `default` only on method overrides or explicit interface implementations.
{% endhint %}

### Multiple Constraints

When you have multiple type parameters, define constraints separately for clarity:

```csharp
public interface IService { }

public class Manager<TKey, TValue>
    where TKey : IComparable<TKey>
    where TValue : IService, new()
{
    public void Process(TKey key, TValue service)
    {
        service = new TValue();
        // Use key and service...
    }
}
```

***

### Key Points:

* Constraints ensure your generic types/methods have specific capabilities.
* You can enforce implementation of interfaces, inheritance from base classes, or structural constraints like `class`, `struct`, or `unmanaged`.
* The `new()` constraint ensures easy instantiation of types.
* Advanced constraints (`allows ref struct`, `default`) manage edge-case scenarios like ref structs or method overrides.

***

### FAQs

**Can I combine multiple constraints?**\
Yes, except when explicitly restricted (e.g., `struct` and `class` can't combine).

**Why can't I constrain types to inherit from `Object` or `ValueType`?**\
These are too general (every type inherits from them), making the constraint meaningless.

**What's the purpose of the `notnull` constraint?**\
To prevent types that allow null values, reducing potential null-reference exceptions.

**Why use the `unmanaged` constraint?**\
It’s useful for low-level interop scenarios and improves performance by ensuring the type isn't managed by the GC.

**When should I use `allows ref struct`?**\
When your generic method or type needs to explicitly handle ref structs.



