## Introduction

The `is` operator is like a detective in your code. Just as a detective examines evidence to determine if it matches certain characteristics, the `is` operator examines an object to determine if it matches a specific type or pattern. It's a powerful tool that helps your code make decisions based on what kind of data it's dealing with.

In the world of programming, objects often need to be identified, categorized, or validated before they can be properly handled. The `is` operator provides a clean, expressive way to perform these checks, making your code both safer and more readable by eliminating the need for complex conditional logic or type casting.

## The `is` operator for type checking

The most basic use of the `is` operator is to check if an object is compatible with a given type:

```csharp
int i = 34;
object iBoxed = i;
int? jNullable = 42;

if (iBoxed is int a && jNullable is int b)
{
    Console.WriteLine(a + b);  // output: 76
}
```

In this example, the `is` operator checks if `iBoxed` is an `int` and if `jNullable` is an `int`. If both conditions are true, the values are assigned to the variables `a` and `b` respectively, which can then be used in the code block.

This pattern is called a declaration pattern, as it declares new variables (`a` and `b`) that are assigned the values if the type check succeeds.

{% hint style="info" %}
The `is` operator performs a check without throwing exceptions, unlike direct casting with the cast operator `()` which can throw an `InvalidCastException` if the cast fails.
{% endhint %}

## Null checking with the `is` operator

The `is` operator provides a clean way to check for null:

```csharp
if (input is null)
{
    return;
}
```

This is equivalent to `if (input == null)`, but with an important difference: when you use the `is` operator with `null`, the compiler guarantees that no user-overloaded `==` or `!=` operator is invoked. This makes the code more predictable and safer.

Starting with C# 9.0, you can use a negation pattern to perform a non-null check:

```csharp
if (result is not null)
{
    Console.WriteLine(result.ToString());
}
```

This is more readable than the traditional `if (result != null)` approach.

## Pattern matching with the `is` operator

The `is` operator really shines when combined with pattern matching, allowing you to check not just the type of an object, but also its properties and values.

### Property patterns

You can check if an object has specific property values:

```csharp
static bool IsFirstFridayOfOctober(DateTime date) =>
    date is { Month: 10, Day: <=7, DayOfWeek: DayOfWeek.Friday };
```

This example checks if a `DateTime` object represents the first Friday of October by examining its `Month`, `Day`, and `DayOfWeek` properties.

### List patterns

Starting with C# 11, you can use list patterns to match elements of a list or array:

```csharp
int[] empty = [];
int[] one = [1];
int[] odd = [1, 3, 5];
int[] even = [2, 4, 6];
int[] fib = [1, 1, 2, 3, 5];

Console.WriteLine(odd is [1, _, 2, ..]);   // false
Console.WriteLine(fib is [1, _, 2, ..]);   // true
Console.WriteLine(fib is [_, 1, 2, 3, ..]);     // true
Console.WriteLine(fib is [.., 1, 2, 3, _ ]);     // true
Console.WriteLine(even is [2, _, 6]);     // true
Console.WriteLine(even is [2, .., 6]);    // true
Console.WriteLine(odd is [.., 3, 5]); // true
Console.WriteLine(even is [.., 3, 5]); // false
Console.WriteLine(fib is [.., 3, 5]); // true
```

In list patterns:
- `_` is a discard pattern that matches any single element
- `..` is a slice pattern that matches zero or more elements
- You can combine these with constant patterns to match specific values

## Advanced pattern matching

The `is` operator supports a wide range of patterns beyond what we've covered:

### Constant patterns

```csharp
if (obj is 100)
{
    Console.WriteLine("The value is 100");
}
```

### Type patterns with discard

```csharp
if (obj is string _)
{
    Console.WriteLine("It's a string, but we don't need its value");
}
```

### Relational patterns

```csharp
if (temperature is > 100)
{
    Console.WriteLine("It's boiling!");
}
```

### Logical patterns (and, or, not)

```csharp
if (number is > 0 and < 100)
{
    Console.WriteLine("Number is between 1 and 99");
}
```

## Performance considerations

The `is` operator is generally efficient, but there are some performance considerations:

- Type checking with `is` is faster than using `typeof` and `GetType()` comparisons
- Pattern matching with `is` can be more efficient than multiple separate checks
- When using declaration patterns, the variable is only assigned if the check succeeds, avoiding unnecessary assignments

{% hint style="warning" %}
Excessive use of pattern matching can make code harder to read. Use it judiciously and consider breaking complex patterns into smaller, more readable pieces.
{% endhint %}

## Key Points

- The `is` operator checks if an expression is compatible with a given type or pattern
- It can be used for type checking, null checking, and pattern matching
- Declaration patterns allow you to declare variables that are assigned if the check succeeds
- The `is` operator with `null` bypasses any overloaded equality operators
- C# 9.0 introduced the `not` pattern for more readable negation
- C# 11 added list patterns for matching elements in arrays and collections
- Pattern matching with `is` can make complex conditional logic more readable and maintainable

## FAQs

**Q: What's the difference between the `is` operator and the `as` operator?**  
A: The `is` operator returns a boolean indicating whether an object is compatible with a type, while the `as` operator attempts to cast an object to a specified type and returns null if the cast fails. The `is` operator can also be used with patterns, which the `as` operator cannot.

**Q: Can the `is` operator be used with value types?**  
A: Yes, the `is` operator works with both reference types and value types. For value types, it can check if a value is of a specific type or matches a specific pattern.

**Q: Is there a performance penalty for using pattern matching with the `is` operator?**  
A: Pattern matching is generally efficient, but complex patterns might introduce some overhead compared to simple type checks. In most cases, the readability benefits outweigh any minor performance considerations.

**Q: Can I use the `is` operator with interfaces?**  
A: Yes, you can use the `is` operator to check if an object implements a specific interface.

**Q: How does the `is` operator handle inheritance?**  
A: The `is` operator returns true if the object is an instance of the specified type or any derived type. For example, if `Dog` inherits from `Animal`, then a `Dog` object will return true when checked with `is Animal`.

**Q: Can I use the `is` operator with generic types?**  
A: Yes, you can use the `is` operator with generic types, but due to type erasure, you cannot check for specific type arguments at runtime without additional work.

**Q: Is the `is` operator null-safe?**  
A: Yes, the `is` operator is null-safe. If the object being checked is null, the `is` operator will return false (unless you're explicitly checking for null with `is null`).