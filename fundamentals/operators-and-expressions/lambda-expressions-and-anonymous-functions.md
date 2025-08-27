## Introduction

Lambda expressions in C# are like shorthand notes for functions. Just as you might jot down a quick note instead of writing a formal letter, lambda expressions let you define small, focused functions without the ceremony of creating a named method. They're compact, expressive, and can be passed around like any other value.

These anonymous functions are particularly valuable when you need to encapsulate a small piece of logic that doesn't warrant a full method definition, especially when working with collections, asynchronous operations, or event handlers.

## Lambda operator (`=>`)

The lambda operator => (sometimes called the "goes to" operator) is the defining feature of lambda expressions. It separates the input parameters from the function body, like a bridge connecting what goes in to what comes out.

```csharp
// Expression lambda with one parameter
Func<int, int> square = x => x * x;

// Expression lambda with multiple parameters
Func<int, int, int> add = (x, y) => x + y;

// Statement lambda with code block
Func<int, string> numberDescription = x => 
{
    if (x < 0) return "Negative";
    if (x > 0) return "Positive";
    return "Zero";
};

// Parameterless lambda
Action sayHello = () => Console.WriteLine("Hello, World!");
```

Lambda expressions come in two main forms:
- **Expression lambdas**: Consist of a single expression that becomes the return value
- **Statement lambdas**: Contain a block of statements enclosed in braces

{% hint style="info" %}
The lambda operator => is different from the conditional operator ?:, though they may look similar at first glance.
{% endhint %}

## Expression lambdas

Expression lambdas are the simplest form, containing just a single expression after the => operator. The result of this expression becomes the return value of the lambda.

```csharp
// Simple expression lambda
Func<int, bool> isEven = number => number % 2 == 0;

// Using expression lambda with LINQ
int[] numbers = { 1, 2, 3, 4, 5 };
var evenNumbers = numbers.Where(n => n % 2 == 0);
Console.WriteLine(string.Join(", ", evenNumbers));  // Output: 2, 4

// Expression lambda with multiple parameters
Func<double, double, double> power = (x, y) => Math.Pow(x, y);
Console.WriteLine(power(2, 3));  // Output: 8
```

Expression lambdas are particularly useful in LINQ queries and other functional programming scenarios where you need to pass simple operations as arguments.

{% hint style="success" %}
Expression lambdas can be converted to expression trees, which enables scenarios like LINQ to SQL where C# code is translated to SQL queries.
{% endhint %}

## Statement lambdas

When you need more complex logic, statement lambdas allow you to include multiple statements in a code block. Unlike expression lambdas, statement lambdas require explicit return statements if they return a value.

```csharp
// Statement lambda with multiple statements
Func<int, string> gradeScore = score => 
{
    if (score >= 90) return "A";
    if (score >= 80) return "B";
    if (score >= 70) return "C";
    if (score >= 60) return "D";
    return "F";
};

// Statement lambda with void return
Action<string> logMessage = message => 
{
    string timestamp = DateTime.Now.ToString("yyyy-MM-dd HH:mm:ss");
    Console.WriteLine($"[{timestamp}] {message}");
};

// Statement lambda with local variables
Func<int[], double> calculateAverage = numbers => 
{
    int sum = 0;
    foreach (var num in numbers)
    {
        sum += num;
    }
    return (double)sum / numbers.Length;
};
```

Statement lambdas are useful when:
- You need to perform multiple operations
- You need to declare local variables
- You have conditional logic or loops
- You need to handle exceptions

{% hint style="warning" %}
Unlike expression lambdas, statement lambdas cannot be converted to expression trees, which means they can't be used with providers that require expression trees, like LINQ to SQL.
{% endhint %}

## Input parameters

Lambda expressions can have zero or more input parameters. The parameter types can be explicitly declared or implicitly inferred by the compiler.

```csharp
// No parameters
Action helloWorld = () => Console.WriteLine("Hello, World!");

// One parameter with implicit type
Func<int, int> doubleIt = x => x * 2;

// One parameter with explicit type
Func<int, int> tripleIt = (int x) => x * 3;

// Multiple parameters with implicit types
Func<int, int, int> multiply = (x, y) => x * y;

// Multiple parameters with explicit types
Func<double, double, double> divide = (double x, double y) => x / y;

// Using discards for unused parameters
Func<int, int, int> ignoreSecond = (x, _) => x * 2;
```

Beginning with C# 12, you can provide default values for explicitly typed parameter lists:

```csharp
// Lambda with default parameter value
var incrementBy = (int source, int increment = 1) => source + increment;
Console.WriteLine(incrementBy(5));     // Output: 6
Console.WriteLine(incrementBy(5, 2));  // Output: 7

// Lambda with params array
var sum = (params int[] values) => 
{
    int total = 0;
    foreach (var value in values)
    {
        total += value;
    }
    return total;
};

Console.WriteLine(sum(1, 2, 3));  // Output: 6
```

{% hint style="info" %}
When using a single parameter without an explicit type, the parentheses around the parameter are optional. For all other cases (zero parameters, multiple parameters, or explicitly typed parameters), parentheses are required.
{% endhint %}

## Async lambdas

Lambda expressions can be asynchronous by adding the `async` modifier before the parameter list and using the `await` keyword within the lambda body.

```csharp
// Async expression lambda
Func<Task<string>> fetchDataAsync = async () => 
{
    using var client = new HttpClient();
    var response = await client.GetStringAsync("https://api.example.com/data");
    return response;
};

// Async statement lambda
Func<int, Task<string>> fetchUserDataAsync = async (userId) => 
{
    using var client = new HttpClient();
    try
    {
        var response = await client.GetStringAsync($"https://api.example.com/users/{userId}");
        return response;
    }
    catch (HttpRequestException)
    {
        return "User not found";
    }
};

// Async event handler
button.Click += async (sender, e) => 
{
    statusLabel.Text = "Processing...";
    await Task.Delay(1000);  // Simulate work
    statusLabel.Text = "Completed!";
};
```

Async lambdas are particularly useful for:
- Event handlers that need to perform asynchronous operations
- LINQ queries that involve asynchronous operations
- Callback functions for asynchronous methods

{% hint style="warning" %}
When using async lambdas, be careful about capturing variables from the outer scope, as they might change before the asynchronous operation completes.
{% endhint %}

## Lambda expressions and tuples

C# provides built-in support for tuples, which can be used as parameters or return values in lambda expressions.

```csharp
// Lambda with tuple parameter
Func<(int x, int y), double> calculateDistance = point => 
    Math.Sqrt(point.x * point.x + point.y * point.y);

var point = (x: 3, y: 4);
Console.WriteLine(calculateDistance(point));  // Output: 5

// Lambda returning a tuple
Func<int, (int square, int cube)> calculatePowers = n => 
    (square: n * n, cube: n * n * n);

var results = calculatePowers(3);
Console.WriteLine($"Square: {results.square}, Cube: {results.cube}");  // Output: Square: 9, Cube: 27

// Lambda with multiple tuple parameters
Func<(int x, int y), (int x, int y), double> distanceBetweenPoints = 
    (p1, p2) => Math.Sqrt(Math.Pow(p2.x - p1.x, 2) + Math.Pow(p2.y - p1.y, 2));

var point1 = (x: 0, y: 0);
var point2 = (x: 3, y: 4);
Console.WriteLine(distanceBetweenPoints(point1, point2));  // Output: 5
```

Tuples provide a convenient way to group related values without creating a custom class or struct, making lambda expressions more expressive and concise.

## Variable capture

Lambda expressions can access variables defined in the enclosing scope, a feature known as "variable capture" or "closure." The captured variables are preserved even if they would normally go out of scope.

```csharp
// Capturing a local variable
int factor = 10;
Func<int, int> multiply = x => x * factor;
Console.WriteLine(multiply(5));  // Output: 50

// The captured variable can be modified
factor = 20;
Console.WriteLine(multiply(5));  // Output: 100

// Capturing multiple variables
int offset = 5;
Func<int, int> computeWithCapture = x => 
{
    int localVar = 2;  // Local to the lambda
    return x * factor + offset + localVar;
};

// Capturing in a loop
var actions = new List<Action>();
for (int i = 0; i < 5; i++)
{
    // Caution: This captures the loop variable i
    actions.Add(() => Console.WriteLine(i));
}

// All actions will print 5, not 0,1,2,3,4 as might be expected
foreach (var action in actions)
{
    action();
}
```

To avoid unexpected behavior when capturing loop variables, you can create a local copy inside the loop:

```csharp
var correctActions = new List<Action>();
for (int i = 0; i < 5; i++)
{
    int capturedCopy = i;  // Create a local copy
    correctActions.Add(() => Console.WriteLine(capturedCopy));
}

// Now prints 0,1,2,3,4 as expected
foreach (var action in correctActions)
{
    action();
}
```

{% hint style="warning" %}
Be careful when capturing variables in loops, as the lambda captures the variable itself, not its value at the time the lambda is created.
{% endhint %}

Beginning with C# 9, you can apply the `static` modifier to a lambda expression to prevent it from capturing variables from the enclosing scope:

```csharp
int factor = 10;

// This will not compile because static lambdas cannot capture local variables
// Func<int, int> multiply = static x => x * factor;

// Static lambdas can only access static members and constants
Func<int, int> square = static x => x * x;
```

## Lambdas with LINQ

Lambda expressions are commonly used with LINQ (Language Integrated Query) to filter, transform, and aggregate data.

```csharp
// Sample data
var products = new[]
{
    new { Name = "Laptop", Price = 1200, Category = "Electronics" },
    new { Name = "Phone", Price = 800, Category = "Electronics" },
    new { Name = "Chair", Price = 150, Category = "Furniture" },
    new { Name = "Desk", Price = 300, Category = "Furniture" },
    new { Name = "Headphones", Price = 100, Category = "Electronics" }
};

// Filtering with Where
var expensiveProducts = products.Where(p => p.Price > 500);

// Transforming with Select
var productNames = products.Select(p => p.Name);

// Ordering with OrderBy
var orderedByPrice = products.OrderBy(p => p.Price);

// Grouping with GroupBy
var groupedByCategory = products.GroupBy(p => p.Category);

// Aggregation with lambda
var totalElectronicsValue = products
    .Where(p => p.Category == "Electronics")
    .Sum(p => p.Price);

// Multiple operations chained together
var averageFurniturePrice = products
    .Where(p => p.Category == "Furniture")
    .Average(p => p.Price);

// Using multiple parameters with lambda
var priceRange = products.Aggregate(
    (min: double.MaxValue, max: double.MinValue),
    (acc, p) => (
        Math.Min(acc.min, p.Price),
        Math.Max(acc.max, p.Price)
    )
);
```

LINQ methods typically accept delegates like `Func<T, bool>` for filtering or `Func<T, TResult>` for transformations, making lambda expressions a perfect fit.

{% hint style="success" %}
When using lambda expressions with LINQ to Objects, the lambda is compiled to a delegate. When using them with LINQ to SQL or Entity Framework, the lambda is converted to an expression tree that can be translated to SQL.
{% endhint %}

## Type inference and natural type

The compiler can often infer the type of a lambda expression based on the context in which it's used.

```csharp
// Type inferred from assignment
Func<int, bool> isPositive = x => x > 0;

// Type inferred from method parameter
var positiveNumbers = numbers.Where(x => x > 0);

// Explicit parameter type when inference isn't possible
var result = objects.FirstOrDefault((string s) => s.Length > 5);

// Explicit return type
var divideOrDefault = double (int x, int y) => y != 0 ? (double)x / y : 0;
```

Beginning with C# 10, lambda expressions can have a natural type, allowing them to be used without explicit delegate type declarations:

```csharp
// Natural type inference (C# 10+)
var square = (int x) => x * x;  // Func<int, int>
var print = (string s) => Console.WriteLine(s);  // Action<string>
var isValid = (string s) => !string.IsNullOrEmpty(s);  // Func<string, bool>
```

{% hint style="info" %}
Not all lambda expressions have a natural type. If the compiler can't infer the parameter types, you must provide them explicitly or assign the lambda to a delegate type.
{% endhint %}

## Attributes on lambda expressions

Starting with C# 10, you can add attributes to lambda expressions and their parameters:

```csharp
// Attribute on the lambda expression
var parse = [return: NotNullIfNotNull("s")] (string? s) => 
    s != null ? int.Parse(s) : null;

// Attributes on parameters
var concat = ([DisallowNull] string a, [DisallowNull] string b) => a + b;

// Multiple attributes
var process = [Obsolete][return: MaybeNull] (object? input) => 
    input?.ToString();
```

Attributes on lambda expressions are useful for code analysis and can be discovered via reflection, but they don't affect the runtime behavior of the lambda when invoked.

## Key Points

- Lambda expressions create anonymous functions using the => operator
- Expression lambdas contain a single expression and implicitly return its result
- Statement lambdas contain a block of code and require explicit return statements
- Lambda parameters can have explicit or implicit types
- Beginning with C# 12, lambda parameters can have default values and params arrays
- Async lambdas allow asynchronous operations with the async and await keywords
- Lambdas can capture variables from the enclosing scope (closures)
- Static lambdas prevent variable capture from the enclosing scope
- Lambda expressions are commonly used with LINQ for data manipulation
- C# 10 introduced natural type inference for lambda expressions
- Lambda expressions can have attributes applied to them and their parameters

## FAQs

**Q: What's the difference between a lambda expression and an anonymous method?**  
A: Lambda expressions are a more concise syntax introduced in C# 3.0 that replaced the older anonymous method syntax (`delegate(int x) { return x * x; }`). Lambda expressions are more flexible and support expression trees, which anonymous methods don't.

**Q: Can lambda expressions access instance members of the containing class?**  
A: Yes, lambda expressions can access instance members, static members, and local variables from the enclosing scope, unless they're declared with the `static` modifier.

**Q: What happens to captured variables when a lambda is returned from a method?**  
A: Captured variables are stored in a compiler-generated class, allowing them to live beyond the scope of the method that created the lambda. This is why closures work even when the original scope is gone.

**Q: Can I use lambda expressions in all versions of C#?**  
A: Lambda expressions were introduced in C# 3.0. Features like async lambdas came in C# 5.0, static lambdas in C# 9.0, natural type inference in C# 10.0, and default parameters in C# 12.0.

**Q: What's the performance impact of using lambda expressions?**  
A: For most scenarios, the performance impact is negligible. However, creating many short-lived lambdas in performance-critical code can cause additional garbage collection pressure. In such cases, consider using local functions or cached delegates.

**Q: Can I use ref, out, or in parameters with lambda expressions?**  
A: Yes, lambda expressions support ref, out, and in parameters, but they must be explicitly typed:
```csharp
Func<int, ref int, bool> refExample = (int x, ref int y) => { y += x; return true; };
```

**Q: How do I debug lambda expressions?**  
A: You can set breakpoints inside lambda expressions just like regular methods. For complex lambdas, consider extracting them to named methods for easier debugging.
