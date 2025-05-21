# Your first c# program

### Introduction

The **"Hello World"** example is traditionally the simplest way to introduce a new programming language. Let's dive into your first C# program, showcasing how straightforward and powerful C# is.

***

### Hello World in C# (Top-Level Statements)

Modern C# allows you to write minimalistic and easy-to-read code using **top-level statements**, reducing boilerplate code significantly. Here's the simplest way to print `"Hello, World!"` in C#:

```csharp
Console.WriteLine("Hello, World!");
```

#### **Understanding the Code:**

* **`Console`** is a class provided by the .NET libraries, located in the **System** namespace.
* **`WriteLine`** is a method that prints text to the console and moves to a new line.

This format is simple, clean, and directly executable without explicitly declaring classes or methods.

***

### Traditional C# Program Structure

Alternatively, you can write a more explicit and traditional structure, clearly showing how C# organizes programs using classes and methods:

```csharp
using System;

class Program
{
    static void Main(string[] args)
    {
        Console.WriteLine("Hello, World!");
    }
}
```

**Breaking Down the Traditional Structure:**

* **`using System;`** Imports the System namespace, containing fundamental classes&#x20;
* **`class Program`**: Defines a class named `Program` which encapsulates methods and properties.
* **`static void Main(string[] args)`**: The `Main` method is the entry point for a C# program. Execution starts here.
  * **`static`** indicates the method can be called without creating an instance of the class.
  * **`void`** means the method does not return a value.
  * **`string[] args`** allows the program to accept command-line arguments.

***

#### ⚖️ Modern vs Traditional: Quick Comparison Table

| Approach                 | Ideal Use-Cases                          | Complexity    | Explicit Entry Point          |
| ------------------------ | ---------------------------------------- | ------------- | ----------------------------- |
| **Top-level Statements** | Quick scripts, learning, small utilities | Very Simple   | Implicit (Compiler-generated) |
| **Traditional (Main)**   | Large projects, structured applications  | More Detailed | Explicit                      |
