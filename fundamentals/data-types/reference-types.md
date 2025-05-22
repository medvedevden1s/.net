# Reference types

### Introduction

Reference types **do not store their data directly** in the variable. Instead, they hold a **reference to a memory location** on the **heap**, where the actual data resides.&#x20;

This means:

* Multiple variables can point to the **same object**.
* Modifying the object via one variable affects all references to that object.

You can see code example here [stack-and-heap.md](../../memory-management/stack-and-heap.md "mention")&#x20;



**Reference types in C# fall into two main categories:**

#### Built-in Reference Types

These are provided by the .NET runtime and commonly used:

* `object` — The root of all types.
* `dynamic` — Skips compile-time type checking.
* `string` — Immutable sequence of Unicode characters.
* Arrays — Fixed-size collections of elements.

#### Custom Reference Types

You define these in your own code:

* `class` — Most common type for defining objects.
* `interface` — Contract that classes/structs can implement.
* `delegate` — Reference to one or more methods.
* `record` — Immutable reference type with value-based equality

***

### Built-in Reference Types

#### `object`

The `object` type (`System.Object`) is the **base type** from which all other types in C#—both value and reference—derive. You can assign any type to an `object`.

```csharp
object obj = 42;
Console.WriteLine(obj); // 42
```

#### `dynamic`

The `dynamic` type **defers type checking** until runtime, making it useful when interacting with JSON, COM APIs, dynamic languages

```csharp
dynamic val = 10;
Console.WriteLine(val); // 10

val = "Now I'm a string!";
Console.WriteLine(val); // Now I'm a string!
```

{% hint style="info" %}
`dynamic` is like `object`, but with **runtime** type resolution. It trades safety for flexibility.
{% endhint %}

#### `string`

`string` (alias for `System.String`) represents **immutable sequences** of Unicode characters.

```csharp
string first = "Jane";
string last = "Doe";
string full = first + " " + last;
Console.WriteLine(full); // Jane Doe
```

{% hint style="info" %}
Use `StringBuilder` for **efficient string modifications** in loops or large operations.
{% endhint %}

#### Arrays

Arrays are reference types that store **fixed-size sequences** of elements of the same type.

```csharp
string[] names = { "Alice", "Bob", "Charlie" };

foreach (var name in names)
{
    Console.WriteLine(name);
}
```

***

### Custom Reference Types

#### `class`

Defines a user-created type that can contain fields, methods, and properties.

```csharp
public class Product
{
    public string Description;
    public decimal Price;
}
```

Used for modeling real-world entities and behaviors.

***

#### `interface`

Describes a contract. Any type that implements the interface must provide implementations for its members.

```csharp
public interface ILogger
{
    void Log(string message);
}
```

Interfaces allow for **abstraction and loose coupling** in your design.

***

#### `delegate`

A type that represents **a reference to a method**.

```csharp
public delegate void Notify(string message);
```

Commonly used for **event handling** and callbacks.

***

#### `record`

Introduced in C# 9, `record` is a reference type that provides **value-based equality** and **immutable** behavior by default.

```csharp
public record Person(string FirstName, string LastName);
```

Ideal for **data-carrying types** like DTOs or immutable models.

***

### Key points

* Reference types **store memory addresses**, not the actual data, actual data is always stored on the **heap**.
* You can create **custom reference types** using `class`, `interface`, `delegate`, and `record`.
* Built-in reference types include `object`, `dynamic`, `string`, and arrays.
* Reference types can be assigned `null`.



### FAQs

#### What’s the difference between value types and reference types?

* **Value types** store the actual value in the variable itself.
* **Reference types** store a **pointer to data** on the heap.

> Changing one reference variable affects all others pointing to the same object.

***

#### Can reference type variables be `null`?

Yes. `null` means the variable **does not reference any object**.

```csharp
Product product = null;
```

Accessing a member of a `null` reference will result in a `NullReferenceException`.

***

#### Why does `string` behave like a value type?

Although `string` is a reference type, it is **immutable**. Any change to a string results in the creation of a **new string instance**, giving it **value-type-like behavior**.

***

#### When should I use `dynamic`?

Use `dynamic` when:

* You don’t know the type at compile time
* You’re working with **JSON**, **ExpandoObject**, or flexible data
* You want maximum flexibility at the cost of compile-time safety

***

#### What’s the difference between `object` and `dynamic`?

| Feature        | `object`                | `dynamic`     |
| -------------- | ----------------------- | ------------- |
| Type Checking  | Compile-time            | Runtime       |
| Flexibility    | Less flexible           | Very flexible |
| Casting Needed | Yes (to access members) | No            |

```csharp
object obj = "Hello";
// Console.WriteLine(obj.Length); ❌ Compile-time error

Console.WriteLine(((string)obj).Length); // ✅ Must cast to access string members

dynamic dyn = "Hello";
Console.WriteLine(dyn.Length); // ✅ Works without casting

```

#### Lear more [implicit-and-explicit-conversions-casting.md](implicit-and-explicit-conversions-casting.md "mention")

***

#### Are arrays reference types?

Yes. Arrays in C# are **always reference types**, even if they store value-type elements (like `int[]`).

***

#### Can I assign one reference variable to another?

Yes, but both variables will **point to the same object** in memory.

```csharp
var a = new Product();
var b = a;
b.Description = "Updated"; 
Console.WriteLine(a.Description); // Outputs: Updated
```



