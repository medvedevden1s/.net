# The checked and unchecked statements

### Introduction

Integer arithmetic sometimes goes beyond the limits of its type. For example, adding `1` to `int.MaxValue` results in an overflow. By default, C# silently ignores overflow (wraps around), but with the `checked` keyword you can force the runtime to throw an exception. With `unchecked`, you explicitly allow wraparound behavior.

This article explains how **checked** and **unchecked** contexts work, when to use them, and what best practices to follow.

***

### Checked

When code runs in a **checked context**, arithmetic overflow throws a `System.OverflowException`.

```csharp
uint a = uint.MaxValue;

try
{
    checked // enables overflow checking
    {
        Console.WriteLine(a + 3);  
        // throws OverflowException at runtime
    }
}
catch (OverflowException e)
{
    Console.WriteLine(e.Message); 
    // "Arithmetic operation resulted in an overflow."
}
```

📌 Key facts:

* Useful when correctness is critical (e.g., financial calculations).
* Prevents unnoticed data corruption.
* Applies only to integral arithmetic and conversions.

***

### Unchecked

In an **unchecked context**, arithmetic overflow **wraps around**. Extra bits are discarded, and no exception is thrown.

```csharp
uint a = uint.MaxValue;

unchecked
{
    Console.WriteLine(a + 3); 
    // output: 2 (wraparound)
}
```

📌 Key facts:

* Default behavior for runtime expressions (unless compiler option changes it).
* Faster (no overflow checking).
* Dangerous if overflow leads to wrong business logic.

{% hint style="warning" %}
Unchecked arithmetic can silently corrupt results. Use it only when overflow behavior is intended, like in cryptography or low-level bitwise operations.
{% endhint %}

***

### Operators Checked vs Unchecked

You can apply `checked` and `unchecked` as operators to **specific expressions**, not just blocks:

```csharp
double a = double.MaxValue;

// unchecked conversion: wraps around
int x = unchecked((int)a);  
Console.WriteLine(x); // -2147483648

// checked conversion: throws exception
try
{
    int y = checked((int)a);
}
catch (OverflowException e)
{
    Console.WriteLine(e.Message);
}
```

***

### Important Notes

```csharp
int Multiply(int a, int b) => a * b;

int factor = 2;

try
{
    checked
    {
        Console.WriteLine(Multiply(factor, int.MaxValue));  
        // output: -2 (no exception: inside function, unchecked by default)
    }
}
catch (OverflowException e)
{
    Console.WriteLine(e.Message);
}

try
{
    checked
    {
        Console.WriteLine(Multiply(factor, factor * int.MaxValue));
        // exception: the second argument is calculated in checked context
    }
}
catch (OverflowException e)
{
    Console.WriteLine(e.Message);
}
```

{% hint style="danger" %}
&#x20;`checked` only applies to code _textually inside_ the block.&#x20;

It does not automatically change behavior inside called functions.
{% endhint %}

***

### Numeric Types Behavior

* **Integral types (int, uint, long, etc.):** overflow can wrap or throw.
* **decimal:** always throws on overflow, regardless of context.
* **float, double, Half:** overflow results in `PositiveInfinity` or `NegativeInfinity`.
* **Division by zero:** always throws, even inside `unchecked`.

***

### Default Context

* **Runtime expressions:** default = `unchecked`.
* **Constant expressions:** default = `checked`.\
  Overflow causes a **compile-time error**.

```csharp
const int x = int.MaxValue + 1;  
// Compile-time error: overflow in constant expression
```

To bypass this:

```csharp
const int y = unchecked(int.MaxValue + 1); 
```

***

### Key Points

* `checked` → throws exception on overflow.
* `unchecked` → allows wraparound (default at runtime).
* Constant expressions are always checked unless wrapped with `unchecked`.
* `decimal` always throws; `float/double` produce infinity.
* Use `checked` for correctness, `unchecked` for performance or intentional wraparound.

***

### FAQs

❓ **Does unchecked prevent division by zero exceptions?**\
No, division by zero always throws `DivideByZeroException`.

❓ **Can I change the default globally?**\
Yes, with the `/checked+` or `/checked-` compiler option (`CheckForOverflowUnderflow` in project settings).

❓ **Should I use checked everywhere?**\
Not always. It makes sense in domains where overflow is critical (banking, healthcare). In performance-sensitive code where wraparound is acceptable (cryptography, hash functions), `unchecked` is preferred.

❓ **What happens with user-defined operators?**\
Starting with C# 11, you can define your own **checked operators**. But behavior may differ from built-in operators, so review carefully.
