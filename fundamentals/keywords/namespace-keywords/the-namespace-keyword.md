# Namespace keyword

## Introduction

The `namespace` keyword is a logical scope for organizing related types and preventing naming conflicts. Namespaces help structure your code into meaningful segments, making it easier to manage, maintain, and use across different files and assemblies.

### Using Namespaces

Namespaces can contain classes, interfaces, structs, enums, delegates, and nested namespaces.

```csharp
namespace MyApplication
{
    class User { }

    interface IUser { }

    struct UserInfo { }

    enum UserType { Admin, Regular }

    delegate void NotifyUser(string message);

    namespace Utilities
    {
        class Logger { }
    }
}
```

### File-Scoped Namespaces

Introduced to simplify the syntax when a file contains a single namespace declaration, file-scoped namespaces apply to the entire file without braces.

```csharp
namespace MyApplication;

class User { }

interface IUser { }

struct UserInfo { }

enum UserType { Admin, Regular }

delegate void NotifyUser(string message);
```

File-scoped namespaces can use other namesapases, but  cannot contain additional namespace declarations, either nested or sequential.

```csharp
// Globally accessible
using System;

namespace MyApplication;

// Scoped within MyApplication
using System.Text;

public class User
{
    public void Log()
    {
        Console.WriteLine("User logged."); // System is globally accessible
        StringBuilder sb = new StringBuilder(); // System.Text scoped here
    }
}

// Not allowed
namespace AnotherNamespace; // Error

// Nested namespace not allowed
namespace NestedNamespace // Error
{
    class Logger { }
}
```

### Nested Namespaces

Namespaces can be nested within other namespaces using the traditional syntax.

```csharp
namespace MyApplication
{
    namespace Utilities
    {
        class Logger
        {
            public static void Log(string message)
            {
                Console.WriteLine(message);
            }
        }
    }
}

// Calling a method from nested namespace
class Program
{
    static void Main()
    {
        MyApplication.Utilities.Logger.Log("Nested namespace call");
    }
}
// Output: Nested namespace call
```

### Multiple namespace declarations

Namespaces can be split across multiple files, allowing logical grouping and modular code organization.

File 1:

```csharp
namespace MyCompany.Project
{
    class User { }
}
```

File 2:

```csharp
namespace MyCompany.Project
{
    class UserManager { }
}
```

### Global Namespace

Every C# file implicitly has a global namespace. Types defined outside of any explicit namespace declaration reside in this global namespace. This global namespace allows easy access to types across different namespaces without needing qualification.

```csharp
// Global namespace
class GlobalClass { }

namespace MyNamespace
{
    class NamespacedClass
    {
        void UseGlobalClass()
        {
            GlobalClass globalInstance = new GlobalClass(); // Direct access to global namespace type
        }
    }
}
```

You can explicitly use the global namespace keyword (`global::`) to resolve naming conflicts or clarify references.

```csharp
namespace MyNamespace
{
    class Console { }

    class Program
    {
        static void Main()
        {
            global::GlobalClass instance = new global::GlobalClass();
            instance.SomeMethod();
        }
    }
}
```

### Key points

* **Traditional namespaces**: Require curly braces `{}`.
* **File-scoped namespaces**: Simplify syntax for single-namespace files.
* **Using directives**: Position affects their scope.
* **Global namespace**: Implicit in every file.

### FAQs

#### Why use namespaces?

Namespaces organize code, prevent naming conflicts, and create globally unique identifiers.

#### Can I have multiple namespaces in one file?

Yes, using traditional syntax. With file-scoped namespaces, you can only have one.

#### Can namespaces be nested?

Yes, namespaces can nest within other namespaces using traditional syntax but not with file-scoped syntax.
