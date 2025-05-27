# The abstract keyword

### Introduction

The `abstract` modifier indicates clearly that a class or its member lacks a complete implementation. It's like defining a blueprint or a general concept without specifying every detail. Derived (non-abstract) classes must then explicitly provide these missing details.



### Using Abstract Classes and Methods

Consider geometric shapes: all shapes have an area, but the calculation differs for each specific shape. An abstract class can represent the general idea of "Shape," leaving specific calculations (like area) to individual shapes.

Here's how you'd clearly set up an abstract `Shape` class with a method `CalculateArea` that each derived class must implement:

```csharp
abstract class Shape
{
    // Abstract method with no implementation
    public abstract double CalculateArea();
}

class Square : Shape
{
    public double Side { get; }

    public Square(double side)
    {
        Side = side;
    }

    // Concrete implementation of abstract method
    public override double CalculateArea() => Side * Side;
}

class Circle : Shape
{
    public double Radius { get; }

    public Circle(double radius)
    {
        Radius = radius;
    }

    // Concrete implementation of abstract method
    public override double CalculateArea() => Math.PI * Radius * Radius;
}

class Program
{
    static void Main()
    {
        Shape square = new Square(5);
        Shape circle = new Circle(3);

        Console.WriteLine($"Square Area: {square.CalculateArea()}");
        Console.WriteLine($"Circle Area: {circle.CalculateArea():F2}");
    }
}
```

Output:

```bash
Square Area: 25
Circle Area: 28.27
```



***

### Abstract Properties

Abstract properties specify that derived classes must explicitly define getter and/or setter logic:

```csharp
csharpCopyEditabstract class Shape
{
    // Abstract property to be implemented by derived classes
    public abstract string Name { get; }
}

class Triangle : Shape
{
    public override string Name => "Triangle";
}

class Program
{
    static void Main()
    {
        Shape triangle = new Triangle();
        Console.WriteLine($"Shape: {triangle.Name}");
    }
}
```

Output:

```bash
Shape: Triangle
```

***

### Abstract Class Constructors

Even though abstract classes can't be instantiated, they can still have constructors—usually `protected`. These constructors help clearly initialize common data for derived classes:

```csharp
abstract class Shape
{
    public string Color { get; }

    protected Shape(string color)
    {
        Color = color;
        Console.WriteLine($"Shape created with color: {color}");
    }

    public abstract double CalculateArea();
}

class Rectangle : Shape
{
    public double Width { get; }
    public double Height { get; }

    public Rectangle(string color, double width, double height) : base(color)
    {
        Width = width;
        Height = height;
    }

    public override double CalculateArea() => Width * Height;
}

class Program
{
    static void Main()
    {
        var rectangle = new Rectangle("Blue", 4, 5);
        Console.WriteLine($"Rectangle Area: {rectangle.CalculateArea()}");
    }
}

```

Output:

```
mathematicaCopyEditShape created with color: Blue
Rectangle Area: 20
```

***

### What You **Can** and **Can't** Do with Abstract

Here's a clear breakdown:

| ✅ **You Can**                                                    | ❌ **You Can't**                                       |
| ---------------------------------------------------------------- | ----------------------------------------------------- |
| Create abstract classes and methods                              | Instantiate abstract classes directly                 |
| Define abstract properties and indexers                          | Combine abstract with the `sealed` modifier           |
| Have constructors in abstract classes (usually protected)        | Mark static methods or properties as abstract         |
| Mix abstract and non-abstract methods in the same abstract class | Have implementation bodies (`{}`) in abstract methods |

***

### Unified Key Points:

* **Abstract classes** provide incomplete implementations, acting as blueprints for derived classes.
* You **can't instantiate** abstract classes directly.
* Abstract methods/properties must be implemented explicitly in derived non-abstract classes.
* Abstract methods and properties are implicitly virtual.
* Abstract methods don't have bodies (no curly braces `{}`).
* An abstract class can include constructors, typically `protected`, to initialize common data for derived classes.
* Abstract and `sealed` modifiers are mutually exclusive—`sealed` prevents inheritance, `abstract` requires it.

***

### FAQs

**Why use an abstract class instead of an interface?**\
Use abstract classes when you want to clearly share code (fields, methods with implementations) among derived classes. Interfaces are better when you only need to enforce behavior without shared implementation.

**Can abstract methods be private?**\
No. Abstract methods must be implemented explicitly in derived classes, thus they must be accessible (`public`, `protected`, or `internal`).

**Can I declare abstract static methods or properties?**\
No. Abstract methods and properties are instance members by definition, as their implementations depend on specific class instances.

**Must derived classes implement all abstract members?**\
Yes. A derived non-abstract class **must explicitly implement all abstract methods and properties** defined in the abstract class.

**Can constructors be abstract?**\
No. Constructors can't be abstract, as they don't have explicit overriding logic. Constructors in abstract classes typically use the `protected` modifier.
