# Pointers

### Introduction

P**ointer type** stores the **memory address** of another variable. While C# is generally a type-safe language, it allows direct memory manipulation using **unsafe code blocks**, similar to C or C++.

> Pointer types are considered _unsafe_ and are typically used in low-level scenarios like performance-critical code or interop with native APIs.

***

### Using a pointer

```csharp
using System;

unsafe class Program
{
    static void Main()
    {
        int grade = 90;
        int* ptr = &grade;

        Console.WriteLine("Original Grade: " + grade);
        Console.WriteLine("Memory Address: " + (ulong)ptr);

        *ptr = 95; // Modifying the value using pointer
        Console.WriteLine("Updated Grade: " + grade);
    }
}
```

This example gets the address of the `grade` variable, stores it in a pointer, and then updates its value through the pointer.

```
Original Grade: 90
Memory Address: 864277752408
Updated Grade: 95
```

***

### Pointers with managed objects

Normally, pointer types in C# are used with **unmanaged types** — such as `int`, `double`, `bool`, and structs that contain only unmanaged fields — because these types have a **fixed, predictable layout** in memory and are **not moved by the Garbage Collector (GC)**. Reference types (like `string`, `class`, or arrays):

* Are managed by the **.NET Garbage Collector**
* Can be **relocated in memory** during GC cycles
* Include metadata and headers that make direct memory access unsafe

> Using raw pointers to reference types without precautions can lead to **undefined behavior**, memory corruption, or application crashes.

You **can** use pointers with managed objects by **pinning** them using the `fixed` keyword. This prevents the GC from moving the object during the pointer's lifetime.



```csharp
string myString = "Hello";

fixed (char* ptr = myString)
{
    Console.WriteLine(*ptr);
}
```

In this example:

* `fixed` ensures that `myString` **stays at its current memory location**
* The pointer `ptr` safely accesses the underlying character data

### Key Rules for Pointer Types

* Only valid in **unsafe** contexts.
* Only variables of **unmanaged types** (e.g., `int`, `double`, `char`) can be pointed to.
* Pointers can use:
  * `*` to declare
  * `&` to get the address
  * `->` to access members (used with structs)
  * `[]` to index memory blocks

***

### When to Use Pointers

* Interfacing with native code (e.g., C libraries via P/Invoke)
* Performance-critical operations where bounds checking adds overhead
* Working with memory-mapped files or hardware buffers

{% hint style="warning" %}
Avoid pointers unless absolutely necessary. In most cases, **Span\<T>**, **ref**, or high-performance types in `System.Memory` are safer alternatives.
{% endhint %}

### FAQs

#### Is pointer usage safe in C#?

No. Pointer usage is marked as `unsafe` for a reason — it **bypasses type safety**, memory bounds checking, and garbage collection guarantees. Mistakes can lead to **crashes**, **memory corruption**, or **security vulnerabilities**.

***

#### Do I always need to use `unsafe` for pointers?

Yes. Any code that declares or uses pointers must be inside an `unsafe` context.

You must also:

* Enable **unsafe code** in your project settings, or
* Compile with `/unsafe` (for example: `csc /unsafe Program.cs`)

***

#### Can I use pointers with reference types?

**Not directly.** Reference types (like `string`, `class`, or arrays) are **managed by the GC** and can be moved in memory, which makes raw pointers to them unsafe.

However, you **can use pointers** with them by **pinning** the object using the `fixed` keyword:

```csharp
string text = "Hello";
fixed (char* ptr = text)
{
    Console.WriteLine(*ptr);
}
```

> `fixed` prevents the GC from moving the object while the pointer is in use.

***

#### Why can pointers normally only point to unmanaged types?

Unmanaged types (e.g., `int`, `double`, `bool`, and simple structs) have a **fixed, predictable layout** and aren't moved by the Garbage Collector — making them safe for direct memory access.

***

#### What are safer alternatives to pointers?

Most of the time, pointer use can be avoided using safer, modern features:

* `ref` / `out` parameters — for passing variables by reference
* `Span<T>` and `Memory<T>` — for safe stack-based or heap-based memory access
* `Marshal` and `GCHandle` — for working with unmanaged memory or interop\\

{% hint style="info" %}
If you're working with buffers, slices, or native interop — prefer `Span<T>` or `Memory<T>` unless raw pointers are absolutely necessary.
{% endhint %}
