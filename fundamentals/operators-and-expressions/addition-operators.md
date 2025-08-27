## Introduction

Addition operators in C# are like the basic arithmetic you learned in school, but with superpowers. Just as you can add numbers together, C# lets you combine values using the `+` and `+=` operators. But these operators go beyond simple arithmetic - they can join strings together like linking words in a sentence, or combine delegates like merging two recipes into one.

These versatile operators adapt their behavior based on what you're working with, making them fundamental tools in your C# toolkit.

## Addition operator (+)

The addition operator (`+`) is one of the most commonly used operators in C#. Its behavior depends on the types of its operands.

```csharp
// Numeric addition with integers
int sum = 5 + 3;  // Result: 8

// Numeric addition with floating-point numbers
double total = 3.14 + 2.71;  // Result: 5.85

// Character addition (adds the numeric values)
char c1 = 'A';  // ASCII value 65
char c2 = (char)(c1 + 1);  // Result: 'B' (ASCII value 66)
```

For numeric types (integers, floating-point), the `+` operator performs standard arithmetic addition, returning the sum of the operands.

{% hint style="info" %}
The `+` operator can also be used as a unary operator to indicate a positive value, as in `+5`. This is covered in the Arithmetic operators article.
{% endhint %}

## String concatenation

When one or both operands are strings, the `+` operator performs concatenation, joining the strings together.

```csharp
// Basic string concatenation
string firstName = "John";
string lastName = "Doe";
string fullName = firstName + " " + lastName;  // Result: "John Doe"

// Mixing strings with other types
int age = 30;
string message = "Age: " + age;  // Result: "Age: 30"

// Concatenating with null
string nullString = null;
string result = nullString + " is null";  // Result: " is null"
```

The string concatenation operator automatically converts non-string operands to their string representation. The string representation of `null` is an empty string.

{% hint style="warning" %}
Repeatedly concatenating strings with the `+` operator in a loop can be inefficient. For better performance with multiple concatenations, use `StringBuilder` instead.
{% endhint %}

String interpolation provides a more readable alternative to concatenation:

```csharp
// Using string interpolation instead of concatenation
string name = "Alice";
int score = 95;
string result = $"{name} scored {score} points";  // Result: "Alice scored 95 points"

// Formatting in string interpolation
double pi = Math.PI;
string formatted = $"Pi rounded to 2 decimals: {pi:F2}";  // Result: "Pi rounded to 2 decimals: 3.14"
```

## Delegate combination

For delegate types, the `+` operator combines two delegates, creating a new delegate that invokes both in sequence.

```csharp
// Defining simple delegates
Action sayHello = () => Console.WriteLine("Hello");
Action sayWorld = () => Console.WriteLine("World");

// Combining delegates with the + operator
Action sayHelloWorld = sayHello + sayWorld;

// Invoking the combined delegate
sayHelloWorld();  // Output:
                  // Hello
                  // World

// Handling null delegates
Action nullAction = null;
Action safeAction = nullAction + sayHello;  // Result: equivalent to just sayHello
safeAction();  // Output: Hello
```

When you combine delegates, the resulting delegate invokes the left-hand operand first, followed by the right-hand operand. If either operand is `null`, the result is the non-null operand (which might also be `null` if both are `null`).

{% hint style="success" %}
Delegate combination is particularly useful for event handling, allowing multiple subscribers to respond to a single event.
{% endhint %}

## Addition assignment operator (+=)

The addition assignment operator (`+=`) combines addition and assignment in a single operation. It adds the right operand to the left operand and assigns the result back to the left operand.

```csharp
// Numeric addition assignment
int count = 5;
count += 3;  // Equivalent to: count = count + 3;
Console.WriteLine(count);  // Output: 8

// String concatenation assignment
string message = "Hello";
message += " World";  // Equivalent to: message = message + " World";
Console.WriteLine(message);  // Output: Hello World

// Delegate combination assignment
Action log = () => Console.WriteLine("Log entry");
Action timestamp = () => Console.WriteLine($"Time: {DateTime.Now}");
log += timestamp;  // Combines the delegates
log();  // Output:
        // Log entry
        // Time: [current timestamp]
```

The `+=` operator is not just a shorthand; it evaluates the left operand only once, which can be important when the left operand is a complex expression.

{% hint style="info" %}
The `+=` operator is commonly used to subscribe to events in C#:
```csharp
button.Click += Button_Click;  // Subscribe to the Click event
```
{% endhint %}

## Operator overloadability

User-defined types can overload the `+` operator to define custom addition behavior. When a binary `+` operator is overloaded, the `+=` operator is also implicitly overloaded.

```csharp
// A simple Vector class with overloaded + operator
public struct Vector2D
{
    public double X { get; }
    public double Y { get; }

    public Vector2D(double x, double y)
    {
        X = x;
        Y = y;
    }

    // Overloading the + operator
    public static Vector2D operator +(Vector2D a, Vector2D b)
    {
        return new Vector2D(a.X + b.X, a.Y + b.Y);
    }
}

// Using the overloaded operator
Vector2D v1 = new Vector2D(1, 2);
Vector2D v2 = new Vector2D(3, 4);
Vector2D sum = v1 + v2;  // Result: Vector2D(4, 6)
```

Beginning with C# 14, a user-defined type can explicitly overload the `+=` operator to provide a more efficient implementation, typically when the value can be updated in place rather than allocating a new instance.

```csharp
public class StringBuilder
{
    // Simplified example
    private char[] _buffer;
    private int _position;

    // Overloaded + operator creates a new instance
    public static StringBuilder operator +(StringBuilder sb, string value)
    {
        StringBuilder result = new StringBuilder(sb);
        result.Append(value);
        return result;
    }

    // Overloaded += operator modifies in place (C# 14)
    public static StringBuilder operator +=(StringBuilder sb, string value)
    {
        sb.Append(value);
        return sb;
    }
}
```

If a type doesn't provide an explicit overload for `+=`, the compiler generates an implicit overload based on the `+` operator.

## Key Points

- The `+` operator performs addition for numeric types, concatenation for strings, and combination for delegates
- String concatenation automatically converts non-string operands to their string representation
- The string representation of `null` is an empty string
- For delegates, the `+` operator creates a new delegate that invokes both operands in sequence
- The `+=` operator combines addition and assignment in a single operation
- User-defined types can overload the `+` operator to define custom addition behavior
- When the `+` operator is overloaded, the `+=` operator is also implicitly overloaded
- Beginning with C# 14, types can explicitly overload the `+=` operator for more efficient implementations

## FAQs

**Q: What happens if I try to add values of different numeric types?**  
A: C# follows type conversion rules. The smaller type is usually converted to the larger type before addition (e.g., adding an `int` to a `double` converts the `int` to `double` first).

**Q: Is string concatenation using `+` efficient for multiple operations?**  
A: No, repeatedly using `+` for string concatenation creates many intermediate string objects. For multiple concatenations, use `StringBuilder` or string interpolation.

**Q: What's the difference between `+` and `+=` for delegates?**  
A: Functionally, they're similar - both combine delegates. However, `+=` modifies the original delegate variable, while `+` creates a new delegate without changing the operands.

**Q: Can I use `+` to combine any two delegates?**  
A: The delegates must be of the same type. You can't combine delegates with different signatures (e.g., an `Action` with a `Func<int>`).

**Q: What happens if I add a null value to a string?**  
A: The null value is treated as an empty string, so `"Hello" + null` results in `"Hello"`.

**Q: How do I remove a delegate that was added with `+=`?**  
A: Use the `-=` operator to remove a delegate from a multicast delegate.