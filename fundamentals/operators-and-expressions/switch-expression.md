## Introduction

The `switch` expression is like a sophisticated sorting machine. Imagine a conveyor belt carrying various items that need to be directed to different destinations based on their characteristics. The switch expression examines each item, determines which category it belongs to, and immediately routes it to the appropriate path—all in one smooth, continuous motion.

In the world of programming, we often need to evaluate an expression and choose from multiple possible values based on a pattern match. Before switch expressions, this typically required verbose switch statements or multiple if-else chains, leading to code that was harder to read and maintain. The switch expression provides a concise, expression-based alternative that transforms this common pattern into a more elegant and functional form.

## Basic syntax and usage

The switch expression evaluates a single expression from a list of candidate expressions based on a pattern match with an input expression:

```csharp
public static class SwitchExample
{
    public enum Direction
    {
        Up,
        Down,
        Right,
        Left
    }

    public enum Orientation
    {
        North,
        South,
        East,
        West
    }

    public static Orientation ToOrientation(Direction direction) => direction switch
    {
        Direction.Up    => Orientation.North,
        Direction.Right => Orientation.East,
        Direction.Down  => Orientation.South,
        Direction.Left  => Orientation.West,
        _ => throw new ArgumentOutOfRangeException(nameof(direction), $"Not expected direction value: {direction}"),
    };

    public static void Main()
    {
        var direction = Direction.Right;
        Console.WriteLine($"Map view direction is {direction}");
        Console.WriteLine($"Cardinal orientation is {ToOrientation(direction)}");
        // Output:
        // Map view direction is Right
        // Cardinal orientation is East
    }
}
```

The basic elements of a switch expression are:

1. An expression followed by the `switch` keyword (in this example, the `direction` parameter)
2. A series of switch expression arms, separated by commas
3. Each arm contains a pattern, an optional case guard, the `=>` token, and an expression

The result of a switch expression is the value of the expression from the first matching arm. The arms are evaluated in text order, so the order matters.

{% hint style="info" %}
Unlike the switch statement, the switch expression is an expression that returns a value, not a control flow statement. This makes it ideal for assignments, return statements, and other contexts where you need a value.
{% endhint %}

## Pattern matching in switch expressions

Switch expressions support a wide variety of patterns, making them extremely versatile:

### Constant patterns

The most basic pattern matches against constant values:

```csharp
string GetDayType(DayOfWeek day) => day switch
{
    DayOfWeek.Saturday => "weekend",
    DayOfWeek.Sunday => "weekend",
    _ => "weekday"
};
```

### Type patterns

You can match based on the type of an object:

```csharp
decimal GetValue(object obj) => obj switch
{
    string s => decimal.Parse(s),
    int i => i,
    double d => (decimal)d,
    _ => throw new ArgumentException("Unsupported type", nameof(obj))
};
```

### Property patterns

Property patterns allow you to match against properties of an object:

```csharp
string GetQuadrant(Point point) => point switch
{
    { X: > 0, Y: > 0 } => "First quadrant",
    { X: < 0, Y: > 0 } => "Second quadrant",
    { X: < 0, Y: < 0 } => "Third quadrant",
    { X: > 0, Y: < 0 } => "Fourth quadrant",
    _ => "On an axis"
};
```

### Tuple patterns

You can match against tuples for multi-value matching:

```csharp
string GetCoordinateDescription((int X, int Y) point) => point switch
{
    (0, 0) => "Origin",
    (var x, 0) => $"X-axis at {x}",
    (0, var y) => $"Y-axis at {y}",
    (var x, var y) when x == y => $"Diagonal at {x},{y}",
    _ => "Just a point"
};
```

### Discard pattern

The discard pattern (`_`) matches any value and is typically used as the last arm to handle any remaining cases:

```csharp
int GetValue(object obj) => obj switch
{
    string s when int.TryParse(s, out var result) => result,
    int i => i,
    _ => 0  // Default case using discard pattern
};
```

## Case guards

Sometimes a pattern alone isn't expressive enough to specify the condition for an arm. In such cases, you can use a case guard with the `when` keyword:

```csharp
public readonly struct Point
{
    public Point(int x, int y) => (X, Y) = (x, y);
    
    public int X { get; }
    public int Y { get; }
}

static Point Transform(Point point) => point switch
{
    { X: 0, Y: 0 }                    => new Point(0, 0),
    { X: var x, Y: var y } when x < y => new Point(x + y, y),
    { X: var x, Y: var y } when x > y => new Point(x - y, y),
    { X: var x, Y: var y }            => new Point(2 * x, 2 * y),
};
```

Case guards provide additional flexibility when the patterns themselves aren't sufficient to express your matching logic.

## Exhaustiveness checking

A key feature of switch expressions is exhaustiveness checking. The compiler will warn you if your switch expression doesn't handle all possible input values:

```csharp
// Warning: The switch expression does not handle all possible values of its input type
Direction GetOpposite(Direction direction) => direction switch
{
    Direction.Up => Direction.Down,
    Direction.Down => Direction.Up,
    Direction.Left => Direction.Right,
    // Missing case for Direction.Right
};
```

If none of the patterns match at runtime, a `SwitchExpressionException` is thrown (in .NET Core 3.0 and later) or an `InvalidOperationException` (in .NET Framework).

{% hint style="warning" %}
Always ensure your switch expressions are exhaustive by either handling all possible cases explicitly or including a discard pattern (`_`) as the last arm to catch any remaining cases.
{% endhint %}

## Comparison with switch statements

Switch expressions offer several advantages over traditional switch statements:

```csharp
// Traditional switch statement
string GetQuadrantStatement(Point point)
{
    switch (point)
    {
        case { X: > 0, Y: > 0 }:
            return "First quadrant";
        case { X: < 0, Y: > 0 }:
            return "Second quadrant";
        case { X: < 0, Y: < 0 }:
            return "Third quadrant";
        case { X: > 0, Y: < 0 }:
            return "Fourth quadrant";
        default:
            return "On an axis";
    }
}

// Equivalent switch expression
string GetQuadrantExpression(Point point) => point switch
{
    { X: > 0, Y: > 0 } => "First quadrant",
    { X: < 0, Y: > 0 } => "Second quadrant",
    { X: < 0, Y: < 0 } => "Third quadrant",
    { X: > 0, Y: < 0 } => "Fourth quadrant",
    _ => "On an axis"
};
```

The switch expression is:
1. More concise and readable
2. Expression-based, so it returns a value directly
3. Forces exhaustiveness checking
4. Eliminates the need for `break` statements and the risk of fall-through bugs

## Performance considerations

Switch expressions are generally compiled to efficient code similar to switch statements. The compiler can optimize simple constant-based switch expressions into jump tables or binary searches, depending on the pattern of values.

For more complex patterns, especially those with case guards, there may be some performance overhead compared to hand-optimized conditional logic. However, the readability and maintainability benefits usually outweigh these minor performance considerations.

## Key Points

- Switch expressions evaluate a single expression based on pattern matching against an input expression
- They are expression-based, not statement-based, meaning they return a value
- Switch expressions support various patterns: constant, type, property, tuple, and discard patterns
- Case guards with the `when` keyword provide additional filtering capabilities
- The compiler performs exhaustiveness checking to ensure all possible input values are handled
- If no pattern matches at runtime, a `SwitchExpressionException` is thrown
- Switch expressions are more concise and less error-prone than traditional switch statements
- The arms are evaluated in text order, so the order matters

## FAQs

**Q: When were switch expressions introduced in C#?**  
A: Switch expressions were introduced in C# 8.0, which was released with .NET Core 3.0 in September 2019.

**Q: Can I use switch expressions in older versions of C#?**  
A: No, switch expressions require C# 8.0 or later. If you're working with an older version, you'll need to use switch statements or if-else chains instead.

**Q: How do switch expressions handle null values?**  
A: Switch expressions can handle null values using the constant pattern `null` or the pattern `null =>`. For example: `object obj = null; string result = obj switch { null => "It's null", _ => "It's not null" };`

**Q: Can I use multiple patterns in a single arm?**  
A: Yes, in C# 9.0 and later, you can use pattern combinators like `and`, `or`, and `not` to combine patterns. For example: `shape switch { Circle c and { Radius: > 10 } => "Large circle", _ => "Other shape" }`

**Q: Is there a limit to how many arms a switch expression can have?**  
A: There's no practical limit imposed by the language, but for readability and maintainability, it's generally best to keep the number of arms reasonable. If you find yourself with a very large number of arms, consider refactoring your code.

**Q: Can switch expressions be nested?**  
A: Yes, you can nest switch expressions within other switch expressions, though this can quickly become hard to read. Consider extracting nested switch expressions into separate methods if they become too complex.

**Q: How do I handle multiple values that should produce the same result?**  
A: In C# 9.0 and later, you can use the `or` pattern combinator: `day switch { DayOfWeek.Saturday or DayOfWeek.Sunday => "weekend", _ => "weekday" }`. In C# 8.0, you would need separate arms for each value.