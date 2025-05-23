# Protected internal

### Introduction

The `internal` modifier allows members and types to be accessible **anywhere within the same assembly (project)**, but not from external assemblies or projects. This is especially useful for encapsulating code logic clearly within the boundaries of a specific assembly.

In our banking application, certain components—like internal logging or calculation logic—may only be relevant inside the assembly and should remain hidden from external users or projects.

Here's a clear example demonstrating the use of `internal`:

```csharp
csharpCopyEdit// Internal helper class, accessible only within this project
internal class TransactionLogger
{
    public void Log(string message)
    {
        Console.WriteLine($"Transaction Log: {message}");
    }
}

// Public class that uses internal class internally
public class BankAccount
{
    private decimal _balance;
    private readonly TransactionLogger _logger = new();

    public BankAccount(decimal initialBalance)
    {
        _balance = initialBalance;
        _logger.Log($"Account created with initial balance {initialBalance:C}");
    }

    public void Deposit(decimal amount)
    {
        if (amount <= 0) return;

        _balance += amount;
        _logger.Log($"Deposited {amount:C}");
    }

    public decimal GetBalance() => _balance;
}
```

### Usage Example

Within the same assembly, you can freely access internal classes and members:

```csharp
csharpCopyEditvar account = new BankAccount(1000m);
account.Deposit(500m);
Console.WriteLine($"Current Balance: {account.GetBalance():C}");

// TransactionLogger logger = new(); // ✅ Accessible here (if within same assembly)
// logger.Log("Testing"); 
```

However, from an external project or assembly, internal classes and members are inaccessible:

```csharp
csharpCopyEdit// TransactionLogger externalLogger = new(); // ❌ Compile-time error, inaccessible externally
```

***

### FAQs Section

**When should I use `internal` over `public`?**\
Use `internal` when you want to expose classes or members throughout your assembly but not externally. Use `public` only if you intend external assemblies to access them.

**What's the default accessibility for top-level classes without modifiers?**\
Top-level classes are `internal` by default if you don't specify a modifier explicitly.

```csharp
csharpCopyEditclass Customer {}  // implicitly internal
internal class Order {} // explicitly internal
```

**Can I access internal members from another assembly?**\
By default, no. However, you can explicitly allow this by using the `[InternalsVisibleTo]` attribute in your project's assembly definition.

```csharp
csharpCopyEdit// AssemblyInfo.cs
[assembly: InternalsVisibleTo("MyOtherAssembly")]
```

**Can structs or records be internal?**\
Yes, structs, records, enums, and delegates can also use the `internal` modifier just like classes:

```csharp
csharpCopyEditinternal struct Coordinate
{
    public int X { get; set; }
    public int Y { get; set; }
}
```
