# Page 1

## Introduction to C\#

C# (pronounced "C-Sharp") is a modern, versatile, object-oriented programming language developed by Microsoft. It runs on the .NET platform and is designed to be simple, powerful, and scalable—making it ideal for developing everything from desktop applications to games and cloud services.

C# is standardized by both ECMA and ISO and was originally developed by Anders Hejlsberg during the development of the .NET Framework.

\
**Designed for CLI**: C# is built to work with the Common Language Infrastructure (CLI), which allows different high-level languages to run on multiple platforms using a common runtime.\


### Why Use C#?

* Modern, structured, and easy to learn
* Fully object-oriented
* Cross-platform with .NET Core and .NET 5/6+
* Integrated into a rich development ecosystem (.NET, Visual Studio, Azure)
* Used in web, desktop, mobile, cloud, game, and IoT development

***

### Key Features of C\#

#### Object-Oriented

Supports encapsulation, inheritance, polymorphism, and abstraction.

#### Type-Safe

Prevents unsafe memory operations and enforces type safety.

#### Cross-Platform

Runs on Windows, Linux, and macOS via .NET Core/.NET.

#### High Performance

Compiles to Intermediate Language (IL) and runs on the Common Language Runtime (CLR).

#### Extensive Standard Library

Built-in support for UI, database, I/O, networking, XML, and more.

#### Modern Language Features

* LINQ
* async/await
* Lambda expressions
* Pattern matching
* Records and top-level statements (C# 9+)

***

### C# vs Other Languages

## Table

| Feature           | C#             | Java           | Python         | C++       |
| ----------------- | -------------- | -------------- | -------------- | --------- |
| Object-Oriented   | ✅              | ✅              | ✅              | ✅         |
| Memory Management | Automatic (GC) | Automatic (GC) | Automatic (GC) | Manual    |
| Performance       | High           | Medium         | Low            | Very High |
| Cross-Platform    | ✅ (.NET Core)  | ✅              | ✅              | ✅         |
| Game Development  | ✅ (Unity)      | ❌              | ❌              | ✅         |

***

### Strong Programming Features

* Automatic garbage collection
* Delegates and events
* LINQ and lambda expressions
* Versioned assemblies
* Multithreading
* Generics and indexers
* Properties and events
* Conditional compilation

***

### How Does C# Work?

1. **Write Code**: Using Visual Studio, VS Code, or any editor
2. **Compile Code**: Translates C# to Intermediate Language (IL)
3. **Run Code**: IL is executed by the CLR

***

### First C# Program

\{% code title="HelloWorld.cs" lineNumbers="true" %\}

```csharp
csharpCopyEditusing System;

class HelloWorld
{
    static void Main()
    {
        Console.WriteLine("Hello, World!");
    }
}
```

\{% endcode %\}

> Output: `Hello, World!`

***

### Where Is C# Used?

#### 1. Desktop Applications

* WinForms and WPF for Windows GUI apps

```csharp
csharpCopyEditusing System.Windows.Forms;

class Program
{
    static void Main()
    {
        MessageBox.Show("Hello, Windows Forms!");
    }
}
```

#### 2. Web Development

* ASP.NET Core for backend APIs and full-stack apps

```csharp
csharpCopyEdit[ApiController]
[Route("[controller]")]
public class HelloController : ControllerBase
{
    [HttpGet]
    public string Get() => "Hello from ASP.NET Core API!";
}
```

#### 3. Game Development

* Primary language for Unity game engine

```csharp
csharpCopyEditusing UnityEngine;

public class HelloUnity : MonoBehaviour
{
    void Start()
    {
        Debug.Log("Hello from Unity Game!");
    }
}
```

#### 4. Mobile Development

* Xamarin for cross-platform Android/iOS apps

```csharp
csharpCopyEditpublic class App : Application
{
    public App()
    {
        MainPage = new ContentPage
        {
            Content = new Label { Text = "Hello Xamarin!" }
        };
    }
}
```

#### 5. Cloud & IoT

* Azure SDKs for serverless functions, storage, etc.

```csharp
csharpCopyEditusing Azure.Storage.Blobs;

class Program
{
    static void Main()
    {
        var client = new BlobServiceClient("your-connection-string");
        Console.WriteLine("Connected to Azure Storage!");
    }
}
```

***

### Frequently Asked Questions

> **Is C# free to use?**\
> Yes. C# and .NET are open-source and free.

> **Can I build games with C#?**\
> Absolutely! C# is the main language used in Unity.

> **C# vs Python?**\
> C# is faster and more structured; Python is more flexible and easier to write for quick scripts.

> **Can C# build mobile apps?**\
> Yes, with Xamarin or .NET MAUI for cross-platform development.

> **What can I build with C#?**\
> Desktop software, web services, mobile apps, cloud solutions, IoT projects, and more.

***

### Final Thoughts

C# is one of the most in-demand, productive, and versatile programming languages available today. Whether you're building enterprise software, games, or mobile apps, C# provides a modern, structured, and scalable foundation.
