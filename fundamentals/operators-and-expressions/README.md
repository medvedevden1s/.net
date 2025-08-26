# Operators and expressions

### Introduction

Operators and expressions are the building blocks of any program. They let you combine values, perform calculations, make comparisons, and control how code executes. Understanding how operators work—and how the compiler decides the order of execution—is essential for writing correct and readable code.

***

### Categories Of Operators

#### Arithmetic Operators

Used for numeric calculations:

```csharp
int sum = 5 + 3;   // 8
int diff = 10 - 2; // 8
int product = 4 * 2; // 8
int quotient = 16 / 2; // 8
int remainder = 17 % 9; // 8
```

#### Comparison Operators

Used to compare numbers or values:

```csharp
int a = 5, b = 8;
Console.WriteLine(a < b);   // true
Console.WriteLine(a == b);  // false
```

#### Logical Operators

Work with boolean values:

```csharp
bool isAdmin = true;
bool isActive = false;
Console.WriteLine(isAdmin && isActive); // false
Console.WriteLine(isAdmin || isActive); // true
```

#### Bitwise And Shift Operators

Operate on bits of integers:

```csharp
int x = 6;   // 110 in binary
int y = 3;   // 011 in binary
Console.WriteLine(x & y); // 2 (010)
Console.WriteLine(x << 1); // 12 (1100)
```

#### Equality Operators

Check if values are equal:

```csharp
string s1 = "hello";
string s2 = "world";
Console.WriteLine(s1 == s2); // false
```

\{% hint style="info" %\}\
**Info:** You can overload many operators in your own classes to give them custom meaning.\
\{% endhint %\}

***

### Expressions

An **expression** is a combination of variables, constants, and operators that produces a value.

```csharp
int a = 7;
int b = a + 3;       // expression: a + 3
int c = b >= 10 ? 1 : 0; // ternary expression
Console.WriteLine((int)Math.Sqrt(b * b + c * c));
```

Some expressions don’t return values, such as `Console.WriteLine("Hi")`, which is an **expression statement**.

***

### Interpolated Strings

Expressions can be embedded into strings:

```csharp
double r = 2.3;
string message = $"Area = {Math.PI * r * r:F2}";
Console.WriteLine(message); // Area = 16.62
```

***

### Lambda Expressions

Anonymous functions written inline:

```csharp
int[] nums = { 2, 3, 4, 5 };
var maxSquare = nums.Max(x => x * x);
Console.WriteLine(maxSquare); // 25
```

***

### Query Expressions

LINQ allows SQL-like queries inside code:

```csharp
int[] scores = { 90, 97, 78, 68, 85 };
var highScores = from s in scores
                 where s > 80
                 orderby s descending
                 select s;
Console.WriteLine(string.Join(" ", highScores)); // 97 90 85
```

***

### Operator Precedence

Some operators run before others. Multiplication comes before addition:

```csharp
var result = 2 + 2 * 2; 
Console.WriteLine(result); // 6
```

Use parentheses to override precedence:

```csharp
var result = (2 + 2) * 2; 
Console.WriteLine(result); // 8
```

***

### Operator Associativity

If operators have the same precedence, associativity decides evaluation order:

* **Left-associative**: evaluated left → right (`a + b - c`)
* **Right-associative**: evaluated right → left (`x = y = z`)

```csharp
int a = 13 / 5 / 2;      // left-to-right → (13 / 5) / 2 = 1
int b = 13 / (5 / 2);    // parentheses change order = 6
Console.WriteLine($"a = {a}, b = {b}");
```

***

### Operand Evaluation Order

Operands are always evaluated **left to right**:

```csharp
int x = 1, y = 2, z = 3;
int result = x + y * z; // order: x, y, z, *, +
```

Some operators skip evaluation to save time:

* `&&` and `||` (short-circuit)
* `??` and `??=` (null-coalescing)
* `?.` and `?[]` (null-conditional)

```csharp
string? text = null;
Console.WriteLine(text?.Length); // no exception, prints nothing
```

\{% hint style="warning" %\}\
**Warning:** Be careful with short-circuiting—some code may never execute if the first condition decides the result.\
\{% endhint %\}

***

### Key Points

* Operators let you perform calculations, comparisons, and control flow.
* Expressions combine operators, variables, and values to produce results.
* Precedence and associativity decide the execution order.
* Parentheses override precedence for clarity.
* Operands are always evaluated left to right, but some operators can skip evaluation.

***

### FAQs

**Q: Can I define custom operators in my classes?**\
Yes, you can overload many operators (like `+`, `==`, etc.) for user-defined types.

**Q: What’s the difference between `==` and `Equals`?**\
`==` can be overloaded, while `Equals` is a method you can override. For reference types, `==` usually checks reference equality unless overridden.

**Q: Why does `&&` skip evaluating the second operand sometimes?**\
Because if the first operand is `false`, the result is already `false`, so the second operand isn’t needed.

**Q: Should I always rely on operator precedence rules?**\
No. Use parentheses for readability—your code should be easy for humans to read, not just compilers.
