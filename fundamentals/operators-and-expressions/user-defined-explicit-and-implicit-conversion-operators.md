## Introduction

User-defined conversion operators are like custom bridges between types. Imagine you have two islands (types) and you want to travel between them - conversion operators are the bridges you build to make that journey possible. In C#, you can create your own bridges that allow values to move seamlessly from one type to another.

These custom conversions come in two varieties: implicit conversions that happen automatically (like moving on a conveyor belt), and explicit conversions that require deliberate action (like crossing a bridge with a toll gate).

## Implicit conversion operators

Implicit conversions are like automatic doors that open as you approach. They happen without any special syntax and should always succeed without losing information.

```csharp
// Defining an implicit conversion operator
public readonly struct Digit
{
    private readonly byte digit;

    public Digit(byte digit)
    {
        if (digit > 9)
        {
            throw new ArgumentOutOfRangeException(nameof(digit), "Digit cannot be greater than nine.");
        }
        this.digit = digit;
    }

    // This operator allows Digit to be used anywhere a byte is expected
    // No special syntax is needed when using this conversion
    public static implicit operator byte(Digit d) => d.digit;
    
    public override string ToString() => $"{digit}";
}

// Using the implicit conversion
Digit d = new Digit(7);
byte number = d;  // Implicit conversion happens automatically
Console.WriteLine(number);  // output: 7
```

Key characteristics of implicit conversions:
- They must never throw exceptions
- They should never lose information
- They're used when conversion is guaranteed to succeed
- They're defined using the `implicit` keyword

{% hint style="success" %}
Design implicit conversions to be safe and lossless. Users won't see these conversions in code, so they should never cause unexpected behavior.
{% endhint %}

## Explicit conversion operators

Explicit conversions are like toll gates that require you to stop and take deliberate action. They use cast syntax and may potentially lose information or fail.

```csharp
public readonly struct Digit
{
    private readonly byte digit;

    public Digit(byte digit)
    {
        if (digit > 9)
        {
            throw new ArgumentOutOfRangeException(nameof(digit), "Digit cannot be greater than nine.");
        }
        this.digit = digit;
    }

    // This operator requires explicit cast syntax when used
    // It might fail if the byte value is > 9
    public static explicit operator Digit(byte b) => new Digit(b);
    
    public override string ToString() => $"{digit}";
}

// Using the explicit conversion
byte number = 7;
Digit digit = (Digit)number;  // Explicit cast is required
Console.WriteLine(digit);  // output: 7

// This would throw an exception at runtime
// Digit invalidDigit = (Digit)12;  // ArgumentOutOfRangeException
```

Key characteristics of explicit conversions:
- They may throw exceptions
- They might lose information
- They require explicit cast syntax
- They're defined using the `explicit` keyword

{% hint style="warning" %}
Always use explicit conversion when the conversion might fail or lose information. This makes the potential risk visible in the code.
{% endhint %}

## Conversion operator rules

When defining conversion operators, you need to follow certain rules:

```csharp
// Basic syntax for conversion operators
public static implicit operator TargetType(SourceType source) => /* conversion logic */;
public static explicit operator TargetType(SourceType source) => /* conversion logic */;
```

Important rules to remember:
- The type that defines the conversion must be either the source or target type
- Conversion operators must be static
- You can't create conversion operators for built-in types
- You can define conversions between two user-defined types in either type

```csharp
// Example of conversions between two custom types
public struct Celsius
{
    public double Temperature { get; }
    
    public Celsius(double temperature)
    {
        Temperature = temperature;
    }
    
    // Conversion from Celsius to Fahrenheit
    public static implicit operator Fahrenheit(Celsius c)
        => new Fahrenheit(c.Temperature * 9 / 5 + 32);
        
    public override string ToString() => $"{Temperature}°C";
}

public struct Fahrenheit
{
    public double Temperature { get; }
    
    public Fahrenheit(double temperature)
    {
        Temperature = temperature;
    }
    
    // Conversion from Fahrenheit to Celsius
    public static implicit operator Celsius(Fahrenheit f)
        => new Celsius((f.Temperature - 32) * 5 / 9);
        
    public override string ToString() => $"{Temperature}°F";
}

// Usage
Celsius c = new Celsius(100);
Fahrenheit f = c;  // Implicit conversion
Console.WriteLine(f);  // output: 212°F

Fahrenheit f2 = new Fahrenheit(98.6);
Celsius c2 = f2;  // Implicit conversion
Console.WriteLine(c2);  // output: 37°C
```

{% hint style="info" %}
The `is` and `as` operators don't consider user-defined conversions. They only work with reference conversions, boxing, and unboxing.
{% endhint %}

## Checked conversion operators (C# 11+)

Starting with C# 11, you can define checked explicit conversion operators that are used in checked contexts to provide additional safety.

```csharp
public readonly struct SmallNumber
{
    private readonly int number;
    
    public SmallNumber(int value)
    {
        number = value;
    }
    
    // Regular explicit conversion - might silently lose data
    public static explicit operator byte(SmallNumber s) => (byte)s.number;
    
    // Checked explicit conversion - throws exception on data loss
    public static explicit operator checked byte(SmallNumber s)
    {
        if (s.number < byte.MinValue || s.number > byte.MaxValue)
            throw new OverflowException("Value is outside the range of the byte type");
        return (byte)s.number;
    }
}

// Usage
SmallNumber n = new SmallNumber(1000);

// In unchecked context (default)
byte b1 = (byte)n;  // Truncates to 232 (1000 % 256)

// In checked context
checked
{
    // byte b2 = (byte)n;  // Throws OverflowException
}
```

{% hint style="success" %}
Use checked operators when you want to ensure that conversions don't silently lose data in checked contexts.
{% endhint %}

## Key Points

- User-defined conversion operators allow custom types to be converted to and from other types
- Implicit conversions happen automatically and should never throw exceptions or lose data
- Explicit conversions require cast syntax and may throw exceptions or lose information
- The type defining the conversion must be either the source or target type
- Conversion operators must be static and can't be defined for built-in types
- The `is` and `as` operators don't consider user-defined conversions
- C# 11 introduced checked explicit conversion operators for additional safety in checked contexts

## FAQs

**Q: When should I use implicit versus explicit conversion operators?**  
A: Use implicit conversions when the conversion is guaranteed to succeed without data loss. Use explicit conversions when the conversion might fail or lose information.

**Q: Can I define conversion operators between any two types?**  
A: No, at least one of the types (source or target) must be the type where you're defining the operator. You can't define conversions between two types that you don't own.

**Q: Do user-defined conversions work with the `is` and `as` operators?**  
A: No, the `is` and `as` operators only consider reference conversions, boxing, and unboxing. They don't use user-defined conversion operators.

**Q: Can I chain conversion operators?**  
A: Yes, the compiler can use multiple user-defined conversions in sequence to convert between types, but it will only use one implicit and one explicit conversion at most.

**Q: What's the difference between a conversion operator and a constructor or method that performs conversion?**  
A: Conversion operators allow your type to be used with cast syntax and in contexts where automatic conversions happen. Constructors and methods require explicit method calls.

**Q: What are checked conversion operators and when should I use them?**  
A: Checked conversion operators (C# 11+) are special versions of explicit conversion operators that are used in checked contexts. They provide additional safety by throwing exceptions when data loss would occur.

**Q: Can I overload conversion operators?**  
A: You can define both implicit and explicit conversion operators for the same source and target types, but you can't have multiple implicit or multiple explicit operators with the same signature.