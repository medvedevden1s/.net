# Private

### Introduction

The `private` modifier limits the visibility of a member exclusively to the class or struct in which it’s declared. It ensures encapsulation, prevents unintended access, and helps manage the complexity of your classes by clearly hiding internal logic.

### Code Example

Consider a banking application. You wouldn’t want account balances modified freely from outside the class, as it could lead to critical errors or fraud. Using the `private` modifier, you protect this critical data.

```csharp
class Account
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

#### Usage Example

```csharp
var myAccount = new Account(1000m);
myAccount.Deposit(200m);
bool success = myAccount.Withdraw(150m);
Console.WriteLine($"Transaction successful: {success}, Balance: {myAccount.GetBalance():C}");

// myAccount._balance = 500; ❌ Compile-time error: Cannot access private field
```

> Always keep sensitive or internal fields private and expose them through methods or properties.

***

### Private Members in Interfaces

Interface members are implicitly public. However, starting from C# 8.0, interfaces can define private methods **only when providing default implementations**. Private interface methods help avoid code repetition and encapsulate internal logic clearly:

```csharp
public interface ILogger
{
    void Log(string message);

    // ✅ Private method with default implementation (C# 8+)
    private void LogInternal(string formattedMessage)
    {
        Console.WriteLine($"Internal Log: {formattedMessage}");
    }

    // ✅ Public method that utilizes private method
    void LogError(string error)
    {
        LogInternal($"Error: {error}");
    }
}
```

***

### Private Modifier and Top-level Classes

Top-level classes can never be private, as they'd be inaccessible and unusable. They must instead use `public`, `internal`, or `file` (since C# 11):

```csharp
// ✅ Public class
public class Customer { }

// ✅ Internal class
internal class Order { }

// ✅ File-scoped class (C# 11+)
file class Payment { }

// ❌ Private class (invalid)
// private class Product { } // Compile-time error
```

***

### FAQs Section

**Can I declare a private method inside a public method?**\
No. Methods cannot be nested directly. Instead, declare private methods alongside other class methods and call them from public methods.

**Is it possible to access a private member from another instance of the same class?**\
Yes, private members are accessible from any instance within the same class definition:

```csharp
class Person
{
    private int age;

    public bool IsOlderThan(Person other)
    {
        return this.age > other.age; // ✅ Valid access
    }
}
```

**Can structs have private members?**\
Yes, structs can have private fields or methods. It works similarly to classes.

```csharp
struct Coordinate
{
    private int x;
    private int y;

    public Coordinate(int x, int y)
    {
        this.x = x;
        this.y = y;
    }
}
```

**Can constructors be private?**\
Absolutely, private constructors are commonly used in singleton patterns or to control instance creation:

```csharp
class Singleton
{
    private static Singleton? _instance;
    private Singleton() { }

    public static Singleton Instance => _instance ??= new Singleton();
}
```
