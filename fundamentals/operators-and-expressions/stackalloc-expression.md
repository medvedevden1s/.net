## Introduction

The `stackalloc` expression is like a quick, temporary workspace on your desk. Imagine you're working on a project and need some scratch paper for calculations that you'll discard as soon as you're done. Instead of going to the supply closet (the heap) to get a fresh notebook that will eventually need to be recycled, you simply use a corner of your desk (the stack) for these short-lived notes.

In the world of programming, memory allocation is a critical concern. Most objects in C# are allocated on the managed heap, where they're subject to garbage collection. But sometimes, especially in performance-critical code, you need a faster, more direct way to allocate memory without the overhead of garbage collection. The `stackalloc` expression provides this capability by allocating memory directly on the stack, making it ideal for small, short-lived buffers that don't need to outlive the method they're created in.

## How it works

A `stackalloc` expression allocates a block of memory on the stack. This memory is automatically discarded when the method returns, without requiring garbage collection:

```csharp
int length = 3;
Span<int> numbers = stackalloc int[length];
for (var i = 0; i < length; i++)
{
    numbers[i] = i;
}
```

In this example, we allocate an array of 3 integers directly on the stack and then initialize each element. The memory will be automatically freed when the method exits.

{% hint style="info" %}
Stack-allocated memory is extremely fast to allocate and deallocate, but it's limited in size and scope. It's perfect for small, temporary buffers but not suitable for large or long-lived data structures.
{% endhint %}

## Using with Span<T> and ReadOnlySpan<T>

The most common and safest way to use `stackalloc` is with `Span<T>` or `ReadOnlySpan<T>`:

```csharp
int length = 3;
Span<int> numbers = stackalloc int[length];
for (var i = 0; i < length; i++)
{
    numbers[i] = i;
}
```

When you use `stackalloc` with these types, you don't need to use an unsafe context, making your code safer and more maintainable.

### Conditional allocation

You can use `stackalloc` in conditional expressions to decide whether to allocate on the stack or the heap based on the size:

```csharp
int length = 1000;
Span<byte> buffer = length <= 1024 ? stackalloc byte[length] : new byte[length];
```

This pattern is very useful when you want to use stack allocation for small buffers (for performance) but fall back to heap allocation for larger ones (to avoid stack overflow).

### Using in expressions

You can use `stackalloc` expressions inside other expressions wherever a `Span<T>` or `ReadOnlySpan<T>` is expected:

```csharp
Span<int> numbers = stackalloc[] { 1, 2, 3, 4, 5, 6 };
var ind = numbers.IndexOfAny(stackalloc[] { 2, 4, 6, 8 });
Console.WriteLine(ind);  // output: 1
```

Starting with C# 12, you can also use collection expressions for the same purpose:

```csharp
Span<int> numbers2 = [1, 2, 3, 4, 5, 6];
var ind2 = numbers2.IndexOfAny([2, 4, 6, 8]);
Console.WriteLine(ind2);  // output: 1
```

## Using with pointer types (unsafe)

In unsafe code, you can use `stackalloc` with pointer types:

```csharp
unsafe
{
    int length = 3;
    int* numbers = stackalloc int[length];
    for (var i = 0; i < length; i++)
    {
        numbers[i] = i;
    }
}
```

As the example shows, you must use an unsafe context when working with pointer types. This approach is less recommended in modern C# code, as the `Span<T>` approach provides similar performance with better safety.

{% hint style="warning" %}
When using pointer types, you're responsible for ensuring you don't access memory outside the allocated block, which can lead to security vulnerabilities or program crashes.
{% endhint %}

## Best practices

### 1. Limit allocation size

The stack has limited size (typically a few MB), so you should only use `stackalloc` for small allocations:

```csharp
const int MaxStackLimit = 1024;
Span<byte> buffer = inputLength <= MaxStackLimit ? stackalloc byte[MaxStackLimit] : new byte[inputLength];
```

### 2. Avoid using in loops

Allocating memory on the stack inside a loop can quickly lead to stack overflow:

```csharp
// Bad practice
for (int i = 0; i < iterations; i++)
{
    Span<byte> buffer = stackalloc byte[1024]; // Allocates 1024 bytes on each iteration!
    // Use buffer...
}

// Good practice
Span<byte> buffer = stackalloc byte[1024]; // Allocate once
for (int i = 0; i < iterations; i++)
{
    // Reuse the same buffer
    buffer.Clear(); // Reset the buffer if needed
    // Use buffer...
}
```

### 3. Initialize allocated memory

Unlike memory allocated with `new`, stack-allocated memory is not automatically initialized to zero:

```csharp
Span<int> numbers = stackalloc int[10];
numbers.Clear(); // Initialize all elements to 0

// Or use an initializer
Span<int> initialized = stackalloc int[] { 1, 2, 3 };
```

## Array initializers

You can use array initializer syntax to define the content of stack-allocated memory:

```csharp
Span<int> first = stackalloc int[3] { 1, 2, 3 };
Span<int> second = stackalloc int[] { 1, 2, 3 };
ReadOnlySpan<int> third = stackalloc[] { 1, 2, 3 };

// Using collection expressions (C# 12+):
Span<int> fourth = [1, 2, 3];
ReadOnlySpan<int> fifth = [1, 2, 3];
```

## Security considerations

The use of `stackalloc` automatically enables buffer overrun detection features in the CLR. If a buffer overrun is detected, the process is terminated quickly to minimize the chance of malicious code execution.

However, this doesn't mean you can be careless with bounds checking. Always ensure you're not accessing memory outside the allocated block, especially when using pointer types in unsafe code.

## Performance implications

Stack allocation is significantly faster than heap allocation for several reasons:

1. No garbage collection overhead
2. Better CPU cache locality
3. No synchronization required (stack memory is thread-local)
4. No memory fragmentation concerns

This makes `stackalloc` ideal for performance-critical code paths where you need small, temporary buffers.

## Key Points

- The `stackalloc` expression allocates memory on the stack, which is automatically freed when the method returns
- Stack-allocated memory is not subject to garbage collection
- You can use `stackalloc` with `Span<T>`, `ReadOnlySpan<T>`, or pointer types (in unsafe code)
- Using with `Span<T>` or `ReadOnlySpan<T>` doesn't require unsafe code
- Stack allocation is limited in size; use it only for small, temporary buffers
- Unlike `new`, memory allocated with `stackalloc` is not automatically initialized
- You can use array initializers or collection expressions with `stackalloc`
- The CLR includes buffer overrun detection for stack-allocated memory

## FAQs

**Q: What's the maximum size I should allocate with stackalloc?**  
A: There's no hard rule, but a common guideline is to keep allocations under 1KB. For larger allocations, consider using the heap with `new` instead, or use the conditional pattern: `size <= threshold ? stackalloc byte[size] : new byte[size]`.

**Q: Can I return stack-allocated memory from a method?**  
A: You can return a `Span<T>` or `ReadOnlySpan<T>` that references stack-allocated memory, but this is dangerous as the memory will be invalid after the method returns. The C# compiler will usually prevent this with lifetime checking.

**Q: Is stackalloc thread-safe?**  
A: Yes, in the sense that each thread has its own stack, so stack allocations in different threads don't interfere with each other. However, sharing a reference to stack-allocated memory between threads can be dangerous if the allocating method returns while another thread is still using the memory.

**Q: When was stackalloc introduced in C#?**  
A: The `stackalloc` expression has been part of C# since version 1.0, but it required unsafe code until C# 7.2, which introduced the ability to use it with `Span<T>` in safe code.

**Q: How does stackalloc compare to using a fixed-size buffer in a struct?**  
A: Fixed-size buffers (`fixed byte buffer[100]`) are similar but have different use cases. Fixed buffers are part of a struct's memory layout and have the same lifetime as the struct, while `stackalloc` creates temporary buffers with method-level lifetime.

**Q: Can I use stackalloc in async methods?**  
A: It's generally not recommended. The memory allocated with `stackalloc` is tied to the physical stack frame of the method, but async methods use logical stack frames that can span multiple physical frames. This can lead to unexpected behavior.

**Q: Does stackalloc work with all types?**  
A: No, in the expression `stackalloc T[E]`, T must be an unmanaged type (like int, byte, or a struct containing only unmanaged types). You cannot use `stackalloc` with reference types like string or class instances.