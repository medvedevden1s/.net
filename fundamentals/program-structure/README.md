# Program structure

### Program Structure

C# programs consist of one or more files. Each file contains zero or more namespaces. A namespace contains types such as classes, structs, interfaces, enumerations, and delegates, or other namespaces. The following example clearly shows the basic structure of a C# program using namespaces and classes:

```csharp
using System;
using Finance.Calculations;
using Physics.Calculations;

class Program
{
    static void Main()
    {
        var financeCalc = new Finance.Calculations.Calculator();
        decimal interest = financeCalc.CalculateInterest(1000m, 0.05m, 3);
        Console.WriteLine($"Calculated Interest: {interest:C}");

        var physicsCalc = new Physics.Calculations.Calculator();
        double velocity = physicsCalc.CalculateVelocity(150, 3);
        Console.WriteLine($"Calculated Velocity: {velocity} m/s");
    }
}
```

Here’s how multiple namespaces clearly organize different functionalities in separate files:

```csharp
namespace Finance.Calculations
{
    public class Calculator
    {
        public decimal CalculateInterest(decimal principal, decimal rate, int years)
        {
            return principal * rate * years;
        }
    }
}
```

```csharp
namespace Physics.Calculations
{
    public class Calculator
    {
        public double CalculateVelocity(double distance, double time)
        {
            return distance / time;
        }
    }
}
```



### ❓FAQs

**What exactly is a namespace in C#?**\
A namespace is a logical container used to organize classes, interfaces, structs, and other types clearly and avoid naming conflicts.

**Can multiple classes share the same namespace?**\
Yes. It's common and logical to group related classes together within a single namespace.

**Should the namespace structure match the project's folder structure?**\
It's recommended for readability and easy navigation, although it's not strictly necessary.

**Can one namespace span across multiple files?**\
Absolutely. You can split namespaces across multiple files to logically group and manage large projects.

### 🚩 Summary

You’ve clearly learned about:

* The basic structure of C# programs.
* How namespaces organize and separate different functionalities.
* Practical examples demonstrating a clear project structure.

This knowledge helps you create organized, maintainable, and scalable C# projects.
