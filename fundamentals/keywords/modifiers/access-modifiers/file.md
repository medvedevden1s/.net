# File

### Introduction

The `file` modifier (introduced in C# 11) restricts the visibility of a type to the **source file** in which it is declared. It clearly ensures the type remains private to its file, preventing access from other files even within the same assembly.

Consider a scenario in our banking application, where you have helper logic that should only be used inside a specific file—such as internal validations or calculations. The `file` modifier is ideal for this:

Here's a clear example demonstrating the `file` modifier:

**Account.cs**

```csharp
csharpCopyEditfile class InterestCalculator
{
    public decimal CalculateInterest(decimal amount, decimal rate, int years)
        => amount * rate * years;
}

public class Account
{
    private readonly InterestCalculator _calculator = new();

    public decimal GetInterest(decimal amount, decimal rate, int years)
        => _calculator.CalculateInterest(amount, rate, years);
}
```

Attempting to use the `InterestCalculator` from another file would result in an error:

**AnotherFile.cs**

```csharp
csharpCopyEdit// var calculator = new InterestCalculator(); 
// ❌ Compile-time error: 'InterestCalculator' is inaccessible due to file modifier
```

Inside the same file, usage is straightforward:

```csharp
csharpCopyEditvar account = new Account();
var interest = account.GetInterest(1000m, 0.05m, 3);
Console.WriteLine($"Calculated Interest: {interest:C}");
```

***

### FAQs Section

**When should I use the `file` modifier?**\
Use `file` to restrict types exclusively to their source file. It's especially useful for helper classes or methods intended only within that file.

**Can methods or properties themselves be marked as `file`?**\
No. Only top-level types (classes, structs, records, interfaces, enums, delegates) can be marked with the `file` modifier.

```csharp
csharpCopyEditfile class HelperClass {}    // ✅ Valid
// file void HelperMethod() {} ❌ Compile-time error
```

**What's the difference between `file` and `private` modifiers?**

* **`file`**: Restricts type visibility to the same file, affecting the entire type.
* **`private`**: Restricts member visibility strictly within its containing type.

**Can I have multiple `file` scoped types in one source file?**\
Yes, you can have multiple file-scoped types in a single file:

```csharp
csharpCopyEditfile class HelperA {}
file class HelperB {}
```

Each type is independently restricted to this file.
