# What is a Delegate in C#?

### A delegate is a type that represents a reference to a method with a specific signature.

You can think of it like a **"function pointer" with type safety** — it lets you **store methods in variables**, pass them around, and invoke them dynamically.

***

### 📦 Why Do Delegates Exist?

Delegates let you:

* Pass methods as parameters (callbacks)
* Define plug-in behaviors (strategy pattern)
* Implement **events**
* Enable LINQ / functional-style programming
* Create flexible APIs without inheritance

***

### 🔧 Syntax & Usage

#### 1. Define a delegate type:

```csharp
public delegate void LogHandler(string message);
```

#### 2. Assign a method to it:

```csharp
void LogToConsole(string msg) => Console.WriteLine(msg);

LogHandler logger = LogToConsole;
logger("Hello!"); // prints "Hello!"🔥 Example: Passing Method as Parameter
```

***

```csharp
void ProcessData(int value, LogHandler logger)
{
    logger($"Processing value: {value}");
}
```

```csharp
ProcessData(42, LogToConsole);
```

***

### ✅ Modern Shortcut: `Action` and `Func`

Instead of declaring custom delegates, C# provides built-in generic delegates:

| Type               | Description                   | Example                                    |
| ------------------ | ----------------------------- | ------------------------------------------ |
| `Action<T>`        | A method that returns `void`  | `Action<string> log = Console.WriteLine;`  |
| `Func<T, TResult>` | A method that returns a value | `Func<int, string> f = x => x.ToString();` |

***

### 🧩 Delegate vs Method Group vs Lambda

```csharp
csharp
CopyEdit
delegate void Printer(string msg);

Printer p1 = PrintMessage;        // Method group
Printer p2 = (string m) => PrintMessage(m); // Lambda

void PrintMessage(string m) => Console.WriteLine(m);

```

***

### 🧠 Delegates Are Used In:

| Feature   | How It Uses Delegates                                          |
| --------- | -------------------------------------------------------------- |
| Events    | Events are delegate-based (`event EventHandler`)               |
| LINQ      | Uses `Func<T, TResult>` delegates for filters/projections      |
| Callbacks | You pass a delegate to be called later                         |
| Multicast | Delegates can point to **multiple methods** (invoked in order) |

***

### 🎤 Interview Soundbite

> “A delegate in C# is a type-safe function pointer. It lets me store a reference to a method with a specific signature and invoke it dynamically. Delegates are the foundation of events, callbacks, and LINQ expressions. In modern code, I mostly use Action and Func instead of declaring custom delegate types.”
