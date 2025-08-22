# Selection statements - if, if-else, switch

### Introduction

Selection statements let you **choose different execution paths** in your program based on conditions. The most common ones are:

* `if` → runs code only if a Boolean expression is `true`.
* `if-else` → chooses between two branches.
* `switch` → selects from many paths based on pattern matching.

These statements are essential for decision-making in your programs.

***

### If Statement

The simplest decision-making tool.

```csharp
void DisplayMeasurement(double value)
{
    if (value < 0 || value > 100)
    {
        Console.Write("Warning: not acceptable value! ");
    }

    Console.WriteLine($"The measurement value is {value}");
}

DisplayMeasurement(45);  // Output: The measurement value is 45
DisplayMeasurement(-3);  // Output: Warning: not acceptable value! The measurement value is -3
```

* Runs its body **only if** the condition is `true`.
* Execution skips the block if condition is `false`.

***

### If-Else Statement

Lets you choose **between two paths**.

```csharp
void DisplayWeatherReport(double tempInCelsius)
{
    if (tempInCelsius < 20.0)
    {
        Console.WriteLine("Cold.");
    }
    else
    {
        Console.WriteLine("Perfect!");
    }
}

DisplayWeatherReport(15); // Cold.
DisplayWeatherReport(24); // Perfect!
```

***

### Else If Chains

You can check multiple conditions in order.

```csharp
void DisplayCharacter(char ch)
{
    if (char.IsUpper(ch))
        Console.WriteLine($"An uppercase letter: {ch}");
    else if (char.IsLower(ch))
        Console.WriteLine($"A lowercase letter: {ch}");
    else if (char.IsDigit(ch))
        Console.WriteLine($"A digit: {ch}");
    else
        Console.WriteLine($"Not alphanumeric character: {ch}");
}
```

***

### Switch Statement

`switch` is best when you have **many possible conditions**.

```csharp
void DisplayMeasurement(double measurement)
{
    switch (measurement)
    {
        case < 0.0:
            Console.WriteLine($"Measured value is {measurement}; too low.");
            break;

        case > 15.0:
            Console.WriteLine($"Measured value is {measurement}; too high.");
            break;

        case double.NaN:
            Console.WriteLine("Failed measurement.");
            break;

        default:
            Console.WriteLine($"Measured value is {measurement}.");
            break;
    }
}

DisplayMeasurement(-4);           // too low
DisplayMeasurement(30);           // too high
DisplayMeasurement(double.NaN);   // failed
```

#### Notes:

* Each case must end with `break`, `return`, or `throw`.
* `default` handles unmatched cases.
* Multiple case labels can share the same block.

***

### Multiple Case Labels

```csharp
void DisplayMeasurement(int measurement)
{
    switch (measurement)
    {
        case < 0:
        case > 100:
            Console.WriteLine($"Measured value is {measurement}; out of range.");
            break;
        default:
            Console.WriteLine($"Measured value is {measurement}.");
            break;
    }
}
```

***

### Case Guards

You can add extra conditions with `when`.

```csharp
void DisplayMeasurements(int a, int b)
{
    // Switch on a tuple (a, b) so both values can be checked at once
    switch ((a, b))
    {
        // Case 1: both numbers > 0 AND equal
        case (> 0, > 0) when a == b:
            Console.WriteLine($"Both are valid and equal to {a}.");
            break;

        // Case 2: both numbers > 0 but not equal
        case (> 0, > 0):
            Console.WriteLine($"First is {a}, second is {b}.");
            break;

        // Case 3: all other cases
        default:
            Console.WriteLine("One or both are not valid.");
            break;
    }
}

```

***

### Switch Expression

A more concise way to select values.

```csharp
string WeatherReport(double temp) =>
    temp switch
    {
        < 20.0 => "Cold.",
        > 25.0 => "Hot.",
        _ => "Perfect!"
    };

Console.WriteLine(WeatherReport(18)); // Cold.
```

***

### Key Points

* `if` → one condition.
* `if-else` → choose between two branches.
* `else if` → chain of multiple conditions.
* `switch` → cleaner for many options.
* `switch expression` → concise value selection.
* Case guards (`when`) allow extra condition checks.

***

### FAQs

**Q: When should I use `if` vs `switch`?**

* Use `if` for a few conditions or complex logic.
* Use `switch` when you have multiple distinct cases.

**Q: Do I need `break` in switch expressions?**\
No. Switch expressions don’t use `break`—each arm must return a value.

**Q: Can I have multiple `default` cases?**\
No. Only one `default` is allowed.

**Q: Can `switch` work with types other than numbers?**\
Yes. It works with strings, enums, tuples, and patterns like `<`, `>`, `is null`.
