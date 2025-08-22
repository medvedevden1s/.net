# The lock statement

### Introduction

The `lock` statement ensures **exclusive access** to a shared resource by synchronizing code execution across multiple threads. Only one thread can enter a `lock` block at a time, while other threads wait until the lock is released. This prevents **race conditions** and guarantees **thread safety**.

***

### Basic Syntax

```csharp
lock (x)
{
    // Critical section: only one thread at a time
}
```

* `x` must be a reference type (commonly `object` or `System.Threading.Lock`).
* The compiler generates code that guarantees the lock is released even if an exception occurs.

{% hint style="info" %}
Starting with **.NET 9 / C# 13**, `System.Threading.Lock` is recommended for best performance and safety. In earlier versions, use a dedicated `object` instance.
{% endhint %}

***

### How It Works Under The Hood

If `x` is `System.Threading.Lock`, the compiler generates:

```csharp
using (x.EnterScope())
{
    // Critical section
}
```

Otherwise, it’s equivalent to:

```csharp
object __lockObj = x;
bool __lockWasTaken = false;
try
{
    System.Threading.Monitor.Enter(__lockObj, ref __lockWasTaken);
    // Critical section
}
finally
{
    if (__lockWasTaken) System.Threading.Monitor.Exit(__lockObj);
}
```

***

### Guidelines for Using `lock`

* Always lock on a **dedicated object**, never on:
  * `this` → external callers could also lock it.
  * `typeof(SomeType)` → globally accessible via reflection.
  * `string` literals → interned, may be shared unexpectedly.
* Keep the lock **scope short** → holding locks too long increases contention.
* Use **one lock object per shared resource** → prevents accidental deadlocks.

{% hint style="warning" %}
You cannot use `await` inside a `lock` statement.

If async synchronization is needed, use `SemaphoreSlim` with `await` instead.
{% endhint %}

***

### Example: Safe Bank Account Updates

```csharp
using System;
using System.Threading.Tasks;

public class Account
{
    // In C# 13+ use System.Threading.Lock
    private readonly System.Threading.Lock _balanceLock = new();
    private decimal _balance;

    public Account(decimal initialBalance) => _balance = initialBalance;

    public decimal Debit(decimal amount)
    {
        if (amount < 0)
            throw new ArgumentOutOfRangeException(nameof(amount));

        decimal appliedAmount = 0;
        lock (_balanceLock)
        {
            if (_balance >= amount)
            {
                _balance -= amount;
                appliedAmount = amount;
            }
        }
        return appliedAmount;
    }

    public void Credit(decimal amount)
    {
        if (amount < 0)
            throw new ArgumentOutOfRangeException(nameof(amount));

        lock (_balanceLock)
        {
            _balance += amount;
        }
    }

    public decimal GetBalance()
    {
        lock (_balanceLock)
        {
            return _balance;
        }
    }
}

class AccountTest
{
    static async Task Main()
    {
        var account = new Account(1000);
        var tasks = new Task[100];

        for (int i = 0; i < tasks.Length; i++)
        {
            tasks[i] = Task.Run(() => Update(account));
        }

        await Task.WhenAll(tasks);
        Console.WriteLine($"Account's balance is {account.GetBalance()}");
        // Output: Account's balance is 2000
    }

    static void Update(Account account)
    {
        decimal[] amounts = { 0, 2, -3, 6, -2, -1, 8, -5, 11, -6 };
        foreach (var amount in amounts)
        {
            if (amount >= 0)
                account.Credit(amount);
            else
                account.Debit(Math.Abs(amount));
        }
    }
}
```

***

### Key Points

* `lock` ensures **mutual exclusion** (only one thread executes the block).
* Works by calling `Monitor.Enter` / `Monitor.Exit` behind the scenes.
* Always use a **dedicated lock object** for each shared resource.
* Avoid locking `this`, `typeof`, or `string`.
* Use **`System.Threading.Lock` in C# 13+** for best performance.
* Do not use `await` inside a `lock`.

***

### FAQs

**Q: Can I lock on multiple objects at once?**\
Yes, but it must be done carefully in a consistent order to avoid deadlocks.

**Q: What happens if a thread crashes inside a `lock`?**\
The `finally` block ensures the lock is released even if an exception occurs.

**Q: Is `lock` re-entrant?**\
Yes. The same thread can enter the same lock multiple times without blocking itself.

**Q: Should I lock on static data?**\
Yes, but use a **dedicated static lock object** to protect shared static resources.

**Q: How is `lock` different from `SemaphoreSlim`?**\
`lock` is **synchronous** only. `SemaphoreSlim` supports async/await and controlling concurrency (more than one thread at a time).
