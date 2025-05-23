# The using directive

### Introduction

The `using` directive helps simplify your code by allowing you to reference types defined in a namespace without repeatedly specifying their fully qualified names. Additionally, it offers advanced modifiers and features to enhance code readability and maintainability.

### Basic Using Directive

The simplest use of the `using` directive imports all types from a specified namespace, reducing verbosity in your code.

```csharp
using System.Text;

class Program
{
    static void Main()
    {
        var sb = new StringBuilder();
        sb.Append("Hello, world!");
    }
}
```

{% hint style="info" %}
Without `using System.Text;`you'd need to write `System.Text.StringBuilder`.
{% endhint %}

### The Global Modifier

`global` applies the `using` directive across your entire project, eliminating the need to include the same directive in multiple files.

```csharp
global using System.Collections.Generic;
```

**Key Points:**

* Place global directives at the top of your file before any namespace/type declarations.
* Usually stored in one central file for easier management.
* Compiler warns if global usings are duplicated in multiple files.

### The Static Modifier

The `using static` directive allows direct access to static members of a type without specifying the type name each time. It improves readability, especially with frequent references to static members

**Before (verbose):**

```csharp
public class Circle
{
    public double Radius { get; set; }
    
    public double Area => System.Math.PI * System.Math.Pow(Radius, 2);
}
```

**After (cleaner):**

```csharp
using static System.Math;

public class Circle
{
    public double Radius { get; set; }
    
    public double Area => PI * Pow(Radius, 2);
}
```

### Using Alias

Aliases simplify referencing namespaces or types by shortening long or complex fully-qualified names.

```csharp
using MyIntList = System.Collections.Generic.List<int>;

class Program
{
    static void Main()
    {
        var numbers = new MyIntList { 1, 2, 3 };
    }
}
```

From C# 12 onwards, you can alias more types, including tuples and pointer types.

```csharp
using IntPointer = int*; // Requires unsafe context
```

### Qualified Alias Member (`::`)

Use `::` to clarify ambiguity when using aliases or accessing the global namespace explicitly.

```csharp
class Console { }

class Program
{
    static void Main()
    {
        global::System.Console.WriteLine("Hello, from global namespace!");
    }
}
```

### Scope and Placement Rules

* Regular `using`: Scoped to the file it’s in; must precede namespace/type declarations.
* Global `using`: Applies to the whole compilation; placed before any non-global directives.
* Static `using`: Imports only static members and nested types; no inherited members.

### Best Practices

* Group `global using` directives in a dedicated file for easy maintenance.
* Prefer clarity over brevity. Aliases should simplify code, not obscure it.
* Limit `using static` to a few essential types per file.

### FAQs

**When should I use the `global` modifier?**\
When multiple files in your project consistently reference the same namespaces, use global usings for convenience.

**Can I combine modifiers like `global` and `static`?**\
Yes, `global using static System.Math;` is valid, enabling access to Math's static methods in every file.

**Is it better to place all global usings in one file?**\
Generally, yes. It improves clarity and simplifies management.

**Can `using static` import extension methods?**\
Not directly into scope; it only makes them available for extension lookup.
