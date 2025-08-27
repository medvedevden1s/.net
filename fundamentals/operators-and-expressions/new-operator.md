## Introduction

The `new` operator is like a factory worker on an assembly line. Just as a factory worker takes raw materials and assembles them into a finished product, the `new` operator takes a blueprint (a type) and creates a functioning instance of that type. It's the bridge between the abstract world of type definitions and the concrete world of objects that your program can interact with.

In the world of programming, types are just templates until they're instantiated. The `new` operator is the magic wand that brings these templates to life, allocating memory, initializing fields, and calling constructors to prepare the object for use. Without it, your classes and structs would remain theoretical concepts, unable to store data or perform actions in your running program.

## Constructor invocation

To create a new instance of a type, you typically invoke one of the constructors of that type using the `new` operator:

```csharp
var dict = new Dictionary<string, int>();
dict["first"] = 10;
dict["second"] = 20;
dict["third"] = 30;

Console.WriteLine(string.Join("; ", dict.Select(entry => $"{entry.Key}: {entry.Value}")));
// Output:
// first: 10; second: 20; third: 30
```

You can use an object or collection initializer with the `new` operator to instantiate and initialize an object in one statement, as the following example shows:

```csharp
var dict = new Dictionary<string, int>
{
    ["first"] = 10,
    ["second"] = 20,
    ["third"] = 30
};

Console.WriteLine(string.Join("; ", dict.Select(entry => $"{entry.Key}: {entry.Value}")));
// Output:
// first: 10; second: 20; third: 30
```

## Target-typed new

Constructor invocation expressions are target-typed. That is, if a target type of an expression is known, you can omit a type name, as the following example shows:

```csharp
List<int> xs = new();
List<int> ys = new(capacity: 10_000);
List<int> zs = new() { Capacity = 20_000 };

Dictionary<int, List<int>> lookup = new()
{
    [1] = new() { 1, 2, 3 },
    [2] = new() { 5, 8, 3 },
    [5] = new() { 1, 0, 4 }
};
```

As the preceding example shows, you always use parentheses in a target-typed `new` expression.

{% hint style="info" %}
If a target type of a `new` expression is unknown (for example, when you use the `var` keyword), you must specify a type name.
{% endhint %}

## Array creation

You also use the `new` operator to create an array instance, as the following example shows:

```csharp
var numbers = new int[3];
numbers[0] = 10;
numbers[1] = 20;
numbers[2] = 30;
Console.WriteLine(string.Join(", ", numbers));
// Output:
// 10, 20, 30
```

Use array initialization syntax to create an array instance and populate it with elements in one statement. The following example shows various ways how you can do that:

```csharp
var a = new int[3] { 10, 20, 30 };
var b = new int[] { 10, 20, 30 };
var c = new[] { 10, 20, 30 };
Console.WriteLine(c.GetType());  // output: System.Int32[]
```

## Instantiation of anonymous types

To create an instance of an anonymous type, use the `new` operator and object initializer syntax:

```csharp
var example = new { Greeting = "Hello", Name = "World" };
Console.WriteLine($"{example.Greeting}, {example.Name}!");
// Output:
// Hello, World!
```

## Destruction of type instances

You don't have to destroy earlier created type instances. Instances of both reference and value types are destroyed automatically. Instances of value types are destroyed as soon as the context that contains them is destroyed. Instances of reference types are destroyed by the garbage collector at some unspecified time after the last reference to them is removed.

{% hint style="warning" %}
For type instances that contain unmanaged resources, for example, a file handle, it's recommended to employ deterministic clean-up to ensure that the resources they contain are released as soon as possible. For more information, see the System.IDisposable API reference and the using statement article.
{% endhint %}

## Operator overloadability

A user-defined type can't overload the `new` operator.

## Key Points

- The `new` operator creates a new instance of a type
- It can be used to invoke constructors, create arrays, and instantiate anonymous types
- Target-typed `new` expressions allow you to omit the type name when the target type is known
- Object and collection initializers can be used with the `new` operator for concise initialization
- Instances are automatically destroyed by the runtime when they're no longer needed
- The `new` operator cannot be overloaded by user-defined types

## FAQs

**Q: What's the difference between `new()` and `new T()`?**  
A: `new()` is a target-typed `new` expression (available in C# 9.0 and later) where the type is inferred from the context. `new T()` explicitly specifies the type to instantiate.

**Q: Can I use the `new` operator with value types like `int` or `double`?**  
A: Yes, but it's rarely necessary. Value types are automatically initialized when declared. However, you can use `new int()` to get the default value (0) of an `int`.

**Q: What happens when I use the `new` operator with a struct?**  
A: The `new` operator creates a new instance of the struct and calls its constructor. If you don't provide a constructor, the default constructor initializes all fields to their default values.

**Q: How does memory allocation work with the `new` operator?**  
A: For reference types, the `new` operator allocates memory on the heap. For value types, the memory is allocated wherever the variable is declared (often on the stack for local variables).

**Q: Can I create an instance of an abstract class or interface using the `new` operator?**  
A: No, you cannot directly instantiate abstract classes or interfaces with the `new` operator. You need to instantiate a concrete class that implements the interface or extends the abstract class.

**Q: What's the relationship between the `new` operator and constructors?**  
A: The `new` operator invokes a constructor to initialize the newly created instance. If no constructor is explicitly defined, the default constructor is called.

**Q: How does the `new` operator work with generics?**  
A: The `new` operator can create instances of generic types. You must specify the type arguments unless you're using target-typed `new` or type inference.