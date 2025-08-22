# The virtual keyword

### Introduction

The `virtual` keyword allows a class member (method, property, indexer, or event) to be **overridden** in a derived class. This is one of the key features that enables **polymorphism**—different classes providing different behavior for the same method call.

By default, members are **non-virtual**, meaning they cannot be overridden. To allow customization in derived classes, you must declare them with `virtual`.

***

### Virtual Methods

When you declare a method as `virtual`, derived classes can use the `override` keyword to provide a new implementation. At runtime, the **most derived implementation** is executed.

```csharp
class Shape
{
    public virtual double Area()
    {
        return 0;
    }
}

class Rectangle : Shape
{
    private readonly double _width;
    private readonly double _height;

    public Rectangle(double width, double height)
    {
        _width = width;
        _height = height;
    }

    public override double Area()
    {
        return _width * _height;
    }
}
```

Here, `Area()` in `Rectangle` overrides the base `Shape.Area()`.\
At runtime, the `Rectangle` implementation runs—even if the object is referenced as `Shape`.

***

### Virtual Properties

Like methods, properties can also be virtual:

```csharp
class User
{
    public virtual string Name { get; set; } = "Guest";
}

class Admin : User
{
    public override string Name
    {
        get => base.Name + " (Admin)";
        set => base.Name = value;
    }
}
```

This lets derived classes customize property access logic.

***

### How It Works

When a virtual method is called:

1. The **runtime checks the object’s actual type**.
2. If the derived class has an `override`, that implementation is executed.
3. If not, the base implementation is used.

This enables flexible and extensible code, especially for frameworks and libraries.

{% hint style="info" %}
Unlike `abstract` methods, virtual members can provide a **default implementation**. Derived classes may choose to override it—or not.
{% endhint %}

***

### Example: Shape Calculations

```csharp
class Shape
{
    public const double PI = Math.PI;
    protected double _x, _y;

    public Shape(double x = 0, double y = 0)
    {
        _x = x;
        _y = y;
    }

    public virtual double Area()
    {
        return _x * _y;
    }
}

class Circle : Shape
{
    public Circle(double r) : base(r, 0) { }

    public override double Area() => PI * _x * _x;
}

class Sphere : Shape
{
    public Sphere(double r) : base(r, 0) { }

    public override double Area() => 4 * PI * _x * _x;
}

class Cylinder : Shape
{
    public Cylinder(double r, double h) : base(r, h) { }

    public override double Area() => 2 * PI * _x * _x + 2 * PI * _x * _y;
}

class Program
{
    static void Main()
    {
        double r = 3.0, h = 5.0;
        Shape c = new Circle(r);
        Shape s = new Sphere(r);
        Shape l = new Cylinder(r, h);

        Console.WriteLine($"Area of Circle   = {c.Area():F2}");
        Console.WriteLine($"Area of Sphere   = {s.Area():F2}");
        Console.WriteLine($"Area of Cylinder = {l.Area():F2}");
    }
}
```

**Output:**

```
Area of Circle   = 28.27
Area of Sphere   = 113.10
Area of Cylinder = 150.80
```

***

### Key Points

* `virtual` makes a member overridable.
* Derived classes use `override` to change behavior.
* Virtual methods enable **runtime polymorphism**.
* By default, members are **non-virtual**.
* Virtual members can provide default implementations (unlike `abstract`).

***

### FAQs

**Q: Can I declare constructors as virtual?**\
No. Constructors can’t be virtual because they are used to initialize objects, not to provide polymorphic behavior.

**Q: What’s the difference between `virtual` and `abstract`?**

* `virtual`: provides a default implementation, overriding is optional.
* `abstract`: no implementation, must be overridden.

**Q: Can I prevent further overriding?**\
Yes, use the [sealed ](the-sealed-keyword.md)keyword on an override method to stop further overrides in derived classes.

**Q: Are methods virtual by default?**\
No. In C#, methods are non-virtual unless explicitly marked `virtual`, `abstract`, or defined in an interface.
