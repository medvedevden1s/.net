# The sealed keyword

### Introduction

The `sealed` keyword is used to restrict inheritance. It tells the compiler that no other class can extend a given class or override certain members. This is helpful when you want to prevent unwanted modifications of your class design or protect critical methods from being overridden.

There are three main contexts where `sealed` can be applied:

* On **classes**
* On **methods** and **properties** (when overriding)
* Implicitly on **structs** (all structs are sealed by default)

Let’s explore each in detail.

***

### Sealed Classes

When applied to a class, `sealed` prevents further inheritance. This ensures that no one can extend and potentially break or misuse its behavior.

```csharp
class Payment { }

sealed class CreditCardPayment : Payment
{
    public void Process()
    {
        Console.WriteLine("Processing credit card payment...");
    }
}

// ❌ Not allowed: Cannot inherit from a sealed class
// class CustomPayment : CreditCardPayment { }
```

📌 Key Facts:

* Sealed classes cannot be base classes.
* You use `sealed` when you want to lock the design of a class and ensure it is used only as-is.
* Common with classes that are **optimized for performance** or **security-sensitive**.

{% hint style="success" %}
Structs are always sealed. They cannot be inherited.
{% endhint %}

***

### Sealed Methods And Properties

You can seal an overridden method or property to prevent further overriding in derived classes. This is useful when you want to allow inheritance but lock down specific behaviors.

```csharp
class Report
{
    public virtual void Generate() => Console.WriteLine("Generic Report");
}

class SalesReport : Report
{
    // Sealed override: Cannot be overridden further
    public sealed override void Generate() => Console.WriteLine("Sales Report");

    public virtual void Export() => Console.WriteLine("Exporting Sales Report...");
}

class DetailedSalesReport : SalesReport
{
    // ❌ Not allowed: Generate is sealed in SalesReport
    // public override void Generate() { }

    // ✅ Allowed: Export is still virtual
    public override void Export() => Console.WriteLine("Exporting Detailed Sales Report...");
}
```

📌 Key Facts:

* `sealed` must always be combined with `override`.
* It applies only to **virtual members** from a base class.
* Helps control which parts of your class hierarchy remain flexible.

***

### Why Use Sealed?

When designing classes, ask yourself:

* Should developers be able to extend this class to change behavior?
* Could overriding break important logic or cause security issues?
* Is performance critical? (Sealed classes allow certain runtime optimizations.)

{% hint style="success" %}
Sealed classes and members make your code safer, faster, and more predictable.
{% endhint %}

***

### Common Errors

* `abstract` and `sealed` cannot be used together on a class. Abstract requires inheritance, while sealed forbids it.
*   Attempting to inherit from a sealed class results in:

    > _"Cannot derive from sealed type"_
*   Attempting to override a sealed method results in:

    > _"Cannot override inherited member because it is sealed"_

***

### Key Points

* `sealed` class → cannot be inherited.
* `sealed` method/property → cannot be overridden further.
* `sealed` must always be paired with `override` when applied to members.
* Structs are sealed by default.
* Improves **design safety** and can improve **runtime performance**.

***

### FAQs

**Q: Can I create an object of a sealed class?**\
Yes, you can instantiate it like any other class.

**Q: Why can’t a class be both `abstract` and `sealed`?**\
Because `abstract` requires subclasses to implement missing members, while `sealed` forbids subclasses entirely.

**Q: Are sealed classes common in the .NET Framework?**\
Yes. Many framework classes (like `System.String`) are sealed to guarantee consistent and safe behavior.

**Q: Can I use sealed on interface methods?**\
No. Interfaces don’t have inheritance restrictions — they only define contracts.
