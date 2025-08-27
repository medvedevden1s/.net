## Introduction

Pattern matching in C# is like a sophisticated identification system. Just as you might recognize a friend by their face, voice, or walking style, pattern matching lets your code recognize and extract information from data based on its shape, type, or value. It's a powerful way to write more expressive and concise code.

This feature shines when you need to inspect an object and take different actions based on its characteristics, allowing you to write cleaner alternatives to complex chains of if-else statements or type checking.

## The is expression

The `is` expression checks if an object matches a specified pattern. It's the simplest form of pattern matching, often used to verify an object's type before performing operations on it.

```csharp
// Basic type checking
object greeting = "Hello, World!";
if (greeting is string)
{
    Console.WriteLine("The greeting is a string.");
}

// Type checking with variable declaration (declaration pattern)
if (greeting is string message)
{
    // If greeting is a string, it's assigned to message
    Console.WriteLine(message.ToLower());  // output: hello, world!
}

// Checking for non-null with negated pattern
if (greeting is not null)
{
    Console.WriteLine("The greeting is not null.");
}
```

The `is` expression returns `true` when the pattern matches and `false` otherwise, making it perfect for conditional logic.

{% hint style="info" %}
Unlike traditional type checking with `GetType()` or `typeof()`, pattern matching with `is` can simultaneously check the type and extract the value into a new variable.
{% endhint %}

## The switch statement

The `switch` statement evaluates an expression against a series of cases, executing the code block of the first matching case.

```csharp
// Traditional switch on constant values
int dayOfWeek = 3;
switch (dayOfWeek)
{
    case 1:
        Console.WriteLine("Monday");
        break;
    case 2:
        Console.WriteLine("Tuesday");
        break;
    case 3:
        Console.WriteLine("Wednesday");
        break;
    default:
        Console.WriteLine("Another day");
        break;
}

// Pattern matching in switch statements
object vehicle = new Car { Make = "Toyota", Model = "Corolla" };
switch (vehicle)
{
    case Car car when car.Make == "Toyota":
        Console.WriteLine($"Toyota {car.Model}");
        break;
    case Car car:
        Console.WriteLine($"Some other car: {car.Make} {car.Model}");
        break;
    case Truck truck:
        Console.WriteLine($"A truck: {truck.Make}");
        break;
    case null:
        Console.WriteLine("No vehicle provided");
        break;
    default:
        Console.WriteLine("Unknown vehicle type");
        break;
}
```

Pattern matching in `switch` statements allows for more complex conditions than traditional constant-based switches.

## The switch expression

The `switch` expression, introduced in C# 8.0, provides a more concise syntax for pattern matching, especially when you want to compute a value based on a pattern match.

```csharp
// Simple switch expression
string GetDayName(int dayNumber) => dayNumber switch
{
    1 => "Monday",
    2 => "Tuesday",
    3 => "Wednesday",
    4 => "Thursday",
    5 => "Friday",
    6 => "Saturday",
    7 => "Sunday",
    _ => "Invalid day"
};

// Pattern matching with switch expression
decimal CalculateToll(Vehicle vehicle) => vehicle switch
{
    Car { Passengers: > 4 } => 2.00m,     // Car with more than 4 passengers
    Car _ => 3.00m,                       // Any other car
    Truck { GrossWeight: > 5000 } => 10.00m, // Heavy truck
    Truck _ => 5.00m,                     // Any other truck
    null => throw new ArgumentNullException(nameof(vehicle)),
    _ => throw new ArgumentException("Unknown vehicle type", nameof(vehicle))
};
```

The `switch` expression is more compact than the `switch` statement and returns a value directly, making it ideal for assignments and return statements.

{% hint style="success" %}
The discard pattern `_` is essential in switch expressions to handle cases that don't match any other pattern. Without it, the compiler will warn about possible unhandled cases.
{% endhint %}

## Declaration and type patterns

Declaration and type patterns check if an expression's runtime type is compatible with a specified type.

```csharp
// Declaration pattern - checks type and assigns to a variable
if (shape is Circle circle)
{
    double area = Math.PI * circle.Radius * circle.Radius;
    Console.WriteLine($"Circle area: {area}");
}

// Type pattern - just checks the type without assignment
if (shape is Rectangle)
{
    Console.WriteLine("This is a rectangle");
}

// Type pattern in switch expression
string DescribeShape(Shape shape) => shape switch
{
    Circle => "A round shape",
    Rectangle => "A four-sided shape with right angles",
    Triangle => "A three-sided shape",
    _ => "Some other shape"
};
```

Declaration patterns are particularly useful when you need to both check the type and use the object as that type without explicit casting.

## Constant pattern

The constant pattern checks if an expression equals a specific constant value.

```csharp
// Constant pattern with is expression
if (status is 200)
{
    Console.WriteLine("Success");
}

// Constant pattern in switch expression
string GetHttpStatusDescription(int statusCode) => statusCode switch
{
    200 => "OK",
    404 => "Not Found",
    500 => "Internal Server Error",
    _ => "Unknown Status Code"
};

// Null constant pattern
if (response is null)
{
    Console.WriteLine("No response received");
}
```

Constant patterns work with any constant expression, including literals, enum values, and `null`.

## Relational patterns

Relational patterns, introduced in C# 9.0, compare an expression with a constant using relational operators (<, >, <=, >=).

```csharp
// Relational pattern with is expression
if (temperature is > 100)
{
    Console.WriteLine("Water is boiling");
}

// Relational patterns in switch expression
string ClassifyTemperature(double celsius) => celsius switch
{
    < 0 => "Freezing",
    >= 0 and < 20 => "Cool",
    >= 20 and < 40 => "Warm",
    >= 40 => "Hot",
    double.NaN => "Unknown"
};

// Relational patterns with dates
string GetSeason(DateTime date) => date.Month switch
{
    >= 3 and < 6 => "Spring",
    >= 6 and < 9 => "Summer",
    >= 9 and < 12 => "Autumn",
    12 or (>= 1 and < 3) => "Winter",
    _ => throw new ArgumentOutOfRangeException(nameof(date))
};
```

Relational patterns make range checks more readable and concise compared to traditional comparison operators.

## Logical patterns

Logical patterns combine other patterns using the `not`, `and`, and `or` operators.

```csharp
// Negation pattern (not)
if (input is not null)
{
    Console.WriteLine("Input provided");
}

// Conjunction pattern (and)
if (number is > 0 and < 100)
{
    Console.WriteLine("Number is between 1 and 99");
}

// Disjunction pattern (or)
if (day is "Saturday" or "Sunday")
{
    Console.WriteLine("It's the weekend");
}

// Combined logical patterns
string GetLetterGrade(int score) => score switch
{
    >= 90 and <= 100 => "A",
    >= 80 and < 90 => "B",
    >= 70 and < 80 => "C",
    >= 60 and < 70 => "D",
    >= 0 and < 60 => "F",
    _ => "Invalid score"
};
```

Logical patterns can be combined to create complex conditions while maintaining readability.

{% hint style="warning" %}
Be careful with operator precedence in logical patterns. The `not` pattern binds most tightly, followed by `and`, then `or`. Use parentheses to clarify your intent: `is not (>= 0 and <= 100)`.
{% endhint %}

## Property pattern

Property patterns match an expression's properties or fields against nested patterns.

```csharp
// Simple property pattern
if (person is { Name: "John", Age: >= 18 })
{
    Console.WriteLine("Adult named John");
}

// Nested property patterns
if (order is { Customer: { IsVIP: true }, TotalAmount: > 1000 })
{
    Console.WriteLine("VIP customer with large order");
}

// Extended property pattern (shorthand for nested properties)
if (order is { Customer.IsVIP: true, TotalAmount: > 1000 })
{
    Console.WriteLine("VIP customer with large order");
}

// Property pattern in switch expression
string DescribePoint(Point point) => point switch
{
    { X: 0, Y: 0 } => "Origin",
    { X: 0 } => "On Y-axis",
    { Y: 0 } => "On X-axis",
    { X: > 0, Y: > 0 } => "First quadrant",
    { X: < 0, Y: > 0 } => "Second quadrant",
    { X: < 0, Y: < 0 } => "Third quadrant",
    { X: > 0, Y: < 0 } => "Fourth quadrant",
    _ => "Somewhere on the plane"
};
```

Property patterns are powerful for checking multiple conditions on an object's properties in a single expression.

## Positional pattern

Positional patterns deconstruct an expression and match the resulting values against nested patterns.

```csharp
// Class with Deconstruct method
public class Point
{
    public int X { get; }
    public int Y { get; }
    
    public Point(int x, int y) => (X, Y) = (x, y);
    
    // Deconstruct method enables positional pattern matching
    public void Deconstruct(out int x, out int y) => (x, y) = (X, Y);
}

// Positional pattern matching
if (point is (0, 0))
{
    Console.WriteLine("Point is at the origin");
}

// Positional pattern with nested patterns
if (point is (> 0, > 0))
{
    Console.WriteLine("Point is in the first quadrant");
}

// Positional pattern in switch expression
string ClassifyPoint(Point point) => point switch
{
    (0, 0) => "Origin",
    (0, _) => "On Y-axis",
    (_, 0) => "On X-axis",
    var (x, y) when x > 0 && y > 0 => "First quadrant",
    (< 0, > 0) => "Second quadrant",
    (< 0, < 0) => "Third quadrant",
    (> 0, < 0) => "Fourth quadrant",
    _ => "Somewhere on the plane"
};
```

Positional patterns work with tuples and any type that provides a `Deconstruct` method.

## Var pattern

The var pattern matches any expression and assigns its result to a new variable.

```csharp
// Var pattern to capture the result of an expression
if (GetValue() is var result && result > 0)
{
    Console.WriteLine($"Positive result: {result}");
}

// Var pattern in switch expression with when clause
string ProcessData(object data) => data switch
{
    var obj when obj?.ToString().Length > 10 => "Long string",
    var n when n is int number && number > 0 => "Positive number",
    var _ => "Something else"
};
```

The var pattern is useful when you need to store an intermediate result for further checks or operations.

## Discard pattern

The discard pattern `_` matches any expression without assigning it to a variable.

```csharp
// Discard pattern in positional matching
if (point is (_, 0))
{
    Console.WriteLine("Point is on the X-axis");
}

// Discard pattern in switch expression
string GetDiscountRate(DayOfWeek day) => day switch
{
    DayOfWeek.Saturday or DayOfWeek.Sunday => "5%",
    DayOfWeek.Monday => "3%",
    _ => "0%"  // Matches any other day
};
```

The discard pattern is essential for switch expressions to handle all possible cases and avoid compiler warnings.

## List patterns

List patterns, introduced in C# 11, match arrays or lists against a sequence of patterns.

```csharp
// Simple list pattern
if (numbers is [1, 2, 3])
{
    Console.WriteLine("Array contains exactly 1, 2, 3");
}

// List pattern with nested patterns
if (numbers is [> 0, > 0, > 0])
{
    Console.WriteLine("Array contains exactly three positive numbers");
}

// Slice pattern to match parts of a sequence
if (numbers is [1, 2, .., 9, 10])
{
    Console.WriteLine("Array starts with 1, 2 and ends with 9, 10");
}

// List pattern in switch expression
string DescribeArray(int[] numbers) => numbers switch
{
    [] => "Empty array",
    [var single] => $"Single element: {single}",
    [var first, var second] => $"Two elements: {first}, {second}",
    [1, 2, 3] => "Array with exactly 1, 2, 3",
    [0, ..] => "Array starting with 0",
    [.., 0] => "Array ending with 0",
    [var first, .., var last] => $"Array starting with {first} and ending with {last}",
    _ => "Some other array"
};
```

List patterns are powerful for working with sequences, allowing you to match elements at specific positions or check for patterns at the beginning or end of a sequence.

{% hint style="info" %}
The slice pattern `..` matches zero or more elements. You can use at most one slice pattern in a list pattern.
{% endhint %}

## Key Points

- Pattern matching allows you to test expressions against patterns and extract data when a match succeeds
- The `is` expression checks if an expression matches a pattern
- The `switch` statement and expression provide multi-branch pattern matching
- C# supports many pattern types: declaration, type, constant, relational, logical, property, positional, var, discard, and list
- Patterns can be combined using logical operators (`and`, `or`, `not`)
- Property patterns check an object's properties against nested patterns
- Positional patterns work with tuples and types with a `Deconstruct` method
- List patterns match arrays and lists against sequences of patterns
- Pattern matching often leads to more readable and concise code than traditional if-else chains

## FAQs

**Q: When should I use pattern matching instead of traditional if-else statements?**  
A: Use pattern matching when you need to check an object's type, extract data from it, or perform different actions based on multiple conditions. It's especially valuable when dealing with hierarchies of types or complex conditional logic.

**Q: What's the difference between switch statements and switch expressions?**  
A: Switch statements are traditional control flow statements with case blocks and break statements. Switch expressions, introduced in C# 8.0, are more concise, use the => syntax, and return a value directly. Switch expressions are ideal when computing a value based on a pattern match.

**Q: Can I use pattern matching with my custom types?**  
A: Yes! For property patterns, your type just needs properties or fields. For positional patterns, implement a `Deconstruct` method. Record types automatically provide a `Deconstruct` method based on their constructor parameters.

**Q: What happens if no pattern matches in a switch expression?**  
A: If no pattern matches and there's no discard pattern (`_`), a `SwitchExpressionException` is thrown at runtime. Always include a discard pattern as a fallback unless you're certain all possible cases are covered.

**Q: How does pattern matching perform compared to traditional if-else chains?**  
A: The compiler optimizes pattern matching, so performance is generally comparable to equivalent if-else chains. For simple type checks, traditional methods might be slightly faster, but pattern matching often leads to more maintainable code.

**Q: Can I use pattern matching with nullable value types?**  
A: Yes, pattern matching works well with nullable types. For example, `if (nullable is int value)` will check if the nullable has a value and assign it to `value` if it does.

**Q: What C# versions introduced different pattern matching features?**  
A: Basic pattern matching with `is` was introduced in C# 7.0. C# 8.0 added switch expressions and property patterns. C# 9.0 added relational and logical patterns. C# 11.0 added list patterns.