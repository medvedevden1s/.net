# Hello World: Your First C# Program

### Your First C# Program

The **"Hello World"** example is traditionally the simplest way to introduce a new programming language. Let's dive into your first C# program, showcasing how straightforward and powerful C# is.

***

#### Hello World in C# (Top-Level Statements)

Modern C# allows you to write minimalistic and easy-to-read code using **top-level statements**, reducing boilerplate code significantly. Here's the simplest way to print `"Hello, World!"` in C#:

```csharp
CopyEditConsole.WriteLine("Hello, World!");
```

**Understanding the Code:**

* **`Console`** is a class provided by the .NET libraries, located in the **System** namespace.
* **`WriteLine`** is a method that prints text to the console and moves to a new line.

This format is simple, clean, and directly executable without explicitly declaring classes or methods.

***

#### Traditional C# Program Structure (Main Method)

Alternatively, you can write a more explicit and traditional structure, clearly showing how C# organizes programs using classes and methods:

```csharp
csharpCopyEditusing System;

class Program
{
    static void Main(string[] args)
    {
        Console.WriteLine("Hello, World!");
    }
}
```

**Breaking Down the Traditional Structure:**

* **`using System;`**: Imports the System namespace, containing fundamental classes commonly used in C#.
* **`class Program`**: Defines a class named `Program` which encapsulates methods and properties.
* **`static void Main(string[] args)`**: The `Main` method is the entry point for a C# program. Execution starts here.
  * **`static`** indicates the method can be called without creating an instance of the class.
  * **`void`** means the method does not return a value.
  * **`string[] args`** allows the program to accept command-line arguments.

***

#### 📌 More about the Main Method:

The `Main` method is essential because it defines the entry point for your C# program.

| Feature                | Description                                                                               |
| ---------------------- | ----------------------------------------------------------------------------------------- |
| **Static**             | Must be declared static so it can be called by runtime without creating a class instance. |
| **Location**           | Must reside within a class or struct.                                                     |
| **Access Modifiers**   | Typically `private` by default, but can explicitly be `public`, `internal`, etc.          |
| **Return Types**       | Can return `void`, `int`, `Task`, or `Task<int>`.                                         |
| **Async Support**      | Can be `async` only if returning `Task` or `Task<int>`.                                   |
| **Arguments (`args`)** | Optional parameter (`string[] args`) to accept command-line inputs.                       |

***

#### Command-Line Arguments Example

You can easily access command-line arguments. Here’s a quick example:

```csharp
csharpCopyEditusing System;

class Program
{
    static void Main(string[] args)
    {
        Console.WriteLine("Arguments passed:");
        foreach (var arg in args)
        {
            Console.WriteLine(arg);
        }
    }
}
```

Run your program with:

```shell
dotnet run -- arg1 arg2 arg3
```

This prints:

```sh
Arguments passed:
arg1
arg2
arg3
```

***

#### ⚖️ Modern vs Traditional: Quick Comparison Table

| Approach                 | Ideal Use-Cases                          | Complexity    | Explicit Entry Point          |
| ------------------------ | ---------------------------------------- | ------------- | ----------------------------- |
| **Top-level Statements** | Quick scripts, learning, small utilities | Very Simple   | Implicit (Compiler-generated) |
| **Traditional (Main)**   | Large projects, structured applications  | More Detailed | Explicit                      |

***

### 📚 FAQ (Frequently Asked Questions)

Here are some common questions about writing your first C# programs:

#### ❓ **Why must the `Main` method be static?**

**Answer:**\
Because the `Main` method serves as the entry point for the application, the runtime must invoke it without creating an object instance. A static method belongs directly to the class itself and can thus be called without instantiating the class.

#### ❓ **Can a C# program have multiple `Main` methods?**

**Answer:**\
You can define multiple methods named `Main` in different classes or structs, but your program can only have **one entry point**. If multiple `Main` methods exist, you must explicitly specify which one serves as the entry point during compilation (e.g., using the `StartupObject` compiler option).

#### ❓ **Can the `Main` method be asynchronous?**

**Answer:**\
Yes. If you need asynchronous operations at startup, you can declare your `Main` method with the `async` keyword, but **only** if its return type is `Task` or `Task<int>`:

```csharp
csharpCopyEditstatic async Task Main(string[] args)
{
    await SomeAsyncMethod();
}
```

#### ❓ **Is the `args` parameter always required in the `Main` method?**

**Answer:**\
No. The `args` parameter (`string[] args`) is optional. Include it only if you want to process command-line arguments.
