# Collection expressions

### Introduction

Collection expressions provide a **short and clean syntax** for creating collections.\
They use square brackets `[` `]` and can initialize arrays, spans, lists, and more.

Think of them as a universal shorthand: write once, and the compiler figures out the best collection type based on context.

***

### Basic Usage

You can create a collection directly with square brackets:

```csharp
Span<string> weekDays = ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"];

foreach (var day in weekDays)
{
    Console.WriteLine(day);
}
```

Here we declared a `Span<string>` and filled it with days of the week.

***

### Where You Can Use Them

Collection expressions aren’t just for local variables. You can use them in many places:

```csharp
// Private field
private static readonly ImmutableArray<string> _months =
    ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"];

// Property
public IEnumerable<int> MaxDays => [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31];

// Method parameter
public int Sum(IEnumerable<int> values) => values.Sum();

public void Example()
{
    int sum = Sum([1, 2, 3, 4, 5]); // Passing directly
}
```

{% hint style="warning" %}
Collection expressions **cannot** be used where a compile-time constant is required, such as `const` values or default parameter values.
{% endhint %}

***

### Using Variables Inside

You’re not limited to constants—you can include variables too:

```csharp
string hydrogen = "H";
string helium = "He";
string lithium = "Li";

string[] elements = [hydrogen, helium, lithium];
foreach (var element in elements)
{
    Console.WriteLine(element);
}
```

***

### Spread Operator `..`

You can **inline other collections** using the spread operator `..`.

```csharp
string[] vowels = ["a", "e", "i", "o", "u"];
string[] consonants = ["b", "c", "d", "f", "g", "h", "j", "k",
                       "l", "m", "n", "p", "q", "r", "s", "t",
                       "v", "w", "x", "z"];

string[] alphabet = [..vowels, ..consonants, "y"];
```

📌 `..vowels` expands into `"a", "e", "i", "o", "u"`.\
📌 `..consonants` expands into the rest of the letters.\
📌 You can mix spread elements with individual ones.

***

### Conversions

Collection expressions can be converted into many collection types:

* `Span<T>` / `ReadOnlySpan<T>`
* Arrays: `int[]`, `string[]`
* Types with a `Create(ReadOnlySpan<T>)` method
* Types that support **collection initializers** (`List<T>`, etc.)
* Interfaces: `IEnumerable<T>`, `IReadOnlyCollection<T>`, `IReadOnlyList<T>`, `ICollection<T>`, `IList<T>`

{% hint style="info" %}
Even when converted to `IEnumerable<T>`, the compiler still creates a **real in-memory collection**.\
This is different from LINQ, where sequences can be deferred.
{% endhint %}

***

### Performance Optimizations

The compiler chooses the most efficient implementation:

* `[]` → `Array.Empty<T>()`
* For spans → might be **stack allocated** for speed
* For immutable collections → efficient internal allocation

***

### Collection Builder

You can make **your own types** work with collection expressions.

Example: a `LineBuffer` that stores characters.

```csharp
public class LineBuffer : IEnumerable<char>
{
    private readonly char[] _buffer;
    private readonly int _count;

    public LineBuffer(ReadOnlySpan<char> buffer)
    {
        _buffer = new char[buffer.Length];
        _count = buffer.Length;
        for (int i = 0; i < _count; i++)
            _buffer[i] = buffer[i];
    }

    public int Count => _count;

    public char this[int index] => _buffer[index];

    public IEnumerator<char> GetEnumerator()
    {
        for (int i = 0; i < _count; i++)
            yield return _buffer[i];
    }

    IEnumerator IEnumerable.GetEnumerator() => GetEnumerator();
}
```

To enable collection expressions:

1. Add a **builder class** with a `Create(ReadOnlySpan<T>)` method:

```csharp
internal static class LineBufferBuilder
{
    internal static LineBuffer Create(ReadOnlySpan<char> values) => new LineBuffer(values);
}
```

2. Annotate `LineBuffer` with `CollectionBuilderAttribute`:

```csharp
[CollectionBuilder(typeof(LineBufferBuilder), "Create")]
public class LineBuffer : IEnumerable<char>
{
    // ...
}
```

Now you can use it like this:

```csharp
LineBuffer line = ['H', 'e', 'l', 'l', 'o'];
```

***

### Key Points

* Collection expressions are a **universal shorthand** for creating collections.
* Use `..` (spread operator) to expand other collections inline.
* They can convert into spans, arrays, lists, immutable collections, and more.
* The compiler optimizes their memory usage automatically.
* You can make your own collection types compatible by adding a **builder**.

***

### FAQs

❓ **Can I use collection expressions in constants?**\
No. They can’t be used for `const` fields or default parameter values.

❓ **How are they different from LINQ?**\
LINQ can defer execution. Collection expressions always create a concrete collection immediately.

❓ **Can I mix spread elements and values?**\
Yes, e.g. `[..list1, "extra", ..list2]`.

❓ **Do collection expressions work with custom collections?**\
Yes, but your type must implement `IEnumerable<T>` and provide a builder via `CollectionBuilderAttribute`.
