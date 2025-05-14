# What are Action and Func in C#?

## 📛 Definition

***

* **`Action`** and **`Func`** are **built-in delegate types** in the .NET framework.
* They are part of **System** namespace and provide a **concise way to represent methods as data** (a key concept in functional programming).🧠 Why do they exist?

***

To replace verbose custom `delegate` definitions like this:

```csharp
public delegate int MyOperation(int a, int b);With a simpler, reusable syntax:
```

```csharp
Func<int, int, int> add = (a, b) => a + b;
```

They improve **readability**, reduce **boilerplate**, and enable **passing methods as values** — especially useful in LINQ, events, callbacks, and middleware.

***

### 🔧 1. `Action` — for methods that **return `void`**

#### ✅ Signature:

```csharp
public delegate void Action();             // no parameters
public delegate void Action<T1>();         // 1 param
public delegate void Action<T1, T2>();     // 2 params
// up to Action<T1, ..., T16>
```

#### 🔥 Example:

```csharp
Action greet = () => Console.WriteLine("Hello!");
greet(); // prints Hello!

Action<string> log = msg => Console.WriteLine("Log: " + msg);
log("Something happened");
```

***

### 🔧 2. `Func` — for methods that **return a value**

#### ✅ Signature:

```csharp
public delegate TResult Func<TResult>(); // no parameters
public delegate TResult Func<T, TResult>();
public delegate TResult Func<T1, T2, TResult>();
// up to Func<T1, ..., T16, TResult>
```

> ✅ The last type parameter is always the return type

***

#### 🔥 Example:

```csharp
Func<int, int, int> add = (a, b) => a + b;
int sum = add(2, 3); // 5

Func<string> getTime = () => DateTime.Now.ToShortTimeString();
Console.WriteLine(getTime());
```

***

### 🧩 Key Differences

| Feature     | `Action`                   | `Func`                                     |
| ----------- | -------------------------- | ------------------------------------------ |
| Return type | `void`                     | A value (`TResult`)                        |
| Parameters  | 0 to 16                    | 0 to 16 (last one is return type)          |
| Use case    | Logging, callbacks, events | Computations, projections, transformations |

***

### ✅ Real-World Use Cases

| Use Case                | Delegate Type        | Example                                        |
| ----------------------- | -------------------- | ---------------------------------------------- |
| Logging                 | `Action<string>`     | `logger("something happened")`                 |
| Validation              | `Func<string, bool>` | `name => !string.IsNullOrEmpty(name)`          |
| LINQ `Select`           | `Func<T, TResult>`   | `.Select(x => x.Id)`                           |
| Event handler (no args) | `Action`             | `event Action OnSave`                          |
| Timer callback          | `Action<object>`     | `Timer t = new Timer(state => ..., null, ...)` |

***

### ❗ When NOT to use `Action` / `Func`

| Use Case                                    | Use instead                       |
| ------------------------------------------- | --------------------------------- |
| You need `ref`, `out`, or `params`          | Custom `delegate`                 |
| You want to name a delegate for readability | Custom `delegate`                 |
| Public API contracts                        | Custom-named delegate for clarity |

***

### 🎤 Interview Soundbite

> “Action and Func are built-in delegate types that let me pass methods as parameters in a type-safe, reusable way. Action is for void-returning methods, while Func is for methods that return a value. I use them extensively in LINQ, event callbacks, and high-order functions. They make code cleaner and avoid boilerplate delegate declarations.”

***

### 📌 Summary Table

| Delegate | Parameters | Return Type | Example                                   |
| -------- | ---------- | ----------- | ----------------------------------------- |
| `Action` | 0–16       | `void`      | `Action<string> log = Console.WriteLine;` |
| `Func`   | 0–16       | Last type   | `Func<int, int, int> add = (a,b)=>a+b;`   |

***

## ✅ C# `Action` and `Func` Cheat Sheet

***

### 🔧 `Action<T>` — Void-returning delegates

| Variant           | Signature                 | Example                                                               |
| ----------------- | ------------------------- | --------------------------------------------------------------------- |
| `Action`          | `void ()`                 | `Action a = () => Console.WriteLine();`                               |
| `Action<T>`       | `void (T1)`               | `Action<string> log = msg => Console.WriteLine(msg);`                 |
| `Action<T1,T2>`   | `void (T1, T2)`           | `Action<int, string> print = (i,s) => Console.WriteLine($"{i}:{s}");` |
| `Action<T1..T16>` | Up to 16 input parameters | Used for large callbacks or handlers                                  |

***

### 🔧 `Func<T>` — Value-returning delegates

| Variant                  | Signature                           | Example                                                 |
| ------------------------ | ----------------------------------- | ------------------------------------------------------- |
| `Func<TResult>`          | `TResult ()`                        | `Func<string> getTime = () => DateTime.Now.ToString();` |
| `Func<T, TResult>`       | `TResult (T1)`                      | `Func<int, string> toStr = x => x.ToString();`          |
| `Func<T1,T2, TResult>`   | `TResult (T1, T2)`                  | `Func<int, int, int> add = (a,b) => a + b;`             |
| `Func<T1..T16, TResult>` | Supports up to 16 inputs + 1 return | `Func<int,int,int,int> sum3 = (a,b,c) => a+b+c;`        |

***

### 🧠 Quick Usage Scenarios

| Scenario                 | Use This               | Example                               |
| ------------------------ | ---------------------- | ------------------------------------- |
| Logging messages         | `Action<string>`       | `log("error")`                        |
| File processing pipeline | `Func<string, string>` | `path => File.ReadAllText(path)`      |
| Filtering list items     | `Func<T, bool>`        | `.Where(x => x.IsActive)`             |
| Side-effect actions      | `Action<T>`            | `.ForEach(x => Console.WriteLine(x))` |
| Event-like behavior      | `Action`               | `event Action OnCompleted;`           |

***

### 🧭 When to use `Predicate<T>` or `Comparison<T>`?

| Delegate Type   | Description             | Example                        |
| --------------- | ----------------------- | ------------------------------ |
| `Predicate<T>`  | Same as `Func<T, bool>` | Used in `.Find(Predicate<T>)`  |
| `Comparison<T>` | `int Compare(T x, T y)` | Used in `.Sort(Comparison<T>)` |
