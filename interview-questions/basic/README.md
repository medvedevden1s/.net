# Basic

#### **1. What is the difference between .NET and C#?**

**.NET**: A framework consisting of runtime (CLR) and reusable libraries. CLR does the following:

* Compiles code: Converts Intermediate Language (IL) into native machine instructions via Just-In-Time (JIT) compilation at runtime.
* Manages memory: Automatically handles memory allocation/deallocation through Garbage Collection.
* Provides safety: Ensures type safety and security through code verification and sandboxing.
* Exception Handling: Provides mechanisms to handle runtime errors gracefully.
* Thread Management: Supports multithreading and concurrency.
* **C#**: A programming language with specific syntax and semantics, used to write code that runs within the .NET framework.

#### **2. Difference between .NET Framework, .NET Core, and .NET 5.0?**

* **.NET Framework**: Windows-only, legacy framework.
* **.NET Core**: Cross-platform, optimized, modular.
* **.NET 5.0**: Unification of .NET Framework and .NET Core, cross-platform, unified developer experience.

#### **3. What is IL code (Intermediate Language)?**

* Partially compiled .NET code that is later fully compiled by JIT compiler to machine language at runtime.

#### **4. What is JIT (Just-In-Time compiler)?**

* Compiles IL code to native machine language just before execution.

#### **5. Why compile to IL code?**

| 🔧 Feature               | 💡 What It Means (Simple)                                    | 📦 Example You Can Imagine                           |
| ------------------------ | ------------------------------------------------------------ | ---------------------------------------------------- |
| 🔁 Use many languages    | Different languages (C#, F#, VB) can work together           | A team using both C# and F# on the same project      |
| 🌐 Run anywhere          | One build works on Windows, Linux, Mac, ARM, etc.            | You compile on Windows and run the same app on Linux |
| ⚡ Fast at runtime        | JIT makes your app faster by using the best CPU instructions | Your app runs faster on new computers automatically  |
| 🔍 Easy to inspect/debug | You can look into your app while it runs (Reflection)        | Your app can list its own methods or generate code   |
| 🔒 Safe to run           | IL can be checked for safety before running                  | Avoids crashes like accessing invalid memory         |
| 🧠 Very flexible         | Supports both JIT and precompiled (AOT) styles               | You can choose JIT for dev, AOT for small release    |

#### **6. Does .NET support multiple languages?**

* Yes (C#, [VB.NET](http://vb.net), F#, C++/CLI, etc.). All compile to IL code.

#### **7. Explain CLR (Common Language Runtime).**

* Runtime environment executing .NET code, manages memory (garbage collector), security, and compiles IL to machine language.

[what-exactly-is-clr-common-language-runtime.md](what-exactly-is-clr-common-language-runtime.md "mention")

#### **8. Managed vs Unmanaged Code?**

#### 🔵 **Managed Code**

* Code that runs **under the control of the .NET CLR (Common Language Runtime)**.
* The **CLR takes care of memory**, security, exception handling, and garbage collection.

#### 🔴 **Unmanaged Code**

* Code that **runs directly on the OS**, without CLR help.
* You must **manually manage memory** (malloc/free or new/delete).
* If you mess up, you get **memory leaks**, crashes, or unsafe behavior.

![{687384D9-7D12-454C-A26F-DFDD0008B9B6}.png](attachment:5b17b985-c84d-4840-9158-e01548d5b4a6:687384D9-7D12-454C-A26F-DFDD0008B9B6.png)

#### **9. What is Garbage Collector?**

* Background CLR process, frees unused managed memory resources automatically.

[**What is the Garbage Collector (GC) in .NET?**](https://www.notion.so/What-is-the-Garbage-Collector-GC-in-NET-1ea9fdbd081c8098b525f642f67f443b?pvs=21)

#### **10. What is CTS (Common Type System)?**

CTS, or Common Type System, defines the rules for how types are defined and behave in .NET. It allows different languages like C#, F#, and [VB.NET](http://vb.net) to interoperate safely by sharing a unified type system. CTS defines value vs reference types, inheritance rules, method signatures, and how types map to the CLR. It’s the foundation that enables cross-language compatibility in .NET

**Determine Data type differences int in C# and Integer in VB is System.Int32**

[What is CTS (Common Type System)?](https://www.notion.so/What-is-CTS-Common-Type-System-1ec9fdbd081c808f9cd7ca7641a063af?pvs=21)

#### **11. What is CLS (Common Language Specification)?**

* Guidelines ensuring cross-language compatibility within NET.
* It's a guideline: point 1Determine behaviours like case sensitivity in C# var I and var I in C # is ok, and in VB, those will be the same vars

#### **12. Stack vs Heap?**

* **Stack**: Stores primitive/value types, variables & values together.
* **Heap**: Stores objects/reference types, variables on stack point to values on heap.

[Stack vs Heap in .NET](https://www.notion.so/Stack-vs-Heap-in-NET-1ec9fdbd081c8094953fe711fe452fb7?pvs=21)

#### **13. Value Types vs Reference Types?**

* **Value Types**: Stored directly on stack (int, bool).
* **Reference Types**: Stored on heap, stack holds reference/pointer to heap location.

[Value Types and Ref Types](https://www.notion.so/Value-Types-and-Ref-Types-1ec9fdbd081c8073b2b6c2ac2fffa98d?pvs=21)

#### **14. What is Boxing and Unboxing?**

* **Boxing**: Value type → reference type (object), moves data from stack to heap.
* **Unboxing**: Reference type → value type, moves data back from heap to stack.
* **Performance impact**: Frequent boxing/unboxing reduces performance.

```jsx
int x = 42;
object boxed = x;           // 🔁 Boxing: int → object (heap)
int y = (int)boxed;         // 🔁 Unboxing: object → int (stack)
```

#### **15. Implicit vs Explicit Casting?**

* **Implicit**: Automatic conversion (smaller → larger type, int → double).
* **Explicit**: Requires manual specification (larger → smaller type, double → int), risk of data loss.

```jsx
int a = 10;
double b = a;           // ✅ Implicit cast: int → double (safe, automatic)

double c = 9.8;
int d = (int)c;         // ⚠️ Explicit cast: double → int (requires cast, loses .8)

```

#### **16. Array vs ArrayList?**

* **Array**: Fixed size, strongly typed, better performance.
* **ArrayList**: Dynamic size, loosely typed (causes boxing/unboxing), flexible but slower.

### 📊 Array vs ArrayList in C\#

| Feature             | `Array`                           | `ArrayList`                                |
| ------------------- | --------------------------------- | ------------------------------------------ |
| Namespace           | `System`                          | `System.Collections`                       |
| Type Safety         | ✅ Generic (e.g., `int[]`)         | ❌ Stores `object`, requires casting        |
| Performance         | ✅ Faster (no boxing/unboxing)     | ❌ Slower (due to boxing for value types)   |
| Resizable?          | ❌ Fixed size                      | ✅ Dynamically resizable                    |
| Index-based access  | ✅ Yes                             | ✅ Yes                                      |
| Generic Alternative | Use `T[]`                         | Prefer `List<T>` instead                   |
| Suitable For        | High-performance, fixed-size data | Legacy scenarios (non-generic collections) |

```csharp
int[] numbers = new int[3];         // ✅ Strongly typed, fixed size
numbers[0] = 10;

ArrayList list = new ArrayList();  // ❌ Non-generic, holds object
list.Add(10);                      // ⚠️ int is boxed
list.Add("hello");                 // ⚠️ Allows mixed types

int x = (int)list[0];              // ⚠️ Requires explicit cast

```

#### **17. Generic Collections?**

* Combine strengths: Strongly typed (no boxing), dynamic size (e.g., `List<int>`), recommended choice.

#### **18. What are Threads?**

* Allow parallel execution of code (`System.Threading` namespace).

#### **19. Threads vs Tasks (TPL)?**

* **Threads**: Basic parallelism, manual processor management.
* **Tasks (TPL)**: Better CPU utilization, abstraction over threads, support async/await, result handling, cancellation, chaining.

#### **20. How to handle exceptions in C#?**

In C#, we use `try-catch-finally` to handle exceptions. We catch the most specific exceptions first, avoid swallowing exceptions silently, and use `finally` to ensure cleanup code runs. In production, I always log exceptions to monitor and troubleshoot issues

```csharp
try
{
    // Code that may throw an exception
}
catch (FormatException fe)
{
    Console.WriteLine("Format issue");
}
catch (Exception ex)
{
    // Handle the exception
}
finally
{
    // Optional: code that runs no matter what
}

```

#### **21. Purpose of `finally` block?**

* Runs irrespective of exceptions, ideal for cleanup code.

#### **22. Use of `out` keyword?**

* The out keyword in C# allows a method to return multiple values by reference. It’s used heavily in TryParse-style patterns to avoid exceptions and in performance-sensitive code to minimize allocations. The variable must be assigned inside the method before the method exits.

[What is the `out` keyword?](https://www.notion.so/What-is-the-out-keyword-1ed9fdbd081c80ab8ec9def2a5215baf?pvs=21)

#### **23. What are Delegates?**

A delegate in C# is a type-safe function pointer. It lets me store a reference to a method with a specific signature and invoke it dynamically. Delegates are the foundation of events, callbacks, and LINQ expressions. In modern code, I mostly use Action and Func instead of declaring custom delegate types.

[What is a Delegate in C#?](https://www.notion.so/What-is-a-Delegate-in-C-1ed9fdbd081c8076a889c7de12631f3d?pvs=21)

#### **24. What are Events?**

* Encapsulation over delegates, provides Publisher-Subscriber pattern, safer interaction between components.

#### **25. Abstract Class vs Interface?**

* **Abstract class**: Partially implemented base class, common code sharing via inheritance.
* **Interface**: Contract, defines structure (methods/properties), must be fully implemented.

1. **What Are Nullable Flow Annotation Attributes in C#?**

**Nullable Flow Annotation Attributes** are special metadata **attributes** in `.NET` that help the **compiler understand the nullability of parameters, return values, and fields** **based on runtime conditions**.

**27. What are `Action` and `Func` in C#?** Action and Func are built-in delegate types that let me pass methods as parameters in a type-safe, reusable way. Action is for void-returning methods, while Func is for methods that return a value. I use them extensively in LINQ, event callbacks, and high-order functions. They make code cleaner and avoid boilerplate delegate declarations.

[What are `Action` and `Func` in C#?](https://www.notion.so/What-are-Action-and-Func-in-C-1ed9fdbd081c8019bce4ea6e38395504?pvs=21)

***
