# Bitwise and shift operators

### Introduction

Bitwise and shift operators let you work directly with the **bits** of numbers. These operators are especially useful when working with **low-level operations**, **flags**, or **performance optimizations**.

They include:

* Unary bitwise complement: `~`
* Binary shifts: `<<` (left), `>>` (right), `>>>` (unsigned right)
* Binary logical: `&` (AND), `|` (OR), `^` (XOR)

They work on **integral numeric types** (`int`, `uint`, `long`, `ulong`, `nint`, `nuint`) and `char`.

***

### Bitwise Complement Operator `~`

Reverses every bit in the operand.

```csharp
uint a = 0b_0000_1111_0000_1111;
uint b = ~a;
Console.WriteLine(Convert.ToString(b, 2));
// Output: 1111000011110000
```

{% hint style="warning" %}
The same `~` symbol is also used for **finalizers**, not related to bitwise operations.
{% endhint %}

***

### Left Shift Operator `<<`

Shifts bits to the left. High bits are discarded, new low bits are set to `0`.

```csharp
uint x = 0b_1100_1001;
uint y = x << 4;
Console.WriteLine(Convert.ToString(y, 2));
// Before: 11001001
// After:  10010000
```

If you shift smaller integral types (like `byte`), they are **promoted to int**:

```csharp
byte a = 0b_1111_0001;
var b = a << 8;
Console.WriteLine(b.GetType());  // System.Int32
```

***

### Right Shift Operator `>>`

Shifts bits to the right. Low bits are discarded.

* For **signed types** (`int`, `long`): performs **arithmetic shift** (keeps sign bit).
* For **unsigned types** (`uint`, `ulong`): performs **logical shift** (fills with `0`).

```csharp
int a = int.MinValue;
int b = a >> 3;
Console.WriteLine(Convert.ToString(b, 2));
// Output keeps sign bit: 111100000000...
```

```csharp
uint c = 0b_1000_0000;
uint d = c >> 3;
Console.WriteLine(Convert.ToString(d, 2));
// Output: 00010000 (fills with 0s)
```

***

### Unsigned Right Shift Operator `>>>`

Introduced in **C# 11**. Always performs a **logical shift**, even on signed integers.

```csharp
int x = -8;

int y = x >> 2;   // arithmetic shift
int z = x >>> 2;  // logical shift

Console.WriteLine(y); // -2
Console.WriteLine(z); // 1073741822
```

***

### Bitwise AND Operator `&`

Each bit is `1` only if **both** operands have `1`.

```csharp
uint a = 0b_1111_1000;
uint b = 0b_1001_1101;
uint c = a & b;
Console.WriteLine(Convert.ToString(c, 2)); // 10011000
```

For `bool`, `&` computes **logical AND**.

***

### Bitwise XOR Operator `^`

Each bit is `1` only if operands are **different**.

```csharp
uint a = 0b_1111_1000;
uint b = 0b_0001_1100;
uint c = a ^ b;
Console.WriteLine(Convert.ToString(c, 2)); // 11100100
```

For `bool`, `^` is **logical XOR**.

***

### Bitwise OR Operator `|`

Each bit is `1` if **at least one** operand has `1`.

```csharp
uint a = 0b_1010_0000;
uint b = 0b_1001_0001;
uint c = a | b;
Console.WriteLine(Convert.ToString(c, 2)); // 10110001
```

For `bool`, `|` is **logical OR**.

***

### Compound Assignment

Bitwise and shift operators support compound assignments:

```csharp
uint a = 0b_1111_1000;

a &= 0b_1001_1101; // AND
a |= 0b_0011_0001; // OR
a ^= 0b_1000_0000; // XOR
a <<= 2;           // Left shift
a >>= 4;           // Right shift
a >>>= 4;          // Unsigned right shift
```

{% hint style="info" %}
Compound assignments automatically cast back to the original type if possible.
{% endhint %}

***

### Operator Precedence

From **highest** to **lowest**:

1. `~` (bitwise complement)
2. `<<`, `>>`, `>>>` (shifts)
3. `&` (AND)
4. `^` (XOR)
5. `|` (OR)

```csharp
uint a = 0b_1101;
uint b = 0b_1001;
uint c = 0b_1010;

uint d1 = a | b & c;   // & runs first
uint d2 = (a | b) & c; // parentheses change order
```

***

### Shift Count Rules

* For `int`, `uint`: only **low 5 bits** (mask `0x1F`) of shift count matter.
* For `long`, `ulong`: only **low 6 bits** (mask `0x3F`) matter.

```csharp
int a = 1;
int count = 225; // 225 & 0x1F = 1
Console.WriteLine(a << count); // same as a << 1
```

***

### Enums with Bitwise Operators

Enums with `[Flags]` can use `~`, `&`, `|`, `^` for combining values.

```csharp
[Flags]
enum Permissions { Read = 1, Write = 2, Execute = 4 }

Permissions p = Permissions.Read | Permissions.Write;
Console.WriteLine(p); // Read, Write
```

***

### Operator Overloading

User-defined types can overload:

* `~`, `<<`, `>>`, `>>>`, `&`, `|`, `^`.
* Corresponding compound operators (`<<=`, `|=`, etc.) are also supported.

***

### Truth Tables (Quick Revision)

#### AND (`&`)

Bit is `1` only if both are `1`.

| A | B | A & B |
| - | - | ----- |
| 0 | 0 | 0     |
| 0 | 1 | 0     |
| 1 | 0 | 0     |
| 1 | 1 | 1     |

#### OR (`|`)

Bit is `1` if at least one is `1`.

| A | B | A \| B |
| - | - | ------ |
| 0 | 0 | 0      |
| 0 | 1 | 1      |
| 1 | 0 | 1      |
| 1 | 1 | 1      |

#### XOR (`^`)

Bit is `1` if values are different.

| A | B | A ^ B |
| - | - | ----- |
| 0 | 0 | 0     |
| 0 | 1 | 1     |
| 1 | 0 | 1     |
| 1 | 1 | 0     |

#### NOT (`~`)

Flips each bit.

| A | \~A |
| - | --- |
| 0 | 1   |
| 1 | 0   |

***

### Key Points

* `~` flips all bits.
* `<<`, `>>`, `>>>` shift bits left/right.
* `>>` keeps sign for signed types; `>>>` always fills with `0`.
* `&`, `|`, `^` are the **core bitwise logical operators**.
* Operators support **compound assignments** (`&=`, `<<=`).
* Enums with `[Flags]` commonly use bitwise operators.

***

### FAQs

**Q: What’s the difference between `>>` and `>>>`?**\
`>>` is arithmetic shift (preserves sign), while `>>>` is logical shift (fills with zeros).

**Q: Can I use bitwise operators with `bool`?**\
Yes — `&`, `|`, `^` also work with `bool`.

**Q: Why does shifting a `byte` give me an `int`?**\
Because smaller types are promoted to `int` before shifting.

**Q: Are bitwise operations safe from overflow?**\
Yes — they never cause overflow.

**Q: Why are bitwise operators used with enums?**\
To combine multiple flags in a single value (`[Flags]` attribute).
