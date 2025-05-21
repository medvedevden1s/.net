# Value types

## Introduction

Value types directly store their values within the memory allocated for the variable itself. They are typically allocated on the stack, offering faster access and efficient memory management.

Value types derive from `System.ValueType` in the .NET framework. They include numeric, character, and boolean types.

### Numeric Types

Integral types store whole numbers, supporting signed and unsigned forms:

| Alias  | Type Name     | Size (bits) | Range                           | Default Value | Usage                              |
| ------ | ------------- | ----------- | ------------------------------- | ------------- | ---------------------------------- |
| sbyte  | System.SByte  | 8           | -128 to 127                     | 0             | Small negative and positive values |
| byte   | System.Byte   | 8           | 0 to 255                        | 0             | Small positive-only values         |
| short  | System.Int16  | 16          | -32,768 to 32,767               | 0             | Moderate numeric values            |
| ushort | System.UInt16 | 16          | 0 to 65,535                     | 0             | Moderate positive-only counters    |
| int    | System.Int32  | 32          | -2,147,483,648 to 2,147,483,647 | 0             | General numeric operations         |
| uint   | System.UInt32 | 32          | 0 to 4,294,967,295              | 0             | Large positive-only counters       |
| long   | System.Int64  | 64          | -9.2E+18 to 9.2E+18             | 0L            | Very large numeric values          |
| ulong  | System.UInt64 | 64          | 0 to 1.8E+19                    | 0UL           | Very large positive-only values    |

```csharp
int age = 30;
uint distance = 4294967295U;
long largeNumber = 9223372036854775807L;
```

{% hint style="info" %}
**Signed types** can represent negative and positive numbers.

**Unsigned types** represent positive numbers and zero, allowing for a larger positive range.
{% endhint %}

#### **Floating-Point Types**

Floating-point types store numbers with fractional parts:

| Alias  | Type Name     | Size (bits) | Range (approx.)               | Default Value | Usage                               |
| ------ | ------------- | ----------- | ----------------------------- | ------------- | ----------------------------------- |
| float  | System.Single | 32          | ±1.5 × 10⁻⁴⁵ to ±3.4 × 10³⁸   | 0.0F          | Precision-critical, small decimals  |
| double | System.Double | 64          | ±5.0 × 10⁻³²⁴ to ±1.7 × 10³⁰⁸ | 0.0D          | General-purpose floating-point math |

* **Float**: 32-bit single-precision, approximately 7-digit precision, suffix `f` or `F`.
* **Double**: 64-bit double-precision, approximately 15-digit precision, default for numeric literals with decimal points.

```csharp
float temperature = 36.5F;
double salary = 55000.75;
```

#### **Decimal Type**

The decimal type is ideal for financial and monetary calculations:

| Alias   | Type Name      | Size (bits) | Range (approx.)                | Default Value | Usage                  |
| ------- | -------------- | ----------- | ------------------------------ | ------------- | ---------------------- |
| decimal | System.Decimal | 128         | ±1.0 × 10⁻²⁸ to ±7.9228 × 10²⁸ | 0.0M          | Financial calculations |

* Uses suffix `m` or `M` to explicitly declare as decimal.

```csharp
decimal price = 300.50M;
```

### Non-Numeric Types

**Boolean Type**

Stores logical values of true or false:

| Alias | Type Name      | Possible Values | Usage             |
| ----- | -------------- | --------------- | ----------------- |
| bool  | System.Boolean | true / false    | Conditional logic |

```csharp
bool isActive = true;
if (isActive)
{
    Console.WriteLine("Active status confirmed.");
}
```

**Character Type**

Stores single Unicode characters (UTF-16):

| Alias | Type Name   | Size (bits) | Range            | Default Value | Usage                 |
| ----- | ----------- | ----------- | ---------------- | ------------- | --------------------- |
| char  | System.Char | 16          | U+0000 to U+FFFF | '\0'          | Individual characters |

```csharp
char initial = 'A';
```

### Custom Value Types

**Structs**

Structs allow grouping related data into a single value type, ideal for lightweight data structures.

```csharp
struct Point
{
    public int X;
    public int Y;
}

Point p = new Point { X = 10, Y = 20 };
```

**Enums**

Enums define named constants, improving readability and maintainability:

```csharp
enum Days
{
    Monday,
    Tuesday,
    Wednesday
}

Days today = Days.Tuesday;
```

### Summary

* Value types store data directly within their allocated memory.
* Structs and enums are customizable value types derived implicitly from `System.ValueType`.
* Understanding signed vs unsigned types, floating-point precision, and explicit literal declarations ensures precise and efficient coding.

### FAQs&#x20;

**Q1: What is the difference between signed and unsigned integral types?**

* **Signed types** (`sbyte`, `short`, `int`, `long`) can represent negative and positive numbers.
* **Unsigned types** (`byte`, `ushort`, `uint`, `ulong`) represent only non-negative numbers (zero and positive), offering a larger positive range.

**Q2: Why should I use `decimal` instead of `double` for financial calculations?**

* The `decimal` type provides higher precision (28-29 significant digits), reducing rounding errors, making it ideal for financial and monetary calculations.

**Q3: How do I explicitly specify numeric literal types in code?**

* `F` or `f` for `float` (e.g., `3.5F`).
* `D` or `d` for `double` (optional, e.g., `3.5`).
* `M` or `m` for `decimal` (e.g., `3.5M`).
* `L` or `l` for `long` (e.g., `300L`).

```csharp
float temperature = 36.5F;   // float with suffix F
double salary = 55000.75;    // double (suffix optional)
decimal price = 300.50M;     // decimal with suffix M
long largeNumber = 300L;     // long with suffix L
```

**Q4: Can I store an integer value directly into a `char` type?**

*   Not directly, but you can cast explicitly:

    ```csharp
    char c = (char)65; // c will be 'A'
    ```

**Q5: When should I use structs instead of classes?**

* Structs are ideal for lightweight, immutable, and small-sized data structures that do not require inheritance
