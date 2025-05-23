# The base keyword

### Introduction

The `base` keyword provides a clear way to interact with members of the parent class directly from a child class. Think of it like a hotline that connects the child directly to its parent whenever it needs something specific—like calling a parent's method or using a parent's constructor.

### Why Use the `base` Keyword?

Imagine you have two types of bank accounts: a basic account and a premium account. The premium account offers extra features but still relies on the basic account's core functionality. Using `base` helps you clearly reference methods and constructors from the parent account class, preserving essential behaviors.

### Using `base` to Call Parent Class Methods

Let's see this with a premium account that extends a basic account. The premium account adds extra functionality but wants to reuse the method from the basic account explicitly.

```csharp
csharpCopyEditpublic class Account
{
    protected decimal balance;

    public virtual void ShowBalance()
    {
        Console.WriteLine($"Current balance: {balance:C}");
    }
}

public class PremiumAccount : Account
{
    private decimal bonus;

    public PremiumAccount(decimal initialBalance, decimal bonus)
    {
        this.balance = initialBalance;
        this.bonus = bonus;
    }

    public override void ShowBalance()
    {
        base.ShowBalance(); // Explicitly calling Account's method
        Console.WriteLine($"Bonus balance: {bonus:C}");
    }
}

class Program
{
    static void Main()
    {
        PremiumAccount premium = new PremiumAccount(1500m, 200m);
        premium.ShowBalance();
    }
}
```

```bash
Output:
Current balance: $1,500.00
Bonus balance: $200.00
```

{% hint style="info" %}
Use `base` when you explicitly want the base class method to run, preserving its original behavior alongside new functionality.
{% endhint %}

### Using `base` to Call Parent Constructors

If your premium account needs specific initialization from the basic account, you explicitly call the desired constructor using `base`.

```csharp
csharpCopyEditpublic class Account
{
    protected string accountHolder;

    public Account(string holder)
    {
        accountHolder = holder;
        Console.WriteLine($"Account created for: {holder}");
    }
}

public class PremiumAccount : Account
{
    private decimal bonus;

    public PremiumAccount(string holder, decimal bonus) : base(holder) // Calling Account constructor
    {
        this.bonus = bonus;
        Console.WriteLine($"Premium features activated. Bonus: {bonus:C}");
    }

    static void Main()
    {
        PremiumAccount premium = new PremiumAccount("Alice", 200m);
    }
}
```

```bash
Output:
Account created for: Alice
Premium features activated. Bonus: $200.00
```

{% hint style="success" %}
Explicitly calling base constructors helps clearly define initialization sequences, ensuring predictable object creation.
{% endhint %}

### Key Points:

* `base` accesses methods or constructors in the parent class explicitly.
* Use it in overridden methods to retain original logic.
* Specify parent constructors to control initialization sequence explicitly.
* Can only be used within constructors, instance methods, or property accessors—not in static methods.

### FAQs

**Can I use the `base` keyword from a static method?**\
No, `base` can only be used in instance methods, constructors, or property accessors.

**Why explicitly call a base constructor?**\
To clearly specify which parent constructor runs, especially when the parent class has multiple constructors.

**What happens if I omit a base constructor call?**\
The default parameterless base constructor is automatically called if it exists. If no default constructor is present, you must explicitly specify which constructor to call using `base`.

**Can `base` access private members of the parent class?**\
No, private members aren't accessible outside their declaring class—even through `base`
