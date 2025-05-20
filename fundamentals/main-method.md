# Main Method

C# applications need a clear entry point—where the program starts execution. Two primary ways to define this are:

* **Main Method** (Traditional)
* **Top-level Statements** (Modern)

Let's explore each approach clearly to help you understand when and how to use them effectively.

### The Main Method (Traditional)

The Main method clearly defines where your application starts. It's the conventional way to set the execution point of a C# application.

```csharp
using System;

class Program
{
    static void Main(string[] args)
    {
        Console.WriteLine("Arguments passed:");
        foreach (var arg in args)
        {
            Console.WriteLine(arg);
        }
    }
}
```

Run your program with:

```
dotnet run -- arg1 arg2 arg3
```

#### 📌  Key Facts:

* Must be `static`.
* Must reside within a `class` or `struct`.
* Can accept optional command-line arguments (`string[] args`).
* Can return `void`, `int`, `Task`, or `Task<int>`

***

### Top-Level Statements (Modern)

Since C# 9, you can simplify your entry point without explicitly declaring the Main method.

```csharp
using System;
using Finance.Calculations;
using Physics.Calculations;

var financeCalc = new Calculator();
decimal interest = financeCalc.CalculateInterest(1000m, 0.05m, 3);
Console.WriteLine($"Calculated Interest: {interest:C}");

var physicsCalc = new Calculator();
double velocity = physicsCalc.CalculateVelocity(150, 3);
Console.WriteLine($"Calculated Velocity: {velocity} m/s");
```

#### 📌 Important Points:

* Only one file in your project can use top-level statements.
* The compiler implicitly generates the Main method for you.
* Explicit Main declaration isn't allowed alongside top-level statements.

### ❓ FAQs

#### Why must the Main method be static?

The runtime calls Main directly without creating an object instance, hence it must be static.

#### Can a program have multiple Main methods?

Yes, but you must clearly specify one entry point using the `StartupObject` compiler option.

#### Can the Main method be asynchronous?

Absolutely, here's a quick example:

```csharp
static async Task Main(string[] args)
{
    await SomeAsyncMethod();
}
```

#### Is the args parameter mandatory?

No, it's optional. Use only if command-line interaction is necessary.
