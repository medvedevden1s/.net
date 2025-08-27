# Equality operators - test if two objects are equal or not

### Introduction

Equality operators help you answer a simple question: _are these two things the same?_\
There are two operators:

* `==` — returns `true` when operands are equal
* `!=` — returns `true` when operands are **not** equal

The tricky part is: what does “equal” mean?

* For **value types** (like `int`, `char`, `struct`), it means _contents are equal_.
* For **reference types** (like `class`, `string`, `array`, `delegate`), it usually means _references point to the same object_, unless special rules apply.

Let’s go step by step with clear examples.

{% hint style="info" %}
For constant checks (`x == 0`), pattern matching with `is` works too: if (x is 0)
{% endhint %}

***

### Value Types: Compare By Value

```csharp
int a = 1 + 2 + 3;
int b = 6;
Console.WriteLine(a == b); // True

char c1 = 'a';
char c2 = 'A';
Console.WriteLine(c1 == c2);                 // False
Console.WriteLine(c1 == char.ToLower(c2));   // True
```

#### NaN Gotcha

```csharp
double x = double.NaN;
double y = double.NaN;

Console.WriteLine(x == y);   // False
Console.WriteLine(x != y);   // True
```

This feels strange: _how can something not equal to itself?_

* `NaN` = _Not a Number_ (example: `0.0 / 0.0`).
* By IEEE floating-point rules, `NaN` is **unordered**.
* That means:
  * `NaN == NaN` → false
  * `NaN != NaN` → true
  * `NaN < any` → false
  * `NaN > any` → false

{% hint style="warning" %}
Always check `double.IsNaN(value)` instead of using `==`.
{% endhint %}

***

### Enums: Compare By Underlying Value

```csharp
enum OrderStatus { Pending = 1, Shipped = 2, Delivered = 3 }

var s1 = OrderStatus.Pending;
var s2 = OrderStatus.Shipped;
var s3 = OrderStatus.Pending;

Console.WriteLine(s1 == s2); // False (1 != 2)
Console.WriteLine(s1 == s3); // True  (1 == 1)
```

Works fine for simple enums. But if you build a **custom struct-like enum**, implement `IEquatable<T>` to make `==` consistent:

```csharp
public readonly struct Status : IEquatable<Status>
{
    public int Code { get; }
    public Status(int code) => Code = code;

    public bool Equals(Status other) => Code == other.Code;
    public override bool Equals(object? obj) => obj is Status s && Equals(s);
    public override int GetHashCode() => Code.GetHashCode();

    public static bool operator ==(Status left, Status right) => left.Equals(right);
    public static bool operator !=(Status left, Status right) => !left.Equals(right);
}

var a = new Status(1);
var b = new Status(2);
var c = new Status(1);

Console.WriteLine(a == b); // False
Console.WriteLine(a == c); // True
```

***

### Reference Types: Compare By Reference

```csharp
class Customer { public int Id; public Customer(int id) => Id = id; }

var a = new Customer(1);
var b = new Customer(1);
var c = a;

Console.WriteLine(a == b); // False (different objects)
Console.WriteLine(a == c); // True  (same object reference)
```

By default, classes use **reference equality**.

If a class **overloads `==`**, use `object.ReferenceEquals(x, y)` to check if two variables point to the exact same object.

{% hint style="warning" %}
If you overload `==`, also overload `!=` and implement `Equals` and `GetHashCode`. Inconsistent equality causes bugs in `Dictionary`, `HashSet`, and LINQ.
{% endhint %}

***

### Records: Value-Like Equality

```csharp
public record Point(int X, int Y, string Name);
public record TaggedNumber(int Number, List<string> Tags);

var p1 = new Point(2, 3, "A");
var p2 = new Point(2, 3, "A");
Console.WriteLine(p1 == p2); // True

var n1 = new TaggedNumber(2, new List<string> { "A" });
var n2 = new TaggedNumber(2, new List<string> { "A" });
Console.WriteLine(n1 == n2); // False
```

Records compare **field-by-field**.\
But if a field is a **reference type** (like `List<string>`), equality is by **reference**, not contents.

Use `SequenceEqual` for lists:

```csharp
Console.WriteLine(n1.Tags.SequenceEqual(n2.Tags)); // True
```

***

### Strings: Case-Sensitive Ordinal Equality

```csharp
string s1 = "hello!";
string s2 = "HeLLo!";

Console.WriteLine(s1 == s2);          // False (case differs)
Console.WriteLine(s1 == s2.ToLower()); // True
```

`==` is **case-sensitive ordinal**.\


For case-insensitive:

```csharp
Console.WriteLine(string.Equals(s1, s2, StringComparison.OrdinalIgnoreCase)); // True
```

***

### Tuples: Component-Wise Equality

```csharp
var t1 = (X: 1, Y: "a");
var t2 = (X: 1, Y: "a");

Console.WriteLine(t1 == t2); // True
```

Each element is compared in order.

***

### Delegates: Equal If Invocation Lists Match

```csharp
Action a = () => Console.WriteLine("Hi");
Action b = () => Console.WriteLine("Hi");

Console.WriteLine(a == b); // False (different objects)
```

Each lambda creates a new delegate instance.

But:

```csharp
Action x = a + a;
Action y = a + a;

Console.WriteLine(x == y); // True (same invocation list: a, a)
```

***

### Boxed Values: Why `object` May Be False

```csharp
object o1 = 1;
object o2 = 1;

Console.WriteLine(o1 == o2);        // False (different boxes)
Console.WriteLine((int)o1 == (int)o2); // True
```

When you **box** a value type into an `object`, a new object is created. So two `object`s containing the same value are different references.

***

### Inequality `!=`

```csharp
int a = 1 + 1 + 2 + 3;
int b = 6;
Console.WriteLine(a != b); // True

string s1 = "Hello";
string s2 = "Hello";
Console.WriteLine(s1 != s2); // False
```

For built-in types: `x != y` is the same as `!(x == y)`.

***

### Nullable Types & Null

```csharp
string? s = null;
Console.WriteLine(s == null); // True

int? n1 = null, n2 = null;
Console.WriteLine(n1 == n2);  // True
Console.WriteLine(n1 != n2);  // False
```

Nullable comparisons behave intuitively.

***

### Key Points

* `==`/`!=` test equality/inequality.
* Value types → compare by contents.
* Classes → compare by reference (unless overloaded).
* Records → value-like equality, but reference fields still compare by reference.
* Strings → case-sensitive ordinal by default.
* Tuples → element-by-element comparison.
* Delegates → equal only if invocation lists match.
* `NaN` is never equal to anything, even itself.
* Boxed values are different objects even if contents match.
* If you overload `==`, also overload `!=`, implement `Equals` and `GetHashCode`.
* Use `ReferenceEquals` to check exact object identity.

***

### FAQs

**Q: Why is `NaN == NaN` false?**\
Because `NaN` means “Not a Number,” and by IEEE rules, it is unordered. No comparison with `NaN` is true, even against itself.

**Q: Why does `object o1 = 1; object o2 = 1;` give `false` for `o1 == o2`?**\
Because each `1` was boxed into a separate object. Different references → false.

**Q: How do enums compare?**\
Enums compare by their underlying integer values. Custom struct-like enums need `IEquatable<T>` and operator overloads.

**Q: Why are two records with identical `List<string>` not equal?**\
Because `List<string>` is a reference type, so equality is by reference, not contents.

**Q: Can I check if two references point to the same object?**\
Yes, use `object.ReferenceEquals(x, y)`.

**Q: How do I compare strings ignoring case?**\
Use `string.Equals(a, b, StringComparison.OrdinalIgnoreCase)`.

**Q: Do I need to overload both `==` and `!=`?**\
Yes. If you overload one, you must overload the other to keep behavior consistent.
