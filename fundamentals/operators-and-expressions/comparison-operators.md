# Comparison operators

### Introduction

Comparison operators let you compare values and decide whether one is smaller, greater, or equal to another. They are sometimes called _relational operators_.\
They return a boolean result: `true` or `false`.

Supported types:

* All numeric types (integers and floating-point numbers)
* `char` (compares character codes)
* `enum` (compares underlying numeric values)

### Less Than `<`

Returns `true` if the left value is smaller than the right, otherwise `false`.

```csharp
Console.WriteLine(7.0 < 5.1);   // False (7 is not less than 5.1)
Console.WriteLine(5.1 < 5.1);   // False (equal, not less)
Console.WriteLine(0.0 < 5.1);   // True  (0 is less than 5.1)
```

### Greater Than `>`

Returns `true` if the left value is bigger than the right.

```csharp
Console.WriteLine(7.0 > 5.1);   // True  (7 is greater than 5.1)
Console.WriteLine(5.1 > 5.1);   // False (equal, not greater)
Console.WriteLine(0.0 > 5.1);   // False (0 is not greater than 5.1)
```

### Less Than Or Equal `<=`

Returns `true` if the left value is smaller **or equal**.

```csharp
Console.WriteLine(7.0 <= 5.1);   // False (7 is not smaller or equal)
Console.WriteLine(5.1 <= 5.1);   // True  (they are equal)
Console.WriteLine(0.0 <= 5.1);   // True  (0 is smaller than 5.1)
```

### Greater Than Or Equal `>=`

Returns `true` if the left value is bigger **or equal**.

```csharp
Console.WriteLine(7.0 >= 5.1);   // True  (7 is greater)
Console.WriteLine(5.1 >= 5.1);   // True  (equal)
Console.WriteLine(0.0 >= 5.1);   // False (0 is not greater or equal to 5.1)
```

### Special Case: NaN

When comparing floating-point values (`float` or `double`), `NaN` (Not a Number) behaves differently:

* Any comparison with `NaN` is always `false` (even `NaN == NaN` is false).

```csharp
Console.WriteLine(double.NaN < 5.1);   // False
Console.WriteLine(double.NaN > 5.1);   // False
Console.WriteLine(double.NaN == double.NaN); // False
```

\{% hint style="warning" %\}\
NaN is tricky: it’s _not equal to anything_, including itself. This is by design in IEEE 754 floating-point math.\
\{% endhint %\}

To check if a value is NaN, use `double.IsNaN(value)` or `float.IsNaN(value)`.

### Comparison With `char`

Characters are compared using their Unicode values.

```csharp
Console.WriteLine('a' < 'b');   // True  (97 < 98)
Console.WriteLine('A' < 'a');   // True  (65 < 97)
Console.WriteLine('z' > 'x');   // True
```

### Comparison With `enum`

Enums compare by their underlying numeric values.

```csharp
enum Level { Low = 1, Medium = 2, High = 3 }

Console.WriteLine(Level.Low < Level.Medium);   // True  (1 < 2)
Console.WriteLine(Level.High > Level.Medium);  // True  (3 > 2)
```

\{% hint style="info" %\}\
Enums are compared **only if both operands are of the same enum type**.\
\{% endhint %\}

### Operator Overloading

Custom types can overload comparison operators.

Rules:

* If you overload `<`, you must also overload `>`.
* If you overload `<=`, you must also overload `>=`.

```csharp
public class Box
{
    public int Size { get; }
    public Box(int size) => Size = size;

    public static bool operator <(Box a, Box b) => a.Size < b.Size;
    public static bool operator >(Box a, Box b) => a.Size > b.Size;
}
```

### Key Points

* Comparison operators return `true` or `false`.
* `NaN` is never equal, less, or greater than anything.
* `char` compares Unicode values, not alphabetical order.
* `enum` compares underlying numeric values.
* You can overload comparison operators in your own types.

### FAQs

❓ **Why is `NaN == NaN` false?**\
Because by definition `NaN` means "not a number," so it cannot be equal to anything, not even itself. Always use `double.IsNaN()` for checking.

❓ **Can I compare different enum types?**\
No. Both operands must be of the same enum type.

❓ **Do characters compare alphabetically?**\
Not exactly. They compare by Unicode codes, which often matches alphabetical order in English but not always in other languages.

❓ **What happens if I compare different numeric types?**\
They are converted to a common type before comparison (for example, `int` and `double` → `double`).
