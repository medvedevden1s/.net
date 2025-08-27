## Introduction

Subtraction operators in C# are like taking away items from a collection or stepping backward on a number line. Just as you can remove apples from a basket, C# lets you decrease values using the `-` and `-=` operators. But these operators go beyond simple arithmetic - they can remove delegates from a multicast delegate, similar to removing a person from a team while keeping the rest of the team intact.

These versatile operators adapt their behavior based on what you're working with, making them essential tools in your C# programming toolkit.

## Unary minus operator (-)

The unary minus operator (`-`) negates its operand, effectively changing its sign.

```csharp
// Negating integer values
int positive = 42;
int negative = -positive;  // Result: -42

// Negating floating-point values
double pi = 3.14159;
double negativePi = -pi;  // Result: -3.14159

// Negating already negative values
int alreadyNegative = -10;
int backToPositive = -alreadyNegative;  // Result: 10
```

The unary minus operator works with all built-in numeric types, both integer and floating-point.

{% hint style="info" %}
The unary minus operator is different from the subtraction operator, even though they use the same symbol.
{% endhint %}

## Subtraction operator (-)

The subtraction operator (`-`) is used to subtract one value from another, returning the difference.

```csharp
// Integer subtraction
int difference = 10 - 3;  // Result: 7

// Floating-point subtraction
double result = 5.5 - 2.2;  // Result: 3.3

// Mixed type subtraction (follows type conversion rules)
double mixed = 10 - 3.5;  // Result: 6.5 (int 10 is converted to double)
```

For numeric types (integers, floating-point), the `-` operator performs standard arithmetic subtraction, returning the difference between the operands.

## Delegate removal

For delegate types, the `-` operator removes the right operand from the left operand, creating a new delegate.

```csharp
// Defining simple delegates
Action a = () => Console.Write("a");
Action b = () => Console.Write("b");

// Creating a multicast delegate
var abbaab = a + b + b + a + a + b;
abbaab();  // Output: abbaab

// Removing a delegate
var ab = a + b;
var abba = abbaab - ab;
abba();  // Output: abba

// Removing a delegate completely
var nihil = abbaab - abbaab;
Console.WriteLine(nihil is null);  // Output: True
```

When you remove delegates, the resulting delegate invokes only the methods that remain after removal. If the removal results in an empty invocation list, the result is `null`.

{% hint style="warning" %}
Delegate removal only works if the right-hand operand's invocation list is a proper contiguous sublist of the left-hand operand's invocation list. If not, the left-hand operand is returned unchanged.
{% endhint %}

```csharp
// Defining delegates
Action a = () => Console.Write("a");
Action b = () => Console.Write("b");

// Creating multicast delegates
var abbaab = a + b + b + a + a + b;
var aba = a + b + a;

// Attempting to remove a non-contiguous sublist
var first = abbaab - aba;
first();  // Output: abbaab (unchanged)
Console.WriteLine(object.ReferenceEquals(abbaab, first));  // Output: True

// Delegate instances are compared, not just their targets
Action a2 = () => Console.Write("a");  // Same code but different instance
var changed = aba - a;
changed();  // Output: ab
var unchanged = aba - a2;
unchanged();  // Output: aba (unchanged)
Console.WriteLine(object.ReferenceEquals(aba, unchanged));  // Output: True
```

If the left-hand operand is `null`, the result is `null`. If the right-hand operand is `null`, the result is the left-hand operand.

```csharp
Action a = () => Console.Write("a");

// Removing from null
var nothing = null - a;
Console.WriteLine(nothing is null);  // Output: True

// Removing null from a delegate
var first = a - null;
a();  // Output: a
Console.WriteLine(object.ReferenceEquals(first, a));  // Output: True
```

{% hint style="success" %}
To combine delegates, use the `+` operator. The combination and removal of delegates is particularly useful for event handling in C#.
{% endhint %}

## Subtraction assignment operator (-=)

The subtraction assignment operator (`-=`) combines subtraction and assignment in a single operation. It subtracts the right operand from the left operand and assigns the result back to the left operand.

```csharp
// Numeric subtraction assignment
int count = 10;
count -= 3;  // Equivalent to: count = count - 3;
Console.WriteLine(count);  // Output: 7

// Delegate removal assignment
Action a = () => Console.Write("a");
Action b = () => Console.Write("b");
var printer = a + b + a;
printer();  // Output: aba

Console.WriteLine();
printer -= a;  // Removes the last occurrence of a
printer();  // Output: ab
```

The `-=` operator is not just a shorthand; it evaluates the left operand only once, which can be important when the left operand is a complex expression.

{% hint style="info" %}
The `-=` operator is commonly used to unsubscribe from events in C#:
```csharp
button.Click -= Button_Click;  // Unsubscribe from the Click event
```
{% endhint %}

## Operator overloadability

User-defined types can overload the `-` operator to define custom subtraction behavior. When a binary `-` operator is overloaded, the `-=` operator is also implicitly overloaded.

```csharp
// A simple Vector class with overloaded - operator
public struct Vector2D
{
    public double X { get; }
    public double Y { get; }

    public Vector2D(double x, double y)
    {
        X = x;
        Y = y;
    }

    // Overloading the - operator
    public static Vector2D operator -(Vector2D a, Vector2D b)
    {
        return new Vector2D(a.X - b.X, a.Y - b.Y);
    }
}

// Using the overloaded operator
Vector2D v1 = new Vector2D(5, 7);
Vector2D v2 = new Vector2D(2, 3);
Vector2D difference = v1 - v2;  // Result: Vector2D(3, 4)
```

Beginning with C# 14, a user-defined type can explicitly overload the `-=` operator to provide a more efficient implementation, typically when the value can be updated in place rather than allocating a new instance.

```csharp
public class Counter
{
    private int _value;

    public Counter(int initialValue)
    {
        _value = initialValue;
    }

    // Overloaded - operator creates a new instance
    public static Counter operator -(Counter counter, int amount)
    {
        return new Counter(counter._value - amount);
    }

    // Overloaded -= operator modifies in place (C# 14)
    public static Counter operator -=(Counter counter, int amount)
    {
        counter._value -= amount;
        return counter;
    }
}
```

If a type doesn't provide an explicit overload for `-=`, the compiler generates an implicit overload based on the `-` operator.

## Key Points

- The unary `-` operator negates its operand, changing its sign
- The binary `-` operator performs subtraction for numeric types and removal for delegates
- For delegates, the `-` operator removes the right-hand operand from the left-hand operand
- Delegate removal only works if the right-hand operand is a proper contiguous sublist of the left-hand operand
- The `-=` operator combines subtraction and assignment in a single operation
- User-defined types can overload the `-` operator to define custom subtraction behavior
- When the `-` operator is overloaded, the `-=` operator is also implicitly overloaded
- Beginning with C# 14, types can explicitly overload the `-=` operator for more efficient implementations

## FAQs

**Q: What happens if I try to subtract values of different numeric types?**  
A: C# follows type conversion rules. The smaller type is usually converted to the larger type before subtraction (e.g., subtracting a `double` from an `int` converts the `int` to `double` first).

**Q: Can I use the unary minus operator with unsigned types?**  
A: Yes, but the result is interpreted according to the rules of two's complement representation. For example, `-1u` (negating an unsigned 1) results in the maximum value for `uint`.

**Q: What's the difference between `-` and `-=` for delegates?**  
A: Functionally, they're similar - both remove delegates. However, `-=` modifies the original delegate variable, while `-` creates a new delegate without changing the operands.

**Q: What happens if I try to remove a delegate that doesn't exist in the multicast delegate?**  
A: If the right-hand operand's invocation list is not a proper contiguous sublist of the left-hand operand's invocation list, the result is the unchanged left-hand operand.

**Q: How does delegate equality work during removal?**  
A: Delegates are compared by instance, not just by their targets. Two delegates created from identical lambda expressions are not considered equal.

**Q: Can I use the subtraction operator with strings like I can with the addition operator?**  
A: No, the subtraction operator is not defined for strings. You cannot use `-` to remove characters or substrings from a string.