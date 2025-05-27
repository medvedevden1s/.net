# The const keyword

### Introduction

The `const` keyword clearly indicates that a field or a local variable is constant—its value is set once at compile time and cannot change during program execution. Think of constants as values carved into stone; once defined, they remain unchanged.

### Why Use the `const` Keyword?

Imagine you have mathematical constants, like Pi (π), gravitational force, or fixed application settings. These values never change during your application's lifetime, so marking them as constants clearly documents this immutability and helps avoid accidental modifications.

***

### Defining Constants Clearly

Constants can represent numeric values, strings, Booleans, or null references. Let's define some common examples clearly:

```csharp
csharpCopyEditpublic class MathConstants
{
    public const double Pi = 3.14159;
    public const double EulersNumber = 2.71828;
}

public class ApplicationSettings
{
    public const string ApplicationName = "Finance Manager";
    private const bool IsProduction = true;
}

class Program
{
    static void Main()
    {
        Console.WriteLine($"Pi = {MathConstants.Pi}");
        Console.WriteLine($"Euler's Number = {MathConstants.EulersNumber}");
        Console.WriteLine($"Application Name = {ApplicationSettings.ApplicationName}");
    }
}
```

Output:

```bash
Pi = 3.14159
Euler's Number = 2.71828
Application Name = Finance Manager
```

***

### Constants with Interpolated Strings

Interpolated strings can be constants only if every expression inside them is itself constant. This allows clear and readable constant strings:

```csharp
const string Language = "C#";
const string Platform = ".NET";
const string FullDescription = $"{Platform} - Language: {Language}";
```

{% hint style="info" %}
Interpolated constant strings enhance readability by allowing you to clearly define formatted constant values at compile time.
{% endhint %}

***

### Important Rules and Behaviors

*   **Must be Initialized at Declaration:**\
    Constants must clearly have values defined at compile time:

    ```csharp
    csharpCopyEditconst int MaxUsers = 100; // ✅ Correct

    const int MaxSessions;    // ❌ Error: Must have initialization
    ```
*   **Can't Use `static` Modifier:**\
    Constants are implicitly static and can't explicitly use the `static` keyword:

    ```csharp
    csharpCopyEditpublic const int MaxLimit = 50;     // ✅ Correct

    public static const int MinLimit = 10; // ❌ Error: "static" not allowed
    ```
*   **Only Certain Types Allowed:**\
    Constants must clearly be compile-time evaluable. Only numbers, strings, Booleans, or null references are valid:

    ```csharp
    const DateTime Now = DateTime.Now; // ❌ Error: Not compile-time constant
    ```

***

### Difference Between `const` and `readonly`

| Feature           | `const`                           | `readonly`                              |
| ----------------- | --------------------------------- | --------------------------------------- |
| Initialization    | At declaration only.              | At declaration or in constructor.       |
| When value is set | Compile-time                      | Compile-time or run-time                |
| Types Allowed     | Only numbers, strings, bool, null | Any type                                |
| Static            | Implicitly static                 | Explicitly declared as static if needed |

**Example clearly illustrating difference:**

```csharp
csharpCopyEditclass Example
{
    public const int CompileTimeConstant = 10;
    public readonly int RuntimeConstant;

    public Example(int value)
    {
        RuntimeConstant = value;
    }

    static void Main()
    {
        var example = new Example(50);
        Console.WriteLine($"Const: {CompileTimeConstant}, Readonly: {example.RuntimeConstant}");
    }
}
```

Output:

```bash
Const: 10, Readonly: 50
```

***

### Examples

#### Multiple Constant Declarations:

```csharp
csharpCopyEditpublic const double X = 1.0, Y = 2.0, Z = 3.0;
```

#### Constants Used in Expressions:

```csharp
csharpCopyEditpublic const int BaseValue = 5;
public const int ComputedValue = BaseValue + 10; // Result: 15
```

#### Local Constants Example:

```csharp
csharpCopyEditclass Program
{
    static void Main()
    {
        const string ApiVersion = "v1.2.3";
        Console.WriteLine($"API Version: {ApiVersion}");
    }
}
```

Output:

```bash
API Version: v1.2.3
```

***

### Key Points:

* The `const` keyword declares immutable fields or local variables evaluated at compile-time.
* Constants must have an initializer; no runtime assignments allowed.
* Constants can't be explicitly declared `static` they're implicitly static.
* Supported types: numeric, string, bool, or null references.
* Prefer `readonly` for values set during runtime or requiring initialization logic.

***

### FAQs

**Why can't I modify a constant after declaration?**\
Constants embed their values at compile-time, ensuring immutability and efficiency. Modifying would invalidate this guarantee.

**Can I have a constant method parameter?**\
No, method parameters can't be marked `const`. Parameters are always variable by definition.

**Why can't a constant field use runtime-dependent values like `DateTime.Now`?**\
Because constants must be determined clearly at compile time, runtime-dependent values aren't allowed. Use `readonly` instead.

**Should I use `const` for API endpoints?**\
Only if they're guaranteed not to change. Otherwise, use configuration settings or `readonly`.

**What's the advantage of using constants instead of literals?**\
Constants clearly document your code by giving meaningful names to values and avoid magic numbers or strings, improving readability and maintainability.
