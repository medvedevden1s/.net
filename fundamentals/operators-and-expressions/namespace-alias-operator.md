## Introduction

The namespace alias operator (`::`) is like a GPS navigator for your code. Just as a GPS helps you navigate to a specific location using exact coordinates, the `::` operator helps you precisely navigate to members within specific namespaces, avoiding any ambiguity along the way.

In the complex world of C# applications, namespaces can overlap and collide. The namespace alias operator provides a clear path through this potential confusion, ensuring you always reach exactly the destination you intended, even when multiple routes with the same name exist.

## How it works

The namespace alias operator `::` allows you to access a member of an aliased namespace. You can use the `::` qualifier only between two identifiers. The left-hand identifier must be one of:

- A namespace alias created with a using alias directive
- An extern alias
- The global alias

```csharp
// Basic syntax
namespaceAlias::member
```

The `::` operator ensures that the left-hand identifier is always interpreted as a namespace alias, even if there's a type or namespace with the same name in the current context.

## Using with namespace aliases

The most common use of the `::` operator is with namespace aliases created using the `using alias` directive:

```csharp
using forwinforms = System.Drawing;
using forwpf = System.Windows;

public class Converters
{
    public static forwpf::Point Convert(forwinforms::Point point) => new forwpf::Point(point.X, point.Y);
}
```

In this example, `forwinforms::Point` refers specifically to `System.Drawing.Point`, while `forwpf::Point` refers to `System.Windows.Point`. Without these aliases and the `::` operator, you would need to use the full namespace names each time, making the code more verbose.

{% hint style="info" %}
Namespace aliases are particularly useful when you need to work with types that have the same name but come from different namespaces.
{% endhint %}

## Using with extern aliases

When you're working with multiple assemblies that have the same namespace hierarchy, you can use extern aliases to distinguish between them:

```csharp
// In your .csproj file or command line:
// <Reference Include="Lib1.dll" Aliases="LibOne"/>
// <Reference Include="Lib2.dll" Aliases="LibTwo"/>

extern alias LibOne;
extern alias LibTwo;

class Program
{
    static void Main()
    {
        // Use types from the first library
        LibOne::MyCompany.MyProduct.MyClass obj1 = new LibOne::MyCompany.MyProduct.MyClass();
        
        // Use types from the second library
        LibTwo::MyCompany.MyProduct.MyClass obj2 = new LibTwo::MyCompany.MyProduct.MyClass();
    }
}
```

This allows you to reference types with identical fully-qualified names from different assemblies.

## The global namespace alias

The `global` keyword serves as a special namespace alias that always refers to the global namespace. This is particularly useful when you have a user-defined namespace that conflicts with a system namespace:

```csharp
namespace MyCompany.MyProduct.System
{
    class Program
    {
        static void Main() => global::System.Console.WriteLine("Using global alias");
    }

    class Console
    {
        string Suggestion => "Consider renaming this class";
    }
}
```

In this example, `global::System.Console` explicitly refers to the .NET `System.Console` class, not the `Console` class defined in the `MyCompany.MyProduct.System` namespace.

{% hint style="warning" %}
The `global` keyword is the global namespace alias only when it's the left-hand identifier of the `::` qualifier. In other contexts, it might be interpreted differently.
{% endhint %}

## Comparison with the dot operator

You can also use the dot operator (`.`) to access members of an aliased namespace. However, the dot operator is ambiguous because it's also used to access type members:

```csharp
using Drawing = System.Drawing;

namespace MyApp
{
    class Drawing
    {
        public static void PrintInfo() => System.Console.WriteLine("This is the Drawing class");
    }

    class Program
    {
        static void Main()
        {
            // Ambiguity: Does this refer to the Drawing class or the System.Drawing namespace?
            // Drawing.PrintInfo(); // This would call the Drawing class method
            
            // No ambiguity: This clearly refers to the System.Drawing namespace
            Drawing::Point point = new Drawing::Point(10, 20);
        }
    }
}
```

The `::` operator resolves this ambiguity by ensuring that the left-hand identifier is always interpreted as a namespace alias.

## Common use cases

### 1. Resolving type name conflicts

When you're working with multiple libraries that define types with the same name, namespace aliases and the `::` operator help avoid conflicts:

```csharp
using WinForms = System.Windows.Forms;
using WPF = System.Windows;

class MyForm
{
    // Use Button from Windows Forms
    private WinForms::Button winButton = new WinForms::Button();
    
    // Use Button from WPF
    private WPF::Controls.Button wpfButton = new WPF::Controls.Button();
}
```

### 2. Accessing the original namespace in extension methods

When you're writing extension methods that extend types from the same namespace as your current namespace, the `::` operator can help access the original namespace:

```csharp
namespace System.Collections.Generic
{
    public static class MyExtensions
    {
        public static void MyMethod<T>(this global::System.Collections.Generic.List<T> list)
        {
            // Implementation
        }
    }
}
```

### 3. Working with generated code

When working with generated code or third-party libraries where you can't change the namespace structure, the `::` operator helps navigate complex namespace hierarchies:

```csharp
using Old = LegacyCompany.LegacyProduct.V1;
using New = ModernCompany.ModernProduct.V2;

class MigrationUtility
{
    public static New::Customer ConvertCustomer(Old::Customer oldCustomer)
    {
        // Conversion logic
        return new New::Customer { /* ... */ };
    }
}
```

## Operator overloadability

The `::` operator cannot be overloaded. It's a language construct with fixed semantics that are integral to C#'s namespace resolution system.

## Key Points

- The namespace alias operator (`::`) accesses members of an aliased namespace
- It can only be used between two identifiers
- The left-hand identifier must be a namespace alias, an extern alias, or the global alias
- It ensures the left-hand identifier is always interpreted as a namespace alias, even if there's a type with the same name
- The `global` keyword is a special alias that always refers to the global namespace
- Unlike the dot operator (`.`), the `::` operator is unambiguous in its purpose
- The `::` operator cannot be overloaded

## FAQs

**Q: When should I use the namespace alias operator instead of the dot operator?**  
A: Use the namespace alias operator when there might be ambiguity between a namespace alias and a type with the same name, or when you explicitly want to indicate that you're accessing a member of an aliased namespace.

**Q: Can I use the namespace alias operator with any namespace?**  
A: No, you can only use it with namespace aliases created with a `using alias` directive, extern aliases, or the global alias. You cannot use it with regular namespaces directly.

**Q: What's the difference between a namespace alias and a using directive?**  
A: A namespace alias (`using Alias = Namespace;`) creates a shorthand name for a namespace, while a using directive (`using Namespace;`) brings all types from that namespace into scope without needing to qualify them.

**Q: Can I use the namespace alias operator with types?**  
A: No, the namespace alias operator can only be used to access members of an aliased namespace. For accessing static members of types, use the dot operator.

**Q: How do I create an extern alias?**  
A: Extern aliases are defined at the assembly reference level, either in your project file or through compiler options, and then declared in your code with the `extern alias` directive.

**Q: Is there a performance impact when using the namespace alias operator?**  
A: No, there's no runtime performance impact. The namespace alias operator is resolved at compile time and has no effect on the generated IL code.

**Q: Can I use the namespace alias operator in all versions of C#?**  
A: Yes, the namespace alias operator has been part of C# since version 1.0.