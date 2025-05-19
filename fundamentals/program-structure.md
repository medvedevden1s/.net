# Program structure

## Program Structure in C\#

In earlier lessons, you've created simple programs like **"Hello World!"**. Now, let's explore how C# programs are structured clearly in larger applications.

You'll learn:

* **Why namespaces are important**
* **How multiple files and namespaces organize code**
* **A practical example demonstrating these concepts clearly**

***

### Why Do We Need Namespaces?

Namespaces help organize code logically and clearly prevent naming conflicts. Consider the following clear example:

```csharp
namespace Finance.Calculations
{
    public class Calculator
    {
        public decimal CalculateInterest(decimal principal, decimal rate, int years)
        {
            return principal * rate * years;
        }
    }}
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

{% hint style="info" %}
Both namespaces have a class called `Calculator`

Without namespaces, this would create naming collisions

Namespaces clearly specify context, making your code readable and organized.
{% endhint %}

***

### Structuring a C# Project Clearly

Real-world C# projects are organized into multiple files, namespaces, and classes. Here's a clear project layout:

```
Calculator
├── Finance.cs
├── Physics.cs
└── Program.cs (entry point)
```

#### **Finance.cs**

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

#### **Physics.cs**

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

#### **Program.cs (Main entry point)**

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

***

### Visual Diagram: Understanding Program Structure

Here's how the structure looks visually, clearly showing relationships:

```
scssCopyEditProgram
│
├── Finance.Calculations (namespace)
│       └── Calculator (class)
│               └── CalculateInterest() (method)
│
├── Physics.Calculations (namespace)
│       └── Calculator (class)
│               └── CalculateVelocity() (method)
│
└── Main (entry point)
        ├── creates Finance Calculator instance
        ├── creates Physics Calculator instance
        └── calls methods clearly and separately
```

***

### ❓ FAQs (Frequently Asked Questions)

#### 🔹 **Why use namespaces at all?**

Namespaces clearly prevent naming collisions and logically organize related parts of your code, improving readability and maintainability.

#### 🔹 **Can multiple classes exist within the same namespace?**

Yes, it's common. A namespace groups multiple related classes clearly together.

#### 🔹 **Do namespaces need to match file or folder structure?**

Not strictly, but aligning namespaces with your project's folder structure clearly improves readability and maintainability.

#### 🔹 **Can a namespace span multiple files?**

Absolutely. Namespaces can be reopened and expanded across multiple files, clearly organizing your logic.

***

### 🚩 **Summary**

Now, you've clearly learned:

* How to logically structure a C# program.
* Why namespaces matter and how they prevent naming conflicts.
* How to effectively separate concerns clearly using multiple files.

This foundational knowledge helps you organize future C# projects cleanly, logically, and effectively.
