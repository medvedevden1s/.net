## Introduction

Operator overloading is like teaching a foreign language to a calculator. Just as a calculator understands basic operations like addition and subtraction for numbers, C# understands these operations for built-in types. But what if you want your calculator to "add" two colors, "multiply" a recipe, or "compare" two custom measurements? Operator overloading lets you teach the language of your custom types to the C# compiler, allowing it to understand how to perform standard operations on your objects.

In the world of programming, operators provide a concise and intuitive way to express common operations. When you write `a + b`, the meaning is immediately clear for built-in types. Operator overloading extends this clarity and expressiveness to your own types, making your code more readable and natural. Instead of writing `point1.Add(point2)`, you can write `point1 + point2`, which better reflects the conceptual operation being performed.

## How operator overloading works

A user-defined type can overload a predefined C# operator. That is, a type can provide the custom implementation of an operation in case one or both of the operands are of that type.

To overload an operator, you declare an operator method in your type using the `operator` keyword. An operator declaration must satisfy the following rules:

- It includes a `public` modifier.
- A unary operator has one input parameter. A binary operator has two input parameters. In each case, at least one parameter must have type T or T? where T is the type that contains the operator declaration.
- It includes the `static` modifier, except for the compound assignment operators, such as `+=`.
- The increment (`++`) and decrement (`--`) operators can be implemented as either static or instance methods. Instance method operators are a new feature introduced in C# 14.

Here's a simple example of a structure that represents a rational number and overloads some arithmetic operators:

```csharp
public struct Fraction
{
    private int numerator;
    private int denominator;

    public Fraction(int numerator, int denominator)
    {
        if (denominator == 0)
        {
            throw new ArgumentException("Denominator cannot be zero.", nameof(denominator));
        }
        this.numerator = numerator;
        this.denominator = denominator;
    }

    public static Fraction operator +(Fraction operand) => operand;
    public static Fraction operator -(Fraction operand) => new Fraction(-operand.numerator, operand.denominator);

    public static Fraction operator +(Fraction left, Fraction right)
        => new Fraction(left.numerator * right.denominator + right.numerator * left.denominator, left.denominator * right.denominator);

    public static Fraction operator -(Fraction left, Fraction right)
        => left + (-right);

    public static Fraction operator *(Fraction left, Fraction right)
        => new Fraction(left.numerator * right.numerator, left.denominator * right.denominator);

    public static Fraction operator /(Fraction left, Fraction right)
    {
        if (right.numerator == 0)
        {
            throw new DivideByZeroException();
        }
        return new Fraction(left.numerator * right.denominator, left.denominator * right.numerator);
    }

    // Define increment and decrement to add 1/den, rather than 1/1.
    public static Fraction operator ++(Fraction operand)
        => new Fraction(operand.numerator++, operand.denominator);

    public static Fraction operator --(Fraction operand) =>
        new Fraction(operand.numerator--, operand.denominator);

    public override string ToString() => $"{numerator} / {denominator}";

    // New operators allowed in C# 14:
    public void operator +=(Fraction operand) =>
        (numerator, denominator ) =
        (
            numerator * operand.denominator + operand.numerator * denominator,
            denominator * operand.denominator
        );

    public void operator -=(Fraction operand) =>
        (numerator, denominator) =
        (
            numerator * operand.denominator - operand.numerator * denominator,
            denominator * operand.denominator
        );

    public void operator *=(Fraction operand) =>
        (numerator, denominator) =
        (
            numerator * operand.numerator,
            denominator * operand.denominator
        );

    public void operator /=(Fraction operand)
    {
        if (operand.numerator == 0)
        {
            throw new DivideByZeroException();
        }
        (numerator, denominator) =
        (
            numerator * operand.denominator,
            denominator * operand.numerator
        );
    }

    public void operator ++() => numerator++;

    public void operator --() => numerator--;
}
```

You could extend this example by defining an implicit conversion from `int` to `Fraction`. Then, overloaded operators would support arguments of those two types. That is, it would become possible to add an integer to a fraction and obtain a fraction as a result.

{% hint style="info" %}
You also use the `operator` keyword to define a custom type conversion. For more information, see [User-defined conversion operators](user-defined-explicit-and-implicit-conversion-operators.md).
{% endhint %}

## Overloadable operators

The following table shows the operators that can be overloaded:

| Operators | Notes |
| -------- | -------- |
| `+x`, `-x`, `!x`, `~x`, `++`, `--`, `true`, `false` | The `true` and `false` operators must be overloaded together. |
| `x + y`, `x - y`, `x * y`, `x / y`, `x % y`, `x & y`, `x \| y`, `x ^ y`, `x << y`, `x >> y`, `x >>> y` |  |
| `x == y`, `x != y`, `x < y`, `x > y`, `x <= y`, `x >= y` | Must be overloaded in pairs as follows: `==` and `!=`, `<` and `>`, `<=` and `>=`. |
| `+=`, `-=`, `*=`, `/=`, `%=`, `&=`, `\|=`, `^=`, `<<=`, `>>=`, `>>>=` | The compound assignment operators can be overloaded in C# 14 and later. |

A compound assignment overloaded operator must follow these rules:

- It must include the `public` modifier.
- It can't include the `static` modifier.
- The return type must be `void`.
- The declaration must include one parameter, which represents the right hand side of the compound assignment.

Beginning with C# 14, the increment (`++`) and decrement (`--`) operators can be overloaded as instance members. Instance operators can improve performance by avoiding the creation of a new instance. An instance operator must follow these rules:

- It must include the `public` modifier.
- It can't include the `static` modifier.
- The return type must be `void`.
- It can't declare any parameters, even if those parameters have a default value.

## Non-overloadable operators

The following table shows the operators that can't be overloaded:

| Operators | Alternatives |
| -------- | -------- |
| `x && y`, `x \|\| y` | Overload both the `true` and `false` operators and the `&` or `\|` operators. For more information, see [User-defined conditional logical operators](true-and-false-operators.md). |
| `a[i]`, `a?[i]` | Define an indexer. |
| `(T)x` | Define custom type conversions performed by a cast expression. For more information, see [User-defined conversion operators](user-defined-explicit-and-implicit-conversion-operators.md). |
| `^x`, `x = y`, `x.y`, `x?.y`, `c ? t : f`, `x ?? y`, `??= y`, `x..y`, `x->y`, `=>`, `f(x)`, `as`, `await`, `checked`, `unchecked`, `default`, `delegate`, `is`, `nameof`, `new`, `sizeof`, `stackalloc`, `switch`, `typeof`, `with` | None. |

Before C# 14, the compound operators can't be overloaded. Overloading the corresponding binary operator implicitly overloads the corresponding compound assignment operator.

## Operator overload resolution

{% hint style="info" %}
This section applies to C# 14 and later. Before C# 14, user-defined compound assignment operators and instance increment and decrement operators aren't allowed.
{% endhint %}

If `x` is classified as a variable in a compound assignment expression such as `x «op»= y`, instance operators are preferred over any static operator for `«op»`. If an overloaded `«op»=` operator isn't declared for the type of `x` or `x` isn't classified as a variable, the static operators are used.

For the postfix operator `++`, if `x` isn't classified as a variable or the expression `x++` is used, the instance `operator++` is ignored. Otherwise, preference is given to the instance `operator++`. For example:

```csharp
x++; // Instance operator++ preferred.
y = x++; // instance operator++ isn't considered.
```

The reason for this rule is that `y` should be assigned to the value of `x` before it's incremented. The compiler can't determine that for a user-defined implementation in a reference type.

For the prefix operator `++`, if `x` is classified as a variable in `++x`, the instance operator is preferred over a static unary operator.

## Best practices for operator overloading

When implementing operator overloading, consider these best practices:

1. **Maintain semantic consistency**: Operators should behave in a way that's intuitive and consistent with their standard meaning. For example, `+` should represent addition or concatenation, not subtraction.

2. **Preserve algebraic identities**: When possible, maintain mathematical properties like commutativity (`a + b == b + a`) and associativity (`(a + b) + c == a + (b + c)`).

3. **Implement related operators together**: If you overload `==`, also overload `!=`. If you overload `<`, also overload `>`, `<=`, and `>=`.

4. **Consider overriding `Equals` and `GetHashCode`**: When overloading equality operators, also override the `Equals` method and `GetHashCode` to maintain consistency.

5. **Provide conversion operators when appropriate**: If your type can be naturally converted to or from other types, consider implementing conversion operators.

6. **Be cautious with performance**: Operator overloading can hide complex operations behind simple syntax. Ensure your implementations are efficient, especially for frequently used operators.

## Example: Complex number implementation

Here's a more complete example showing a complex number implementation with operator overloading:

```csharp
public struct Complex
{
    public double Real { get; }
    public double Imaginary { get; }

    public Complex(double real, double imaginary)
    {
        Real = real;
        Imaginary = imaginary;
    }

    // Unary operators
    public static Complex operator +(Complex c) => c;
    public static Complex operator -(Complex c) => new Complex(-c.Real, -c.Imaginary);

    // Binary operators
    public static Complex operator +(Complex a, Complex b)
        => new Complex(a.Real + b.Real, a.Imaginary + b.Imaginary);

    public static Complex operator -(Complex a, Complex b)
        => new Complex(a.Real - b.Real, a.Imaginary - b.Imaginary);

    public static Complex operator *(Complex a, Complex b)
        => new Complex(
            a.Real * b.Real - a.Imaginary * b.Imaginary,
            a.Real * b.Imaginary + a.Imaginary * b.Real);

    public static Complex operator /(Complex a, Complex b)
    {
        double denominator = b.Real * b.Real + b.Imaginary * b.Imaginary;
        if (denominator == 0)
            throw new DivideByZeroException();

        return new Complex(
            (a.Real * b.Real + a.Imaginary * b.Imaginary) / denominator,
            (a.Imaginary * b.Real - a.Real * b.Imaginary) / denominator);
    }

    // Equality operators
    public static bool operator ==(Complex a, Complex b)
        => a.Real == b.Real && a.Imaginary == b.Imaginary;

    public static bool operator !=(Complex a, Complex b)
        => !(a == b);

    // Override Equals and GetHashCode
    public override bool Equals(object obj)
        => obj is Complex other && this == other;

    public override int GetHashCode()
        => HashCode.Combine(Real, Imaginary);

    // Conversion operators
    public static implicit operator Complex(double d)
        => new Complex(d, 0);

    public static explicit operator double(Complex c)
    {
        if (c.Imaginary != 0)
            throw new InvalidCastException("Cannot convert a complex number with non-zero imaginary part to a double.");
        return c.Real;
    }

    public override string ToString()
        => $"{Real} + {Imaginary}i";
}
```

This implementation allows for natural mathematical operations with complex numbers:

```csharp
var a = new Complex(1, 2);
var b = new Complex(3, 4);
var c = a + b;  // 4 + 6i
var d = a * b;  // -5 + 10i
var e = 5.0 + a;  // 6 + 2i (using implicit conversion)
```

## Key Points

- Operator overloading allows you to define custom behavior for C# operators when applied to your types
- To overload an operator, declare a public static method using the `operator` keyword
- At least one parameter must be of the type containing the operator declaration
- Some operators must be overloaded in pairs (e.g., `==` and `!=`)
- C# 14 introduced instance versions of `++` and `--` operators and compound assignment operators
- Not all operators can be overloaded; some have alternative approaches (e.g., indexers, conversion operators)
- Maintain semantic consistency and follow mathematical principles when overloading operators
- Consider overriding `Equals` and `GetHashCode` when implementing equality operators

## FAQs

**Q: Can I change the precedence of operators through overloading?**  
A: No, operator precedence is fixed in C# and cannot be changed through operator overloading. The precedence of your overloaded operators will be the same as the built-in operators.

**Q: Should I always implement both instance and static versions of `++` and `--`?**  
A: It depends on your needs. Instance versions (available in C# 14+) can be more efficient for mutable types as they avoid creating new instances. For immutable types, you'll need to use the static versions that return new instances.

**Q: Can I overload operators for existing types I don't own?**  
A: No, operator overloading must be defined within the type that contains at least one of the operands. However, you can create wrapper types or use extension methods for other operations.

**Q: Is there a performance penalty for using operator overloading?**  
A: There's no inherent performance penalty beyond the cost of the method call. However, poorly implemented operators that create unnecessary objects or perform complex calculations can impact performance.

**Q: How do I decide whether to implement a conversion operator or an overloaded operator?**  
A: Use conversion operators when you want to convert between different types (e.g., `double` to `Complex`). Use overloaded operators when you want to define operations between instances of the same or related types (e.g., adding two `Complex` numbers).

**Q: Can I overload the assignment operator (`=`)?**  
A: No, the assignment operator cannot be overloaded in C#. However, you can often achieve similar functionality through conversion operators or by implementing custom assignment methods.

**Q: How do I implement conditional logical operators (`&&` and `||`)?**  
A: You can't directly overload `&&` and `||`, but you can achieve similar functionality by overloading the `&` and `|` operators along with the `true` and `false` operators. This allows your type to work with conditional logical expressions.