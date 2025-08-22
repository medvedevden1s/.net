# The using statement - ensure the correct use of disposable objects

### Introduction

The `using` statement ensures that **disposable objects** are properly cleaned up. It automatically calls the `Dispose()` method of an object when execution leaves the `using` block—even if an exception occurs. This makes it ideal for handling resources such as **files, streams, database connections, and network sockets**.

***

### Basic Using Statement

```csharp
var numbers = new List<int>();
using (StreamReader reader = File.OpenText("numbers.txt"))
{
    string? line;
    while ((line = reader.ReadLine()) is not null)
    {
        if (int.TryParse(line, out int number))
        {
            numbers.Add(number);
        }
    }
} // reader.Dispose() is called automatically here
```

The file is **closed safely** after reading, even if an exception happens inside the loop.

***

### Async Disposable Objects

Some resources implement **`IAsyncDisposable`**, requiring asynchronous cleanup. Use **`await using`**:

```csharp
await using (var resource = new AsyncDisposableExample())
{
    // Use resource asynchronously
} // await resource.DisposeAsync() is called
```

***

### Using Declarations

Since C# 8, you can use a **using declaration** without braces. The resource is disposed at the **end of the scope** (e.g., end of method).

```csharp
static IEnumerable<int> LoadNumbers(string filePath)
{
    using StreamReader reader = File.OpenText(filePath);

    var numbers = new List<int>();
    string? line;
    while ((line = reader.ReadLine()) is not null)
    {
        if (int.TryParse(line, out int number))
            numbers.Add(number);
    }
    return numbers; // reader.Dispose() happens here
}
```

***

### Multiple Resources

You can declare multiple disposable objects in one `using` statement. They are disposed in **reverse order**.

```csharp
using (StreamReader numbersFile = File.OpenText("numbers.txt"),
                   wordsFile = File.OpenText("words.txt"))
{
  // Process both files
} // wordsFile.Dispose() → numbersFile.Dispose()
```

***

### Using With Ref Structs

You can also use `using` with **ref structs** if they implement the disposable pattern (a `Dispose()` method).

***

### Using an Existing Variable

You can pass an already created instance into a `using` block:

```csharp
StreamReader reader = File.OpenText("data.txt");

using (reader)
{
    Console.WriteLine(reader.ReadLine());
}
// At this point reader.Dispose() is called, but 'reader' is still in scope
```

{% hint style="warning" %}
After the block, the variable is **disposed but still accessible** in scope.\
Accessing it will throw an `ObjectDisposedException`.\
Prefer declaring the disposable inside the `using` block or with a using declaration.
{% endhint %}

***

### Restrictions

* Variables declared in `using` are **readonly**.
  * You can’t reassign them.
  * You can’t pass them as `ref` or `out`.

***

### Key Points

* `using` guarantees `Dispose()` is called, even on exceptions.
* Use **`await using`** for `IAsyncDisposable`.
* Prefer **using declarations** for cleaner code.
* Multiple disposables can be declared in one `using`.
* Disposables declared outside and passed into `using` remain in scope but are already disposed (⚠ risky).

***

### FAQs

**Q: Why use `using` if I can call `Dispose()` manually?**\
Because `using` ensures disposal **even if exceptions occur**, reducing error risk.

**Q: What’s the difference between `using` and `using declaration`?**

* `using ( ... ) { }` → disposal at block end.
* `using var ...;` → disposal at end of the enclosing scope (method, loop, etc.).

**Q: Can I use `await` inside a `using`?**\
Yes, but for async disposal you must use `await using`.

**Q: Why is multiple declaration disposal order reversed?**\
To safely release resources in the opposite order they were acquired (like stack unwinding).

**Q: What happens if I use a disposed object?**\
You’ll get an `ObjectDisposedException`. Always avoid using a disposed instance.
