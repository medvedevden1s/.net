# Code Conventions

Coding conventions help teams maintain readability, consistency, and quality of code. This article clearly explains common C# coding conventions adopted by the dotnet/docs team, focusing on best practices, modern language usage, and clarity.

### Goals of Coding Conventions

We selected our coding conventions based on these clear goals:

* **Correctness**: Ensure code examples remain accurate and robust.
* **Teaching**: Clearly demonstrate when specific language features are appropriate.
* **Consistency**: Provide uniform experiences across documentation and samples.
* **Adoption**: Promote new language features and ensure samples reflect current best practices.

### Tools and Analyzers

We use tools like `.editorconfig` and code analyzers in Visual Studio to enforce coding conventions automatically. Using these tools simplifies compliance with guidelines, reducing manual code reviews.

### Language Guidelines

Follow these core guidelines to ensure readability, correctness, and consistency:

* Use modern C# features whenever applicable.
* Avoid deprecated or outdated constructs.
* Handle exceptions explicitly and meaningfully.
* Prefer LINQ methods for readability.
* Use asynchronous programming (`async` and `await`) for I/O-bound operations.
* Clearly differentiate between run-time types (`string`) and language keywords (`int`).
* Choose `int` over unsigned types unless specifically needed.
* Use `var` only when the type is clear from the context.
* Write clear, simple, and understandable code.

#### Strings and String Interpolation

Use string interpolation for concatenating short strings:

```csharp
string displayName = $"{nameList[n].LastName}, {nameList[n].FirstName}";
```

Use `StringBuilder` for concatenating strings in loops:

```csharp
var phrase = "Sample phrase";
var builder = new StringBuilder();
for (var i = 0; i < 1000; i++)
{
    builder.Append(phrase);
}
```

Prefer raw string literals:

```csharp
var message = """
    Multiline string with raw literals.
    Special characters like \n and \t are literal.
    """;
```

#### Constructors and Initialization

Use PascalCase for record primary constructors:

```csharp
public record Person(string FirstName, string LastName);
```

Use required properties instead of constructors for initialization enforcement:

```csharp
public class Container<T>
{
    public required T Content { get; init; }
}
```

#### Arrays and Collections

Initialize collections succinctly:

```csharp
string[] vowels = ["a", "e", "i", "o", "u"];
```

#### Delegates

Use built-in delegates (`Func<>`, `Action<>`) rather than defining custom delegates:

```csharp
Action<string> printMessage = message => Console.WriteLine(message);
Func<int, int, int> sum = (a, b) => a + b;
```

#### Exception Handling

Prefer `try-catch` blocks for explicit exception handling:

```csharp
try
{
    // Risky operation
}
catch (SpecificException ex)
{
    Console.WriteLine(ex.Message);
}
```

Use the concise `using` statement for disposable resources:

```csharp
using var resource = new Resource();
resource.Use();
```

#### Conditional Operators

Always use short-circuiting logical operators:

```csharp
if ((value != null) && value.IsValid)
{
    // Safe to proceed
}
```

#### Object Instantiation

Use concise syntax:

```csharp
ExampleClass instance = new();
```

#### Event Handling

Prefer lambda expressions for event handlers that don’t require removal:

```csharp
button.Click += (sender, args) => Console.WriteLine("Button clicked");
```

#### Static Members

Clearly call static members using class names:

```csharp
Math.Abs(-10);
```

#### LINQ Queries

Clearly name query variables and use implicit typing:

```csharp
var seattleCustomers = from customer in Customers
                       where customer.City == "Seattle"
                       select customer;
```

#### Implicit Typing

Use implicit typing (`var`) when the type is obvious from context:

```csharp
var count = 5;
```

Explicitly type variables when not obvious:

```csharp
int parsedValue = int.Parse(input);
```

#### File Scoped Namespaces

Prefer file-scoped namespace declarations:

```csharp
namespace MyNamespace;
```

#### Using Directives

Place using directives outside namespace declarations to ensure clarity and avoid ambiguity:

```csharp
using System;

namespace MyNamespace
{
    public class Example { }
}
```

### Formatting and Layout

* Use 4-space indentation.
* Adhere to 65-character line limits for readability.
* Use Allman style braces clearly:

```csharp
if (condition)
{
    // Statements
}
```

### Comments

* Use single-line comments (`//`) clearly for brief explanations.
* Use XML comments for public methods and properties:

```csharp
/// <summary>
/// Processes the input and returns the result.
/// </summary>
```

### Layout and Clarity

* Keep one statement per line.
* Use blank lines to separate methods.
* Clearly use parentheses to emphasize logic:

```csharp
if ((x > y) && (y > z))
{
    // logic
}
```

Following these clear and consistent guidelines will significantly enhance readability, maintainability, and quality of your C# code.
