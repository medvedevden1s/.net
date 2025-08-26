# The yield statement

### Introduction

The `yield` statement allows you to build **iterators**—methods, operators, or accessors that return elements one at a time instead of all at once. With `yield`, you can create sequences without building intermediate collections, making your code **simpler, memory-efficient, and lazy-evaluated**.

There are two forms of `yield`:

* **`yield return`** – provides the next element in a sequence.
* **`yield break`** – signals that iteration should stop.

***

### Yield Return

```csharp
foreach (int i in ProduceEvenNumbers(9))
{
    Console.Write(i + " ");
}
// Output: 0 2 4 6 8

IEnumerable<int> ProduceEvenNumbers(int upto)
{
    for (int i = 0; i <= upto; i += 2)
    {
        yield return i; // produces one value at a time
    }
}
```

Execution is **deferred**: elements are produced only when the caller requests them.

***

### Yield Break

```csharp
Console.WriteLine(string.Join(" ", TakeWhilePositive(new int[] {2, 3, 4, 5, -1, 3, 4})));
// Output: 2 3 4 5

Console.WriteLine(string.Join(" ", TakeWhilePositive(new int[] {9, 8, 7})));
// Output: 9 8 7

IEnumerable<int> TakeWhilePositive(IEnumerable<int> numbers)
{
    foreach (int n in numbers)
    {
        if (n > 0)
            yield return n;
        else
            yield break; // stop iteration early
    }
}
```

Iteration also stops naturally when the method reaches its end.

***

### Async Iterators

You can also yield asynchronously with **`IAsyncEnumerable<T>`**. Use `await foreach` to consume results:

```csharp
await foreach (int n in GenerateNumbersAsync(5))
{
    Console.Write(n + " ");
}
// Output: 0 2 4 6 8

async IAsyncEnumerable<int> GenerateNumbersAsync(int count)
{
    for (int i = 0; i < count; i++)
    {
        yield return await ProduceNumberAsync(i);
    }
}

async Task<int> ProduceNumberAsync(int seed)
{
    await Task.Delay(1000);
    return 2 * seed;
}
```

***

### Yield With Custom Types

Iterators can return **`IEnumerator<T>`** or **`IEnumerator`**, often used when implementing `GetEnumerator`.

```csharp
public static void Example()
{
    var point = new Point(1, 2, 3);
    foreach (int coordinate in point)
    {
        Console.Write(coordinate + " ");
    }
    // Output: 1 2 3
}

public readonly record struct Point(int X, int Y, int Z)
{
    public IEnumerator<int> GetEnumerator()
    {
        yield return X;
        yield return Y;
        yield return Z;
    }
}
```

***

### Restrictions

* Not allowed in methods with `in`, `ref`, or `out` parameters.
* Not allowed in **lambda expressions** or **anonymous methods**.
* Not allowed in **unsafe blocks** (before C# 13). In C# 13+, allowed in methods containing unsafe blocks but not inside the unsafe block itself.
* Cannot be used in `catch` or `finally` blocks, or in a `try` block with a `catch`.
  * Allowed in a `try` block with only a `finally`.

***

### Execution Flow

Iterators execute **lazily**. The body runs only when iteration begins:

```csharp
var numbers = ProduceEvenNumbers(5);
Console.WriteLine("Caller: about to iterate.");

foreach (int i in numbers)
{
    Console.WriteLine($"Caller: {i}");
}

IEnumerable<int> ProduceEvenNumbers(int upto)
{
    Console.WriteLine("Iterator: start.");
    for (int i = 0; i <= upto; i += 2)
    {
        Console.WriteLine($"Iterator: about to yield {i}");
        yield return i;
        Console.WriteLine($"Iterator: yielded {i}");
    }
    Console.WriteLine("Iterator: end.");
}
```

Output:

```
Caller: about to iterate.
Iterator: start.
Iterator: about to yield 0
Caller: 0
Iterator: yielded 0
Iterator: about to yield 2
Caller: 2
Iterator: yielded 2
Iterator: about to yield 4
Caller: 4
Iterator: yielded 4
Iterator: end.
```

Each `yield return` **pauses execution**, returns a value, and resumes from the same spot on the next iteration.

***

### Key Points

* `yield return` produces the next element in a sequence.
* `yield break` stops iteration early.
* Iterators can return `IEnumerable<T>`, `IEnumerator<T>`, or async equivalents (`IAsyncEnumerable<T>`).
* Execution is **lazy**—elements are generated on demand.
* Restrictions apply (no ref/out params, no lambdas, limited try/catch usage).
* Ideal for **streams of data**, **custom collections**, and **on-demand processing**.

***

### FAQs

**Q: What’s the benefit of `yield` vs returning a collection?**\
`yield` avoids building intermediate collections, saving memory and enabling infinite or very large sequences.

**Q: Is `yield` thread-safe?**\
No, iterators are not inherently thread-safe. Synchronization must be added separately.

**Q: Can I use `yield` with LINQ?**\
Yes. Many LINQ methods rely on `yield` internally for deferred execution.

**Q: Can I mix `yield return` and `return` in the same method?**\
No. If a method uses `yield`, it must return `IEnumerable` / `IEnumerator` (or async equivalents). Regular `return` is only allowed to exit the method, not return values.

**Q: When should I prefer async iterators?**\
When producing values involves **I/O or async operations**, e.g., reading database rows, network packets, or large files.
