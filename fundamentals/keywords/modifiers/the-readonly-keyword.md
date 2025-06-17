# The readonly keyword

### Introduction

The `readonly` keword says assignment to the field can only occur as part of the declaration or in a constructor in the same class. It helps keep your data stable by preventing unintended or accidental changes after initialization, ensuring safer and more predictable behavior.

### Readonly Field

A `readonly` field can only be assigned values at the declaration point or inside a constructor. After construction, its value can't be changed, ensuring its stability throughout the object's lifetime.

```csharp
csharpCopyEditclass Car
{
    // Assigned during declaration
    public readonly string Brand = "BMW";

    // Assigned in constructor
    public readonly int Year;

    public Car(int year)
    {
        Year = year;
    }

    void UpdateYear()
    {
        // Year = 2025; // Compile-time error: readonly fields can't be modified after constructor.
    }
}
```

{% hint style="info" %}
Unlike `const`, `readonly` fields can be assigned different values in different constructors or during runtime.
{% endhint %}

#### 📌 **Key Facts**:

* Assigned only during declaration or in constructors.
* After construction, assignment is prohibited.
* Suitable for runtime constants (e.g., timestamps).

### Readonly Instance Members

In a `struct`, marking methods with `readonly` explicitly indicates that these methods won’t alter the struct’s data.

```csharp
public struct Point
{
    public int X;
    public int Y;

    public readonly double Distance()
    {
        return Math.Sqrt(X * X + Y * Y);
    }

    // Compile-time error if uncommented:
    // public readonly void Move(int dx, int dy) { X += dx; Y += dy; }
}
```

* **Method `Distance()`**:
  * This method is marked with `readonly`, clearly stating that it won't change the `Point`'s data (`X` and `Y` fields).
  * It simply calculates and returns a value based on current field values. No fields are modified here.
* **Method `Move()` (commented)**:
  * Attempting to modify fields (`X += dx; Y += dy;`) from a method marked as `readonly` causes a compile-time error.
  * This error happens because the compiler strictly enforces the immutability rule for methods marked as `readonly`.

#### Why Use `readonly` Methods?

* **Clarity**: It clearly communicates your intent—that the method is "safe" and won’t alter data.
* **Safety**: Prevents accidental modification of struct fields, helping avoid subtle bugs.
* **Optimization**: Allows the compiler to optimize your code more efficiently.

### Ref Readonly Return

A method returning a `ref readonly` reference ensures that the caller can’t modify the object it references.

```csharp
public class Example
{
    private static readonly Point _origin = new Point { X = 0, Y = 0 };

    public static ref readonly Point Origin => ref _origin;
}
```

{% hint style="info" %}
Using `ref readonly` protects data integrity by preventing callers from modifying referenced objects.
{% endhint %}

### Readonly Structs

Marking a struct as `readonly` indicates it’s fully immutable—all its fields must remain unchanged after initialization.

```csharp
csharpCopyEditpublic readonly struct ImmutablePoint
{
    public int X { get; }
    public int Y { get; }

    public ImmutablePoint(int x, int y)
    {
        X = x;
        Y = y;
    }
}
```

\{% hint style="success" %\}\
Immutable structs enhance thread safety, providing reliable, consistent data even in multi-threaded environments.\
\{% endhint %\}

### Important Security Consideration

Publicly visible `readonly` fields referring to mutable reference types can introduce potential security risks, as external code may unintentionally alter internal data.

```csharp
csharpCopyEditpublic class Risky
{
    public readonly StringBuilder Builder = new StringBuilder();
}
```

\{% hint style="danger" %\}\
Avoid exposing public `readonly` mutable fields. Always prefer immutable types (e.g., `string`, immutable collections) for public fields to ensure safety.\
\{% endhint %\}

***

### Key Points:

* **Readonly Fields**: Immutable after constructor initialization.
* **Readonly Methods**: Clearly express that instance state won’t be changed.
* **Ref Readonly Return**: Protects referenced objects from modification.
* **Readonly Struct**: Enforces full immutability for struct fields.

***

### FAQs

**Can I assign to a readonly field after the constructor?**\
No. Assignment is allowed only at declaration or within constructors.

**How is `readonly` different from `const`?**\
`const` fields require compile-time constants, while `readonly` fields can be set at runtime, allowing different values per constructor.

**Can readonly methods modify struct fields?**\
No. Marking a method as `readonly` explicitly prevents modifications to struct fields.

**Why should I use `ref readonly` returns?**\
To safely return references without risking external modification of internal state.

**Are all fields in a readonly struct immutable?**\
Yes. Marking a struct as `readonly` ensures all its fields remain unchanged after initialization.
