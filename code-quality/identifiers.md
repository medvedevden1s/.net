# Identifiers

Identifiers are names you assign to elements in your code, such as classes, variables, methods, interfaces, enums, structs, and namespaces. Using clear and consistent identifiers makes your code easier to read and maintain.

Let's clearly understand the rules and conventions for creating identifiers in C#.

### Naming Rules

Identifiers in C# must follow specific rules. The compiler generates errors if these rules aren't met:

* Must begin with a letter or underscore (`_`).
* Can contain letters, digits, or underscores.
* Can include Unicode characters.
* Can't match reserved keywords unless prefixed with `@`.

#### Examples of Valid Identifiers:

```csharp
int userAge;
string _userName;
float temperatureInCelsius;
var π = 3.14159; // Unicode character
int @class; // using reserved keyword with @ prefix
```

#### Examples of Invalid Identifiers:

```csharp
int 2ndUser;  // starts with a digit
string user-name; // contains invalid character '-'
double public; // reserved keyword without @ prefix
```

{% hint style="warning" %}
Avoid using the `@` prefix unless necessary, as it decreases readability
{% endhint %}

### Naming Conventions

While not enforced by the compiler, C# follows standard naming conventions for readability and maintainability.

#### PascalCase

Use PascalCase for classes, methods, namespaces, interfaces, structs, enums, delegates, and public properties.

```csharp
// Class
public class PaymentProcessor
{
    // Method
    public void ProcessPayment(decimal amount)
    {
        // Implementation
    }
}

// Interface
public interface IOrderService
{
    void SubmitOrder();
}

// Struct
public struct Coordinate
{
    public double Latitude;
    public double Longitude;
}

// Enum
public enum Color
{
    Red,
    Blue,
    Green
}
```

#### camelCase

Use camelCase for private fields, method parameters, and local variables. Prefix private instance fields with `_`.

```csharp
public class UserService
{
    private string _userName;

    public void SetUserName(string userName)
    {
        _userName = userName;
    }
}
```

#### Static Fields

Use the prefix `s_` for static fields and `t_` for thread-static fields.

```csharp
public class Logger
{
    private static ILogger s_loggerInstance;

    [ThreadStatic]
    private static int t_threadCount;
}
```

#### Constants

Use PascalCase for constants, both local and class-level.

```csharp
public class MathConstants
{
    public const double Pi = 3.14159;
}
```

### Recommended Best Practices

* **Avoid consecutive underscores (`__`).** Such identifiers are reserved for compiler-generated code.
* **Use meaningful and descriptive names.** Prefer clarity over brevity.
* **Avoid abbreviations unless widely understood (e.g., HTTP, XML).**
* **Single-letter variables should only be used in loops or generic types.**

#### Example of Clear Identifier Usage

```csharp
public class OrderProcessor
{
    private readonly IOrderService _orderService;

    public OrderProcessor(IOrderService orderService)
    {
        _orderService = orderService;
    }

    public void ProcessNewOrder(Order newOrder)
    {
        if (newOrder.IsValid())
        {
            _orderService.SubmitOrder();
        }
    }
}
```

{% hint style="success" %}
Consistent and clear identifiers significantly enhance code readability and maintenance.&#x20;
{% endhint %}



### ❓ **FAQ**

**Can an identifier start with a number?**\
No, identifiers must begin with a letter or underscore (`_`).

**Are identifiers case-sensitive in C#?**\
Yes, C# identifiers are case-sensitive. For example, `userAge` and `UserAge` are treated as distinct identifiers.

**Can I use reserved keywords as identifiers?**\
Yes, by prefixing the keyword with `@` (e.g., `int @class;`). However, it's discouraged as it can reduce readability.

**Is there a limit to identifier length in C#?**\
No explicit limit is enforced, but identifiers should remain concise for readability and maintainability.

**Can identifiers include Unicode characters?**\
Yes, identifiers can include Unicode characters, enhancing support for internationalization. For example, you could use Greek letters such as `π`.

**Why should I avoid using consecutive underscores (`__`)?**\
Identifiers containing consecutive underscores are reserved for compiler-generated code, so using them is discouraged to avoid potential conflicts.
