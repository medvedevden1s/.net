# Public

### Introduction

The `public` modifier provides the highest level of accessibility, allowing unrestricted access to methods, properties, fields, and other members. Members marked as public are accessible from anywhere within your codebase or from external assemblies referencing your code.

Continuing our banking example, you need certain operations like deposits, withdrawals, or checking balances to be publicly accessible. Here’s a clear example using the `public` modifier:

```csharp
csharpCopyEditpublic class Account
{
    private decimal _balance;

    public Account(decimal initialBalance)
    {
        _balance = initialBalance;
    }

    public void Deposit(decimal amount)
    {
        if (amount > 0)
            _balance += amount;
    }

    public bool Withdraw(decimal amount)
    {
        if (amount <= 0 || amount > _balance)
            return false;

        _balance -= amount;
        return true;
    }

    public decimal GetBalance() => _balance;
}
```

You can access public methods externally with ease:

```csharp
csharpCopyEditvar myAccount = new Account(1500m);
myAccount.Deposit(500m);
bool success = myAccount.Withdraw(200m);

Console.WriteLine($"Transaction successful: {success}, Balance: {myAccount.GetBalance():C}");
```

***

### FAQs Section

**When should I declare a member public?**\
Declare a member as public whenever external access is required. This typically includes methods and properties designed to interact with other parts of your application or external code.

**Are class members public by default?**\
No. If you don't specify an access modifier explicitly, class members are `private` by default, restricting access to within the class itself.

```csharp
csharpCopyEditclass Example
{
    int count;             // implicitly private
    public int CountPublic; // explicitly public
}
```

**Can public methods access private members in the same class?**\
Yes. Public methods can access all members of their own class, including private ones.

```csharp
csharpCopyEditpublic class Person
{
    private string _name = "John";

    public string GetName() => _name; // ✅ Accesses private member
}
```

**Can constructors be public?**\
Absolutely, constructors often are public to enable external object creation:

```csharp
csharpCopyEditpublic class Customer
{
    public Customer(string name)
    {
        Name = name;
    }

    public string Name { get; }
}

// Usage
var customer = new Customer("John Doe");
```
