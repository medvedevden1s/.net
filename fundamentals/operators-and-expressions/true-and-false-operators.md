## Introduction

The `true` and `false` operators are like judges in a courtroom who can definitively answer yes or no questions. Just as a judge evaluates evidence to determine if a statement is true or false, these operators evaluate an object to determine if it should be considered definitely true or definitely false. They provide a way for your custom types to participate in logical operations and conditional statements, allowing them to be used in contexts where a boolean decision is needed.

In the world of programming, we often need to make decisions based on whether something is true or false. While primitive types like `bool` have clear true and false values, custom types might have more complex criteria for what constitutes "truthiness" or "falseness." The `true` and `false` operators bridge this gap, allowing you to define custom logic that determines when your type should be treated as true or false in boolean contexts.

## How the `true` and `false` operators work

The `true` operator returns the `bool` value `true` to indicate that its operand is definitely true, while the `false` operator returns the `bool` value `true` to indicate that its operand is definitely false.

This might seem confusing at first, so let's clarify:

- When you ask "Is this object true?", the `true` operator is called. If it returns `true`, the answer is "Yes, this object is true."
- When you ask "Is this object false?", the `false` operator is called. If it returns `true`, the answer is "Yes, this object is false."

A type that implements both operators must follow these semantics. It's important to note that the `true` and `false` operators aren't guaranteed to complement each other. That is, both operators might return the `bool` value `false` for the same operand, indicating that the object is neither definitely true nor definitely false.

If a type defines one of these two operators, it must also define the other operator.

```csharp
public struct LaunchStatus
{
    public static readonly LaunchStatus Green = new LaunchStatus(0);
    public static readonly LaunchStatus Yellow = new LaunchStatus(1);
    public static readonly LaunchStatus Red = new LaunchStatus(2);

    private int status;

    private LaunchStatus(int status)
    {
        this.status = status;
    }

    public static bool operator true(LaunchStatus x) => x == Green || x == Yellow;
    public static bool operator false(LaunchStatus x) => x == Red;

    public static LaunchStatus operator &(LaunchStatus x, LaunchStatus y)
    {
        if (x == Red || y == Red || (x == Yellow && y == Yellow))
        {
            return Red;
        }

        if (x == Yellow || y == Yellow)
        {
            return Yellow;
        }

        return Green;
    }

    public static bool operator ==(LaunchStatus x, LaunchStatus y) => x.status == y.status;
    public static bool operator !=(LaunchStatus x, LaunchStatus y) => !(x == y);

    public override bool Equals(object obj) => obj is LaunchStatus other && this == other;
    public override int GetHashCode() => status;
}
```

In this example, we define a `LaunchStatus` struct with three possible states: Green, Yellow, and Red. We implement the `true` operator to return `true` when the status is Green or Yellow, and the `false` operator to return `true` when the status is Red.

## Using types with `true` and `false` operators

Types that define the `true` and `false` operators can be used in boolean expressions, such as in conditional statements:

```csharp
public class LaunchStatusTest
{
    public static void Main()
    {
        LaunchStatus okToLaunch = GetFuelLaunchStatus() && GetNavigationLaunchStatus();
        Console.WriteLine(okToLaunch ? "Ready to go!" : "Wait!");
    }

    static LaunchStatus GetFuelLaunchStatus()
    {
        Console.WriteLine("Getting fuel launch status...");
        return LaunchStatus.Red;
    }

    static LaunchStatus GetNavigationLaunchStatus()
    {
        Console.WriteLine("Getting navigation launch status...");
        return LaunchStatus.Yellow;
    }
}
```

When this code runs, it first calls `GetFuelLaunchStatus()`, which returns `LaunchStatus.Red`. Since `LaunchStatus.Red` is definitely false (the `false` operator returns `true`), the right-hand operand of the `&&` operator isn't evaluated. This demonstrates the short-circuiting behavior of the conditional logical operators. The output is:

```
Getting fuel launch status...
Wait!
```

## Boolean expressions and conditional logical operators

A type with the defined `true` operator can be used in controlling conditional expressions in the `if`, `do`, `while`, and `for` statements and in the conditional operator `?:`. This allows your custom types to be used directly in conditions without explicitly comparing them to something.

If a type with defined `true` and `false` operators overloads the logical OR operator `|` or the logical AND operator `&` in a certain way, the conditional logical OR operator `||` or conditional logical AND operator `&&`, respectively, can be evaluated for the operands of that type. This enables short-circuit evaluation for your custom types.

{% hint style="info" %}
The `bool?` type (nullable boolean) is a good example of a type that supports three-valued logic. It can be `true`, `false`, or `null`, which is useful when working with databases that have a three-valued boolean type. C# provides the `&` and `|` operators that support this three-valued logic with `bool?` operands.
{% endhint %}

## Three-valued logic with nullable booleans

While not directly related to the `true` and `false` operators, it's worth mentioning that C# supports three-valued logic through the `bool?` type:

```csharp
bool? a = null;
bool? b = true;

// Three-valued logic with & and |
bool? result1 = a & b;  // null
bool? result2 = a | b;  // true

// Conditional operators still work with bool?
if (b == true)  // This works because b is true
{
    Console.WriteLine("b is true");
}

if (a == true)  // This doesn't execute because a is null
{
    Console.WriteLine("a is true");
}
```

## Implementation considerations

When implementing the `true` and `false` operators for your type, consider the following:

1. Always implement both operators if you implement one
2. Make sure your implementation follows the expected semantics
3. Consider implementing the logical operators (`&`, `|`) to enable conditional logical operators (`&&`, `||`)
4. Ensure your implementation is consistent with other aspects of your type, such as equality

Here's a more complete example showing how you might implement a type with three-valued logic:

```csharp
public struct ThreeValuedLogic
{
    private readonly int? _value;  // null, 0, or 1

    private ThreeValuedLogic(int? value)
    {
        _value = value;
    }

    public static readonly ThreeValuedLogic True = new ThreeValuedLogic(1);
    public static readonly ThreeValuedLogic False = new ThreeValuedLogic(0);
    public static readonly ThreeValuedLogic Unknown = new ThreeValuedLogic(null);

    // True operator returns true only if definitely true
    public static bool operator true(ThreeValuedLogic x) => x._value == 1;
    
    // False operator returns true only if definitely false
    public static bool operator false(ThreeValuedLogic x) => x._value == 0;

    // Logical AND
    public static ThreeValuedLogic operator &(ThreeValuedLogic x, ThreeValuedLogic y)
    {
        if (x._value == 0 || y._value == 0)
            return False;
        if (x._value == null || y._value == null)
            return Unknown;
        return True;
    }

    // Logical OR
    public static ThreeValuedLogic operator |(ThreeValuedLogic x, ThreeValuedLogic y)
    {
        if (x._value == 1 || y._value == 1)
            return True;
        if (x._value == null || y._value == null)
            return Unknown;
        return False;
    }

    // For display purposes
    public override string ToString()
    {
        if (_value == null) return "Unknown";
        return _value == 1 ? "True" : "False";
    }
}
```

With this implementation, you can use the type in conditional expressions and benefit from short-circuit evaluation:

```csharp
ThreeValuedLogic a = ThreeValuedLogic.True;
ThreeValuedLogic b = ThreeValuedLogic.Unknown;
ThreeValuedLogic c = ThreeValuedLogic.False;

// Short-circuit evaluation with &&
if (c && a)  // c is definitely false, so a is not evaluated
{
    Console.WriteLine("This won't execute");
}

// Short-circuit evaluation with ||
if (a || b)  // a is definitely true, so b is not evaluated
{
    Console.WriteLine("This will execute");
}
```

## Key Points

- The `true` operator returns `true` to indicate that its operand is definitely true
- The `false` operator returns `true` to indicate that its operand is definitely false
- If a type implements one of these operators, it must implement both
- The operators aren't guaranteed to complement each other; both can return `false` for the same operand
- Types with these operators can be used in boolean expressions and conditional statements
- When combined with overloaded logical operators, they enable short-circuit evaluation with `&&` and `||`
- The `bool?` type provides built-in support for three-valued logic

## FAQs

**Q: Why would I implement the `true` and `false` operators for my type?**  
A: These operators allow your type to be used directly in conditional expressions, making your code more readable and expressive. They're particularly useful for types that represent some form of state or condition that naturally maps to boolean logic.

**Q: Can I implement just one of the operators?**  
A: No, if you implement either the `true` or `false` operator, you must implement both. This is a requirement of the C# language.

**Q: How do these operators relate to the `bool` conversion operators?**  
A: They're different. The `true` and `false` operators are used in boolean expressions, while explicit or implicit conversion operators to `bool` are used when you need to convert your type to a boolean value. In many cases, you might want to implement both.

**Q: Can the `true` and `false` operators return values other than `true` or `false`?**  
A: No, these operators must return a `bool` value. The name of the operator indicates when it should return `true`.

**Q: How do I ensure short-circuit evaluation works with my type?**  
A: Implement the `true` and `false` operators, and also implement the logical AND (`&`) and logical OR (`|`) operators. The compiler will then enable the conditional logical operators (`&&` and `||`) for your type.

**Q: Are there any built-in types that use these operators?**  
A: The `bool?` (nullable boolean) type effectively uses this concept to support three-valued logic, although it's implemented differently in the runtime.

**Q: Can I use these operators to implement a type with more than three logical states?**  
A: Yes, you can implement a type with any number of states, but in boolean contexts, they'll be reduced to "definitely true," "definitely false," or "neither." This is similar to how fuzzy logic or multi-valued logic systems work.