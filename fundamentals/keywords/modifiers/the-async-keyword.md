# The async keyword

### Introduction

The `async` keyword marks methods or expressions that perform asynchronous operations. Asynchronous programming allows your application to perform tasks without waiting or blocking other tasks, keeping your application responsive.

### Why Use the `async` Keyword?

Imagine you're developing a web application that fetches data from APIs. Without asynchronous programming, your application must wait (block) until each request completes, slowing down the user experience. By using `async`, your application can clearly handle multiple requests efficiently, improving performance and responsiveness.

***

### How Does Async Work?

An `async` method initially runs synchronously—just like a regular method—until it encounters its first `await` statement. At that point:

1. The method pauses at the await point.
2. Control immediately returns to the calling method.
3. The awaited task runs asynchronously (in the background).
4. Once complete, the method resumes execution from where it paused.

Let's clarify this behavior clearly with detailed logging:

#### Practical Async Example (Detailed Logging)

```csharp
csharpCopyEditusing System;
using System.Net.Http;
using System.Threading.Tasks;

class Program
{
    static async Task Main()
    {
        Console.WriteLine("Program started.");

        var task = GetContentLengthAsync("https://dotnet.microsoft.com/");

        Console.WriteLine("Doing other work in Main...");

        int length = await task;

        Console.WriteLine($"Downloaded content length: {length:N0} characters.");
        Console.WriteLine("Program finished.");
    }

    static async Task<int> GetContentLengthAsync(string url)
    {
        Console.WriteLine("Starting download in GetContentLengthAsync...");
        
        using HttpClient client = new HttpClient();
        
        var content = await client.GetStringAsync(url);  // Method pauses here
        
        Console.WriteLine("Finished downloading in GetContentLengthAsync.");

        return content.Length;
    }
}
```

**Expected Output:**

```bash
Program started.
Starting download in GetContentLengthAsync...
Doing other work in Main...
Finished downloading in GetContentLengthAsync.
Downloaded content length: 58,312 characters.
Program finished.
```

{% hint style="info" %}
Notice how after reaching the `await`, the method pauses and returns control immediately to the caller (Main method). When the awaited task completes, execution continues clearly from the exact point it paused.
{% endhint %}

***

### Async Return Types

Async methods can return these types:

| Return Type       | Explanation                                  | Example                                |
| ----------------- | -------------------------------------------- | -------------------------------------- |
| `Task`            | Awaitable method with no result (void-like). | `async Task SaveDataAsync()`           |
| `Task<T>`         | Awaitable method returning a value (`T`).    | `async Task<int> GetNumberAsync()`     |
| `void`            | Event handlers only; not awaitable.          | `async void Button_Click(...)`         |
| Custom awaitables | Any type implementing `GetAwaiter()`.        | `async ValueTask<int> GetValueAsync()` |

***

### Important Restrictions Explained Clearly

* **Cannot have `in`, `ref`, or `out` parameters**:\
  Async methods return immediately upon encountering an `await`, making it impossible to ensure changes to parameters (like `ref` or `out`) remain consistent or thread-safe after resumption.
* **Cannot return by reference (`ref return`)**:\
  Reference returns must clearly point to a stable memory location. Async methods, however, pause and resume execution asynchronously, making memory locations potentially invalid or unstable.
* **Must include at least one `await` statement**:\
  Without `await`, your method runs synchronously, defeating the purpose of `async`. The compiler warns you (`CS4014`) because an async method without `await` might indicate you misunderstood or mistakenly omitted necessary asynchronous logic.

***

### Key Points:

* `async` clearly indicates methods that perform asynchronous operations.
* Async methods pause at `await`, returning immediately to callers, allowing the program to remain responsive.
* Supported return types: `Task`, `Task<T>`, `void` (for event handlers only), and custom awaitables like `ValueTask<T>`.
* Avoid `async void` methods for all but event handlers due to difficulty in error handling.
* Restrictions clearly protect async methods from unsafe or unpredictable behaviors (no `ref`, `out`, or `ref return`).

***

### FAQs

**Why exactly can't async methods have `ref` or `out` parameters?**\
Because async methods pause and resume execution, the state of these parameters could become unpredictable or unstable across asynchronous boundaries. This could cause unexpected behavior or errors.

**Why can't I use `ref return` in an async method?**\
Because async methods resume after awaiting a task, the memory location you're referencing could be invalidated or changed, resulting in unsafe or undefined behavior.

**Can async methods run in parallel?**\
Async methods enable concurrency (overlapping tasks), but not automatically parallelism (multiple simultaneous threads). For explicit parallel execution, use constructs like `Task.Run` or `Parallel`.

**How do exceptions work with async methods?**\
Async methods propagate exceptions clearly through returned tasks. When you await the task, exceptions become re-thrown and handled using normal try-catch blocks around the await statement.

**Can constructors be async?**\
No, constructors can't be async because constructors must clearly return a fully initialized instance immediately. For async initialization, consider clearly using factory methods or explicit initialization methods after construction.
