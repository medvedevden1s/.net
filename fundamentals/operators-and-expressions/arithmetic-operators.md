# Arithmetic operators

### Introduction

Arithmetic operators are some of the most commonly used tools in programming. They let you perform addition, subtraction, multiplication, division, and more. While they look simple, the details—like integer division, floating-point behavior, overflow, and operator precedence—are important to understand clearly.

***

### Increment And Decrement

The **increment (`++`)** and **decrement (`--`)** operators add or subtract `1` from a variable.

#### Postfix form (`x++` / `x--`)

Value is returned **before** the operation:

```csharp
int i = 3;
Console.WriteLine(i++); // prints 3
Console.WriteLine(i);   // now 4
```

#### Prefix form (`++x` / `--x`)

Value is returned **after** the operation:

```csharp
double a = 1.5;
Console.WriteLine(++a); // prints 2.5
Console.WriteLine(a);   // stays 2.5
```

{% hint style="info" %}
Use prefix when you need the updated value immediately; postfix when you need the old value first.
{% endhint %}

***

### Unary Plus And Minus

* `+x` just returns `x`.
* `-x` flips the sign of `x`.

```csharp
Console.WriteLine(+4);    // 4
Console.WriteLine(-4);    // -4
Console.WriteLine(-(-4)); // 4

uint u = 5;
var neg = -u; 
Console.WriteLine(neg); // -5 (type promoted to long)
```



***

### Multiplication, Division, And Remainder

#### Multiplication `*`

```csharp
Console.WriteLine(5 * 2);     // 10
Console.WriteLine(0.5 * 2.5); // 1.25
```

#### Division `/`

* **Integer division** rounds towards zero:

```csharp
Console.WriteLine(13 / 5);   // 2
Console.WriteLine(-13 / 5);  // -2
```

* **Floating-point division** keeps decimals:

```csharp
Console.WriteLine(13 / 5.0); // 2.6
```

#### Remainder `%`

* With integers:

```csharp
Console.WriteLine(5 % 4);   // 1
Console.WriteLine(-5 % 4);  // -1
```

* With floating-point:

```csharp
Console.WriteLine(5.9 % 3.1); // 2.8
```

\{% hint style="info" %\}\
For both integers and floats, the sign of the remainder follows the **left operand**.\
\{% endhint %\}

***

### Addition And Subtraction

```csharp
Console.WriteLine(5 + 4);   // 9
Console.WriteLine(7.5 - 2); // 5.5
```

* `+` also works for **string concatenation**.
* `+` and `-` are also used with **delegates** (combine / remove).

***

### Compound Assignment

Shorthand for applying an operation and assignment together:

```csharp
int a = 5;
a += 9;  // a = 14
a -= 4;  // a = 10
a *= 2;  // a = 20
a /= 4;  // a = 5
a %= 3;  // a = 2
```

\{% hint style="success" %\}\
Compound assignments evaluate the left operand only once, which makes them safer and faster in some cases.\
\{% endhint %\}

***

### Operator Precedence And Associativity

Order of evaluation (highest → lowest):

1. Postfix `x++`, `x--`
2. Prefix `++x`, `--x`, unary `+`, `-`
3. Multiplicative `*`, `/`, `%`
4. Additive `+`, `-`

* Binary arithmetic operators are **left-associative**.

```csharp
Console.WriteLine(2 + 2 * 2);   // 6
Console.WriteLine((2 + 2) * 2); // 8
```

***

### Overflow And Division By Zero

#### Integer Types

* **Division by zero** → always throws `DivideByZeroException`.
* **Overflow** depends on context:

```csharp
int max = int.MaxValue;
Console.WriteLine(unchecked(max + 3)); // wraps around
try
{
    int val = checked(max + 3); // throws OverflowException
}
catch { Console.WriteLine("Overflow occurred"); }
```

#### Floating-Point Types

* Never throw exceptions. Instead, produce:
  * `Infinity` (`1.0 / 0.0`)
  * `NaN` (`0.0 / 0.0`)

```csharp
double inf = 1.0 / 0.0;
Console.WriteLine(inf); // Infinity
```

#### Decimal Type

* Overflow and division by zero **always throw exceptions**.

***

### Round-Off Errors

Floating-point math isn’t exact:

```csharp
double x = 0.1 * 3;
Console.WriteLine(x == 0.3); // False
Console.WriteLine(x - 0.3);  // tiny error
```

\{% hint style="danger" %\}\
Avoid floats for financial calculations—use `decimal` instead.\
\{% endhint %\}

***

### Operator Overloadability

You can redefine arithmetic operators in your own types:

```csharp
public record struct Point(int X, int Y)
{
    public static Point operator +(Point l, Point r) => new(l.X + r.X, l.Y + r.Y);

    public static Point operator checked +(Point l, Point r)
    {
        checked { return new(l.X + r.X, l.Y + r.Y); }
    }
}
```

* Normal operator runs in **unchecked context**.
* `checked` operator runs in **checked context**.

***

### Key Points

* Arithmetic operators handle numeric operations, but also work with strings, delegates, and events in some cases.
* Increment/decrement exist in **postfix** and **prefix** forms.
* Integer division truncates, while floating-point division keeps decimals.
* `%` keeps the sign of the left operand.
* Overflow handling depends on type and context (`checked` vs `unchecked`).
* Floating-point math can produce `Infinity`, `NaN`, and round-off errors.
* You can overload arithmetic operators in custom types.

***

### FAQs

**Q: What’s the difference between `/` on integers and floats?**\
On integers, result is truncated. On floats/decimals, you get full precision.

**Q: Why is `0.1 * 3 != 0.3` with doubles?**\
Because doubles use binary fractions, which can’t exactly represent `0.1`.

**Q: Can I overload compound assignment operators (`+=`, `-=`)?**\
Yes, starting from **C# 14**. Otherwise, they are derived automatically from the base operator.

**Q: What’s the safest type for money?**\
Always use **decimal** to avoid floating-point round-off errors.
