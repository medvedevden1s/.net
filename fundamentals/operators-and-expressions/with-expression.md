## Introduction

The `with` expression is like a master sculptor creating a replica of a statue with a few subtle changes. Just as the sculptor doesn't destroy the original work but instead creates a new piece based on it, the `with` expression produces a copy of an object with specific properties modified. It preserves the original while giving you a new version that reflects your desired changes.

In the world of programming, modifying objects often involves a choice between mutating existing data (which can lead to side effects) or creating entirely new objects from scratch (which can be verbose and inefficient). The `with` expression offers an elegant middle ground—nondestructive mutation—allowing you to create modified copies of objects with minimal code and maximum clarity, while leaving the original untouched.

## Basic usage

A `with` expression produces a copy of its operand with the specified properties and fields modified. You use the object initializer syntax to specify what members to modify and their new values:

```csharp
using System;

public class WithExpressionBasicExample
{
    public record NamedPoint(string Name, int X, int Y);

    public static void Main()
    {
        var p1 = new NamedPoint("A", 0, 0);
        Console.WriteLine($"{nameof(p1)}: {p1}");  // output: p1: NamedPoint { Name = A, X = 0, Y = 0 }
        
        var p2 = p1 with { Name = "B", X = 5 };
        Console.WriteLine($"{nameof(p2)}: {p2}");  // output: p2: NamedPoint { Name = B, X = 5, Y = 0 }
        
        var p3 = p1 with 
            { 
                Name = "C", 
                Y = 4 
            };
        Console.WriteLine($"{nameof(p3)}: {p3}");  // output: p3: NamedPoint { Name = C, X = 0, Y = 4 }

        Console.WriteLine($"{nameof(p1)}: {p1}");  // output: p1: NamedPoint { Name = A, X = 0, Y = 0 }
    }
}
```

In this example, we create a new record `p1` and then use the `with` expression to create two new records, `p2` and `p3`, each with different modifications. Notice that the original record `p1` remains unchanged.

{% hint style="info" %}
The `with` expression is particularly useful with immutable types like records, where you want to create new instances with slight modifications without changing the original.
{% endhint %}

## Compatible types

The `with` expression can be used with several types:

1. **Record types** - The most common use case, as records are designed for immutability
2. **Structure types** - For creating modified copies of value types
3. **Anonymous types** - For working with anonymous objects

Here's an example using an anonymous type:

```csharp
var apples = new { Item = "Apples", Price = 1.19m };
Console.WriteLine($"Original: {apples}");  // output: Original: { Item = Apples, Price = 1.19 }
var saleApples = apples with { Price = 0.79m };
Console.WriteLine($"Sale: {saleApples}");  // output: Sale: { Item = Apples, Price = 0.79 }
```

## Runtime type preservation

The result of a `with` expression has the same run-time type as the expression's operand, which is particularly important when working with inheritance:

```csharp
using System;

public class InheritanceExample
{
    public record Point(int X, int Y);
    public record NamedPoint(string Name, int X, int Y) : Point(X, Y);

    public static void Main()
    {
        Point p1 = new NamedPoint("A", 0, 0);
        Point p2 = p1 with { X = 5, Y = 3 };
        Console.WriteLine(p2 is NamedPoint);  // output: True
        Console.WriteLine(p2);  // output: NamedPoint { X = 5, Y = 3, Name = A }
    }
}
```

In this example, even though `p1` is declared as a `Point`, its runtime type is `NamedPoint`. When we use the `with` expression, the result `p2` maintains the same runtime type `NamedPoint`, not just the declared type `Point`.

## Reference type behavior

When a member is a reference type, only the reference to a member instance is copied when an operand is copied. Both the copy and original operand have access to the same reference-type instance:

```csharp
using System;
using System.Collections.Generic;

public class ExampleWithReferenceType
{
    public record TaggedNumber(int Number, List<string> Tags)
    {
        public string PrintTags() => string.Join(", ", Tags);
    }

    public static void Main()
    {
        var original = new TaggedNumber(1, new List<string> { "A", "B" });

        var copy = original with { Number = 2 };
        Console.WriteLine($"Tags of {nameof(copy)}: {copy.PrintTags()}");
        // output: Tags of copy: A, B

        original.Tags.Add("C");
        Console.WriteLine($"Tags of {nameof(copy)}: {copy.PrintTags()}");
        // output: Tags of copy: A, B, C
    }
}
```

In this example, modifying the `Tags` list in the original record also affects the copy, because both records share the same list instance.

## Custom copy semantics

For record types, you can customize how copying works by defining a copy constructor. A copy constructor is a constructor with a single parameter of the containing record type, which copies the state of its argument to a new record instance.

When a `with` expression is evaluated:
1. The copy constructor is called to create a new instance based on the original
2. The new instance is then updated according to the specified modifications

By default, the copy constructor is implicit (compiler-generated), but you can explicitly declare one to customize the copying behavior:

```csharp
using System;
using System.Collections.Generic;

public class UserDefinedCopyConstructorExample
{
    public record TaggedNumber(int Number, List<string> Tags)
    {
        protected TaggedNumber(TaggedNumber original)
        {
            Number = original.Number;
            Tags = new List<string>(original.Tags);  // Create a new list with the same elements
        }

        public string PrintTags() => string.Join(", ", Tags);
    }

    public static void Main()
    {
        var original = new TaggedNumber(1, new List<string> { "A", "B" });

        var copy = original with { Number = 2 };
        Console.WriteLine($"Tags of {nameof(copy)}: {copy.PrintTags()}");
        // output: Tags of copy: A, B

        original.Tags.Add("C");
        Console.WriteLine($"Tags of {nameof(copy)}: {copy.PrintTags()}");
        // output: Tags of copy: A, B
    }
}
```

In this example, we've customized the copy constructor to create a new list for the `Tags` property, rather than sharing the reference. As a result, modifying the original's list doesn't affect the copy.

{% hint style="warning" %}
You can't customize the copy semantics for structure types. For structs, the `with` expression always performs a memberwise copy.
{% endhint %}

## Working with computed properties

When using the `with` expression with types that have computed properties, you need to be careful about how those properties are implemented:

```csharp
public record Rectangle(double Width, double Height)
{
    // Good: Computed on access
    public double Area => Width * Height;
    
    // Problematic: Computed and stored at creation time
    public double BadPerimeter { get; } = Width * 2 + Height * 2;
}

var rect1 = new Rectangle(5, 10);
var rect2 = rect1 with { Width = 7 };

Console.WriteLine(rect2.Area);         // Correct: 70 (7 * 10)
Console.WriteLine(rect2.BadPerimeter); // Incorrect: Still shows the original perimeter (30)
```

Computed properties in record types should be computed on access, not initialized when the instance is created. Otherwise, a property could return the computed value based on the original instance, not the modified copy.

## Performance considerations

The `with` expression is generally efficient, but there are some performance considerations:

1. For records, it uses the copy constructor and then updates the specified properties
2. For anonymous types, the compiler generates efficient code to create a new instance
3. For structs, it performs a memberwise copy

For large objects with many properties, only the properties you specify are updated, which is more efficient than manually creating a new instance and copying all properties.

## Key Points

- The `with` expression creates a copy of an object with specified properties modified
- It works with record types, structure types, and anonymous types
- The result has the same runtime type as the original operand
- For reference type members, only the reference is copied, not the underlying object
- You can customize copy behavior for records by defining a copy constructor
- Computed properties should be calculated on access, not at initialization
- The original object remains unchanged, making this ideal for immutable programming patterns

## FAQs

**Q: When was the with expression introduced in C#?**  
A: The `with` expression was introduced in C# 9.0, along with record types, as part of the .NET 5 release in November 2020.

**Q: Can I use the with expression with regular classes?**  
A: No, the `with` expression only works with record types, structure types, and anonymous types. For regular classes, you would need to implement similar functionality manually.

**Q: How does the with expression handle inheritance?**  
A: The `with` expression preserves the runtime type of the object, even when the declared type is a base type. This ensures that all properties, including those from derived types, are properly copied.

**Q: Is the with expression thread-safe?**  
A: The `with` expression itself is thread-safe as it creates a new instance rather than modifying the existing one. However, if your custom copy constructor has side effects, those would need their own thread safety considerations.

**Q: Can I modify nested properties with a single with expression?**  
A: No, the `with` expression only allows you to modify top-level properties. For nested modifications, you would need to use multiple `with` expressions or other approaches.

**Q: How does the with expression compare to the Builder pattern?**  
A: Both provide ways to create objects with specific property values, but the `with` expression is more concise and integrated into the language. The Builder pattern offers more control over the construction process and validation but requires more code.

**Q: Does the with expression work with nullable reference types?**  
A: Yes, the `with` expression respects nullable reference type annotations and will maintain the nullability context of the original object.