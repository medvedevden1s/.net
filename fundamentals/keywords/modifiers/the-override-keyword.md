# The override keyword

### Introduction

The `override` keyword allows a derived class to **replace** a base class member—like a method or property—with its own version. This is especially useful when working with **abstract** or **virtual** members, where polymorphic behavior is needed.

Using `override`, you can customize behavior while still maintaining a common interface defined in a base class.

### Overriding Virtual Or Abstract Members

When a base class declares a method, property, indexer, or event as `virtual` or `abstract`, it allows derived classes to `override` that member.

```csharp
abstract class Shape
{
    public abstract double GetArea();
}

class Square : Shape
{
    private readonly int _sideLength = 4;

    public override double GetArea() => _sideLength * _sideLength;
}

class Circle : Shape
{
    private readonly double _radius;

    public Circle(double radius)
    {
        _radius = radius;
    }

    public override double GetArea() => Math.PI * _radius * _radius;
}

class Program
{
    static void Main()
    {
        Shape square = new Square();
        Shape circle = new Circle(3);

        Console.WriteLine($"Square area: {square.GetArea()}");     // 16
        Console.WriteLine($"Circle area: {circle.GetArea():F2}");  // 28.27
    }
}

```

###

```csharp
class Employee
{
    public string Name { get; }
    protected decimal _baseSalary;

    public Employee(string name, decimal baseSalary)
    {
        Name = name;
        _baseSalary = baseSalary;
    }

    public virtual decimal CalculateSalary() => _baseSalary;
}

class SalesEmployee : Employee
{
    private decimal _salesBonus;

    public SalesEmployee(string name, decimal baseSalary, decimal salesBonus)
        : base(name, baseSalary)
    {
        _salesBonus = salesBonus;
    }

    public override decimal CalculateSalary() => _baseSalary + _salesBonus;
}

var alice = new SalesEmployee("Alice", 1000, 500);
var bob = new Employee("Bob", 1200);

Console.WriteLine($"{alice.Name} earned: {alice.CalculateSalary()}"); // 1500
Console.WriteLine($"{bob.Name} earned: {bob.CalculateSalary()}");     // 1200
```

{% hint style="warning" %}
If you forget to use `override`, the compiler treats it as a new method, which may cause bugs.
{% endhint %}

```csharp
class Animal
{
    public virtual void Speak()
    {
        Console.WriteLine("Animal speaks");
    }
}

class Dog : Animal
{
    // Forgot to use override!
    public void Speak()
    {
        Console.WriteLine("Dog barks");
    }
}
```

```csharp
Animal pet = new Dog();
pet.Speak(); // ❌ Output: Animal speaks
```

We expected "Dog barks", right? But because `Dog.Speak()` does **not override** the base method, it hides it instead. The call goes to `Animal.Speak()`.

### Key Points:

* `override` replaces a virtual/abstract member from a base class.
* Required for polymorphism to work correctly.
* Only works with instance members, not static.

### FAQs

**Can I override a non-virtual method?**\
No. Only `virtual`, `abstract` methods can be overridden.

**Can I make an override method private?**\
No. Access level must match the base member.

**Can constructors be overridden?**\
No. Constructors are never inherited, so they cannot be overridden.

**Can I override a method with different parameters?**\
No. The method signature must match exactly.

**How is override different from new?**\
`override` replaces the base method in runtime.\
`new` hides the base method and only works through the derived type reference.
