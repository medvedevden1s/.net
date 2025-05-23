# Private protected

### Introduction

The `private protected` modifier combines the behavior of both `private` and `protected`. Members declared as `private protected` are accessible **only within their own assembly** and **only by derived (child) classes**. This modifier provides strict encapsulation, clearly indicating members are exclusively for internal subclass use.

### Code Example

Continuing with our banking scenario, imagine certain sensitive calculations or operations within your base class should only be accessible by subclasses within the same project (assembly), not by external subclasses or other classes internally.

Here's a clear example demonstrating `private protected`:

```csharp
csharpCopyEditpublic class BankAccount
{
    private protected decimal CalculateBonus(decimal amount)
    {
        // Sensitive calculation logic intended only for subclasses in this assembly
        return amount * 0.05m;
    }
}

public class SavingsAccount : BankAccount
{
    public decimal ApplyBonus(decimal deposit)
    {
        var bonus = CalculateBonus(deposit);
        return deposit + bonus;
    }
}
```

### Usage Example

You can use `private protected` methods only from derived classes within the same assembly:

```csharp
csharpCopyEditvar savings = new SavingsAccount();
var totalDeposit = savings.ApplyBonus(1000m);
Console.WriteLine($"Total deposit including bonus: {totalDeposit:C}");

// savings.CalculateBonus(100m); ❌ Compile-time error: Cannot access private protected member externally
```

***

### FAQs Section

**When should I use the `private protected` modifier?**\
Use this modifier when you want certain inherited functionality to be used only by subclasses within the same assembly, preventing external subclass access.

**What's the difference between `protected internal` and `private protected`?**

* `protected internal`: Accessible from subclasses in **any assembly** OR within the same assembly (less restrictive, logical OR).
* `private protected`: Accessible only from subclasses **within the same assembly** (more restrictive, logical AND).

**Can a class be declared as `private protected`?**\
No, top-level classes can't be `private protected`. This modifier is valid only for class members (methods, fields, properties, etc.):

```csharp
csharpCopyEdit// private protected class AccountHelper {} ❌ Compile-time error
```

**Can structs or records have `private protected` members?**

* **Structs**: No, structs don't support inheritance, so members can't be `private protected`.
* **Records**: Yes, records support inheritance, allowing use of `private protected`.

```csharp
csharpCopyEditpublic record BaseRecord
{
    private protected void RecordLogic() {}
}

public record DerivedRecord : BaseRecord
{
    public void UseLogic()
    {
        RecordLogic(); // ✅ Valid usage within same assembly
    }
}
```
