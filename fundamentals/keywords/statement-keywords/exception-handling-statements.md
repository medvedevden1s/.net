# Exception-handling statements

### Introduction

Errors are inevitable in programs, and exceptions are the way to handle them gracefully. Instead of crashing the application, you can **throw** an exception when something goes wrong, and **try** to catch and handle it. This ensures your program remains stable and predictable.

***

### The Throw Statement

The `throw` keyword is used to create and signal an error condition.

```csharp
if (shapeAmount <= 0)
{
    throw new ArgumentOutOfRangeException(nameof(shapeAmount), "Amount of shapes must be positive.");
}
```

📌 Key facts:

* `throw` must be followed by an exception object (`System.Exception` or derived).
* You can use built-in exceptions like `ArgumentOutOfRangeException` or create custom ones.
* Inside a `catch` block, you can use `throw;` to re-throw the original exception.

```csharp
try
{
    ProcessShapes(shapeAmount);
}
catch (Exception e)
{
    Console.WriteLine(e);

    // throw;   // ✅ Keeps original stack trace:
    // System.ArgumentOutOfRangeException: ...
    //    at ProcessShapes(Int32 shapeAmount)
    //    at Main()

    // throw e; // ❌ Resets stack trace:
    // System.ArgumentOutOfRangeException: ...
    //    at Main()   <-- original source (ProcessShapes) is lost
}

```

{% hint style="info" %}
`throw;` preserves the stack trace, while `throw e;` resets it. Always prefer `throw;` when re-throwing.
{% endhint %}

***

### Throw Expressions

From C# 7 onward, you can use `throw` inside expressions:

```csharp
// Conditional operator
string first = args.Length >= 1 
    ? args[0]
    : throw new ArgumentException("Please supply at least one argument.");

// Null-coalescing operator
public string Name
{
    get => name;
    set => name = value ?? throw new ArgumentNullException(nameof(value), "Name cannot be null");
}

// Expression-bodied method
DateTime ToDateTime(IFormatProvider provider) => 
    throw new InvalidCastException("Conversion not supported.");
```

{% hint style="info" %}
Throw expressions make code more compact and expressive.
{% endhint %}

***

### The Try Statement

A `try` block lets you execute code safely. If an exception occurs, execution jumps to the corresponding `catch` block. Optionally, a `finally` block runs cleanup code.

#### Try-Catch

```csharp
try
{
    var result = Process(-3, 4);
    Console.WriteLine($"Processing succeeded: {result}");
}
catch (ArgumentException e)
{
    Console.WriteLine($"Processing failed: {e.Message}");
}
```

* Multiple `catch` blocks are allowed.
* They are checked top-to-bottom; only one runs.
* A `catch` without a type matches **any exception** and must be last.

***

#### Try-Finally

The `finally` block **always executes** (except when the process terminates immediately).

```csharp
public async Task HandleRequest(int itemId, CancellationToken ct)
{
    Busy = true;
    try
    {
        await ProcessAsync(itemId, ct);
    }
    finally
    {
        Busy = false; // cleanup
    }
}
```

Use it for cleanup tasks like releasing files, closing connections, or resetting state.

***

#### Try-Catch-Finally

You can combine them:

```csharp
public async Task ProcessRequest(int itemId, CancellationToken ct)
{
    Busy = true;

    try
    {
        await ProcessAsync(itemId, ct);
    }
    catch (Exception e) when (e is not OperationCanceledException)
    {
        LogError(e, $"Failed to process request for {itemId}");
        throw;
    }
    finally
    {
        Busy = false;
    }
}
```

***

### Exception Filters (`when`)

An exception filter adds conditions to your `catch`:

```csharp
try
{
    var result = Process(-3, 4);
}
catch (Exception e) when (e is ArgumentException || e is DivideByZeroException)
{
    Console.WriteLine($"Handled: {e.Message}");
}
```

📌 Advantages:

* Stack isn’t unwound until filter matches → better debugging.
* Cleaner than nested `if` checks.
* Useful for logging without catching everything.

```csharp
catch (IOException ex) when (ex.Message.Contains("not found"))
{
    Console.WriteLine("File not found.");
}
```

***

### Exceptions in Async and Iterators

* In **async methods**, exceptions appear when you `await` the task.
* In **iterators**, exceptions are thrown when moving to the next element.

```csharp
try
{
    int result = await ProcessAsync(-1);
}
catch (ArgumentException e)
{
    Console.WriteLine($"Failed: {e.Message}");
}
```

***

### Key Points

* `throw` creates and signals exceptions.
* Use `throw;` inside a `catch` to preserve stack trace.
* Use `try-catch` to handle errors, `try-finally` for cleanup, or both together.
* Exception filters (`when`) allow conditional handling without losing stack info.
* Async/iterator exceptions propagate only when awaited or iterated.

***

### FAQs

**Why not always use a single `catch(Exception)`?**\
Because it hides specific problems. Always catch the most specific exception possible.

**When should I use `finally`?**\
For cleanup tasks like closing files, releasing resources, or resetting flags.

**Is `throw e;` ever useful?**\
Rarely. Prefer `throw;` to keep debugging info intact.

**Can I log inside an exception filter?**\
Yes, filters are evaluated before stack unwinding, perfect for logging without changing behavior.

**What happens if no `catch` is found?**\
The CLR terminates the current thread, and in most cases the whole program stops.
