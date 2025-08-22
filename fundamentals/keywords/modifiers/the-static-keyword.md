# The static keyword

### Introduction

The `static` keyword is used to define members that belong to the type itself rather than to an instance of the type. This means there is only **one copy shared across the entire program**, instead of each object having its own copy.

You can apply `static` to:

* Fields
* Methods
* Properties
* Operators
* Events
* Constructors
* Classes (must contain only static members)
* Local functions
* Lambda expressions and anonymous methods

***

### Static Members

A static member belongs to the type, not an object. You access it through the type name, not an instance.

```csharp
public class MathHelper
{
    public static double Pi = 3.14159;

    public static double Square(double number) => number * number;
}

class Program
{
    static void Main()
    {
        Console.WriteLine(MathHelper.Pi);           // Access static field
        Console.WriteLine(MathHelper.Square(5));   // Call static method
    }
}
```

📌 Key Facts:

* Static members are **shared** across all instances.
* There’s only one copy in memory.
* They’re accessed using the **type name**, not an object.

{% hint style="warning" %}
You cannot use `this` inside static methods because they don’t belong to an instance.
{% endhint %}

***

### Static Classes

A class declared with `static` cannot be instantiated and must contain only static members.

```csharp
static class CompanyEmployee
{
    public static void DoSomething() => Console.WriteLine("Work done!");
    public static void DoSomethingElse() => Console.WriteLine("Extra work!");
}

class Program
{
    static void Main()
    {
        CompanyEmployee.DoSomething();
    }
}
```

📌 Key Facts:

* Cannot be instantiated with `new`.
* All members must be static.
* Often used for **utility/helper classes**.

***

### Static Fields And Methods

Static fields store data shared across all instances. Static methods can operate without object state.

```csharp
public class Employee
{
    public string Name;
    public string Id;
    public static int EmployeeCounter;

    public Employee(string name, string id)
    {
        Name = name;
        Id = id;
        EmployeeCounter++;
    }
}

class Program
{
    static void Main()
    {
        var e1 = new Employee("Alice", "A01");
        var e2 = new Employee("Bob", "B02");

        Console.WriteLine($"Total employees: {Employee.EmployeeCounter}");
    }
}
```

Output:

```
Total employees: 2
```

***

### Static Constructors

A static constructor initializes static members. It runs **once per type**, before the first use.

```csharp
class Logger
{
    public static string Config;

    static Logger()
    {
        Config = "Default logging configuration loaded.";
        Console.WriteLine("Static constructor executed.");
    }
}
```

📌 Key Facts:

* Called automatically, no parameters allowed.
* Executes only once per type, before first use.
* Commonly used for **initializing static fields**.

***

### Static Local Functions

You can declare local functions as `static` to ensure they don’t capture outer variables.

```csharp
class Calc
{
    public void CalculateSum()
    {
        int a = 3, b = 7;

        static int Add(int x, int y) => x + y;

        Console.WriteLine($"Sum: {Add(a, b)}");
    }
}
```

📌 Benefit: Prevents accidental dependency on instance or local variables.

***

### Static Lambdas

Lambdas can also be static, meaning they can’t capture local variables or `this`.

```csharp
class Program
{
    static void Main()
    {
        Func<int, int, int> add = static (x, y) => x + y;
        Console.WriteLine(add(5, 10));
    }
}
```

***

### Common Mistakes

* ❌ Trying to use `this` inside static members.
* ❌ Declaring instance members inside a static class.
* ❌ Expecting each object to have its own copy of a static field (it’s shared).
* ❌ Relying on order of static field initialization across different fields → results can be undefined until explicitly set.

***

### Key Points

* `static` members belong to the **type, not the instance**.
* `static` classes cannot be instantiated and can only contain static members.
* Static fields are **shared across all objects**.
* Static constructors run once, before first use.
* Static local functions and lambdas cannot capture external state.

***

### FAQs

**Q: Can a static method access instance members?**\
No. Since static methods don’t belong to an object, they can only access static members.

**Q: Are static classes common in .NET?**\
Yes. Examples include `System.Math`, `System.Console`, and `System.IO.File`.

**Q: Can generic classes have static fields?**\
Yes, but each closed generic type gets its own copy of static fields.

**Q: Can I create objects of a static class?**\
No. Static classes can’t be instantiated.

**Q: What happens if I don’t define a static constructor?**\
The compiler provides one automatically if needed, but you won’t see it.
