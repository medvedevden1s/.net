# The out (generic modifier)

### Introduction

In C#, the `out` keyword, when used with generic type parameters, specifies covariance. Covariance allows a generic type to return more specific (derived) types than originally specified, enabling implicit type conversions, making your code cleaner, more flexible, and easier to maintain.



### Without out modifier &#x20;

Imagine building a reporting application where each report class implements a simple, non-generic interface:

```csharp
public interface IReportGenerator
{
    Report GenerateReport();
}

public class FinancialReportGenerator : IReportGenerator
{
    public Report GenerateReport()
    {
        return new FinancialReport();
    }
}

public class SalesReportGenerator : IReportGenerator
{
    public Report GenerateReport()
    {
        return new SalesReport();
    }
}
```

#### Issues with this approach:

* You lose type-specific information since all report generators return a base `Report` type.
* It requires explicit casting if you need specific functionality from derived reports.
* Your code becomes less readable, less intuitive, and harder to test and maintain.

### How Covariance Solves These Issues

Using covariance, we can create strongly-typed interfaces that clearly indicate what type of report they return, eliminating the need for explicit casting and enhancing type safety:

```csharp
public interface IReportGenerator<out T> where T : Report
{
    T GenerateReport();
}

public class FinancialReportGenerator : IReportGenerator<FinancialReport>
{
    public FinancialReport GenerateReport()
    {
        return new FinancialReport();
    }
}

public class SalesReportGenerator : IReportGenerator<SalesReport>
{
    public SalesReport GenerateReport()
    {
        return new SalesReport();
    }
}

class Program
{
    static void Main()
    {
        IReportGenerator<FinancialReport> financialGenerator = new FinancialReportGenerator();
        IReportGenerator<Report> generalGenerator = financialGenerator; // Implicit conversion due to covariance

        Report report = generalGenerator.GenerateReport();
        Console.WriteLine(report.GetType().Name);
    }
}
```

**Output:**

```
FinancialReport
```

{% hint style="info" %}
Covariance (`out`) applies only to return types, not input parameters.

It works exclusively with reference types.
{% endhint %}

### Benefits of Covariance

* **Type Safety**: Eliminates runtime casting errors by providing compile-time checks.
* **Code Readability**: Clearly indicates the intent and specific types being used.
* **Flexibility and Extensibility**: Easier to extend and adapt your code for future changes.
* **Testability**: Simplifies unit testing by reducing type-casting complexity.

### FAQs

**Why doesn't covariance apply to input parameters?**

Covariance (`out`) specifically applies to outputs (return types), while contravariance (`in`) applies to inputs (parameters).

**Can covariance be used with value types?**

No, covariance cannot be used with value types because covariance relies on reference type inheritance. Value types in C# do not support inheritance, meaning they cannot have more derived or less derived types. Hence, covariance is only applicable to reference types where a type hierarchy exists.
