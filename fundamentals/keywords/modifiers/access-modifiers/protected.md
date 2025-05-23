# Protected

### Introduction

The `protected` modifier limits the visibility of class members so they can be accessed **only within their own class or from derived classes**. This modifier clearly indicates that certain members are specifically designed for inheritance scenarios, allowing subclasses to reuse or modify base class logic.

In a banking scenario, suppose you have a general `BankAccount` class with shared behavior, but specialized accounts (like `SavingsAccount`) that extend functionality. You want subclasses to access certain members directly while still restricting access from outside.

Here's a practical example:

```csharp
csharpCopyEditpublic class BankAccount
{
    protected decimal Balance;

    public BankAccount(decimal initialBalance)
    {
        Balance = initialBalance;
    }

    public void Deposit(decimal amount)
    {
        if (amount > 0)
            Balance += amount;
    }

    public virtual bool Withdraw(decimal amount)
    {
        if (amount <= 0 || amount > Balance)
            return false;

        Balance -= amount;
        return true;
    }
}

public class SavingsAccount : BankAccount
{
    private const decimal MinimumBalance = 100m;

    public SavingsAccount(decimal initialBalance) : base(initialBalance) { }

    public override bool Withdraw(decimal amount)
    {
        if (Balance - amount < MinimumBalance)
            return false;

        return base.Withdraw(amount);
    }
}
```

### Usage Example

Notice that `Balance` is not accessible externally, but a subclass can use it:

```csharp
csharpCopyEditvar savings = new SavingsAccount(500m);
savings.Deposit(200m);

bool success = savings.Withdraw(550m);
Console.WriteLine($"Withdrawal successful: {success}");

// savings.Balance = 300m; ❌ Compile-time error: Cannot access protected member from outside the class hierarchy
```

***

### FAQs Section

**When should I declare members as protected?**\
Use the protected modifier when designing a base class whose members should be accessible or overridable by subclasses, but not exposed publicly.

**Can protected members be accessed from other instances of the same class or derived class?**\
Yes. Protected members can be accessed from instances of the same class or any derived class.

```csharp
csharpCopyEditclass Base
{
    protected int value;

    public bool HasSameValue(Base other)
    {
        return this.value == other.value; // ✅ Valid access
    }
}
```

**Can a struct have protected members?**\
No. Structs cannot have protected members since structs do not support inheritance.

```csharp
csharpCopyEditstruct Coordinate
{
    // protected int X; ❌ Compile-time error
}
```

**Can constructors be protected?**\
Yes. Protected constructors allow only subclasses to create instances, preventing direct instantiation externally.

```csharp
csharpCopyEditpublic class Vehicle
{
    protected Vehicle() { }
}

public class Car : Vehicle
{
    public Car() : base() { }
}

// Vehicle vehicle = new Vehicle(); ❌ Compile-time error: protected constructor
Car car = new Car(); // ✅ Valid
```
