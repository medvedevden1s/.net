# The fixed statement

### Introduction

The `fixed` statement pins a variable in memory, preventing the **garbage collector (GC)** from moving it. This is important when working with **pointers** inside `unsafe` code. Normally, the GC can relocate objects during memory compaction, which would invalidate any pointers. By using `fixed`, you ensure that the variable stays at the same memory address for the duration of the block.

***

### Using `fixed` With Arrays

When applied to an array, the pointer points to the **first element**.

```csharp
unsafe
{
    byte[] bytes = { 1, 2, 3 };

    fixed (byte* pointerToFirst = bytes) // pins array
    {
        Console.WriteLine($"Address: {(long)pointerToFirst:X}");
        Console.WriteLine($"Value: {*pointerToFirst}"); // dereference -> 1
    }
}

// Output:
// Address: 2173F80B5C8
// Value: 1
```

{% hint style="warning" %}
The pointer declared inside a `fixed` block is **readonly** – you cannot reassign it to another memory location.
{% endhint %}

***

### Pinning Variables With `&`

You can also pin a **specific variable** or element using the `&` (address-of) operator.

```csharp
unsafe
{
    int[] numbers = { 10, 20, 30 };

    fixed (int* toFirst = &numbers[0], toLast = &numbers[^1]) // pins first & last
    {
        Console.WriteLine(toLast - toFirst); // output: 2 (distance between elements)
    }
}
```

***

### Pinning Object Fields

Fields inside objects are moveable too. `fixed` ensures the **whole object** isn’t moved while the field is pinned.

***

### With `Span<T>` and `GetPinnableReference`

Types like `Span<T>` and `ReadOnlySpan<T>` can also be pinned because they implement `GetPinnableReference()`.

```csharp
unsafe
{
    int[] numbers = { 10, 20, 30, 40, 50 };
    Span<int> middle = numbers.AsSpan()[1..^1]; // {20, 30, 40}

    fixed (int* p = middle)
    {
        for (int i = 0; i < middle.Length; i++)
        {
            Console.Write(p[i]);
        }
        // output: 203040
    }
}
```

***

### Pinning Strings

Strings are also moveable. With `fixed`, you can safely access characters via pointers.

```csharp
unsafe
{
    string message = "Hello!";

    fixed (char* p = message)
    {
        Console.WriteLine(*p); // output: H
    }
}
```

***

### Fixed-Size Buffers

In **unsafe contexts**, you can also use `fixed` to declare a **fixed-size buffer** inside a struct.

```csharp
unsafe struct Buffer
{
    public fixed int items[5]; // fixed-size buffer
}
```

***

### Alternatives to `fixed`

* **`stackalloc`** – allocates memory on the stack, which is never moved by GC.
* **Safe managed code** – prefer safe constructs unless low-level performance/pinning is required.

{% hint style="danger" %}
Code with `unsafe` and `fixed` must be compiled with the **AllowUnsafeBlocks** option.&#x20;

Such code is not **memory safe** and should only be used when absolutely necessary.
{% endhint %}

***

### Key Points

* `fixed` pins variables so their memory addresses don’t change during GC.
* Works with arrays, variables, object fields, strings, spans, and fixed-size buffers.
* Pointers declared in a `fixed` block are **readonly** and only valid inside the block.
* Use `stackalloc` when you want GC-free stack memory without pinning.

***

### FAQs

**Why do we need `fixed` if pointers exist?**\
Because GC may relocate objects. Without pinning, a pointer could suddenly point to invalid memory.

**Can I use `fixed` in safe code?**\
No, it requires an `unsafe` context.

**Is pinning bad for performance?**\
Excessive pinning can hurt GC performance since pinned objects reduce memory compaction efficiency. Use it sparingly.

**Can I reassign a `fixed` pointer?**\
No. It is readonly for safety reasons.

**Should I prefer `stackalloc` over `fixed`?**\
Yes, if you only need temporary stack memory. Use `fixed` when you must access heap objects directly with pointers.
