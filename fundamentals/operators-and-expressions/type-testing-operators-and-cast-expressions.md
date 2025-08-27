# Type-testing operators and cast expressions

### Introduction

Type-testing operators and cast expressions are like identity checks in the C# world. Think of them as security guards at a building entrance - they verify if an object is what it claims to be, or transform it into something else when needed.

These operators and expressions are essential when working with inheritance hierarchies, interfaces, and when you need to safely convert between different types in your code.

### The `is` operator

The is operator is like a security guard who checks ID cards without taking any action. It simply verifies if an object is compatible with a specific type and returns a boolean result.

```csharp
// Basic type checking
object obj = "Hello, World!";
bool isString = obj is string;        // true - obj is a string
bool isInt = obj is int;              // false - obj is not an int

// Checking inheritance relationships
public class Base { }
public class Derived : Base { }

object b = new Base();
Console.WriteLine(b is Base);         // true - direct type match
Console.WriteLine(b is Derived);      // false - b is not a Derived instance

object d = new Derived();
Console.WriteLine(d is Base);         // true - inheritance relationship
Console.WriteLine(d is Derived);      // true - direct type match
```

The is operator returns true when:

* The object is exactly of the specified type
* The object is derived from the specified type
* The object implements the specified interface
* A boxing conversion exists from the object's type to the specified type

{% hint style="info" %}
The is operator never throws exceptions, making it safe for type checking before casting.
{% endhint %}

### Type testing with pattern matching

C# 7.0 enhanced the is operator with pattern matching capabilities, allowing you to check a type and declare a variable in one step.

```csharp
// Without pattern matching
object obj = "Hello, World!";
if (obj is string)
{
    string str = (string)obj;
    Console.WriteLine(str.Length);  // 13
}

// With pattern matching
if (obj is string message)
{
    Console.WriteLine(message.Length);  // 13
}

// Works with numeric types too
object num = 42;
if (num is int n)
{
    Console.WriteLine(n * 2);  // 84
}

// Can be combined with other conditions
int? nullable = 7;
if (num is int x && nullable is int y)
{
    Console.WriteLine(x + y);  // 49
}
```

Pattern matching with the is operator:

* Combines type checking and variable declaration
* Only assigns the variable if the type check succeeds
* Makes the variable available in the scope where the condition is true
* Reduces code verbosity and potential casting errors

### The as operator

The as operator is like a security guard who not only checks ID but also gives a visitor pass if the ID is valid. It attempts to convert an object to a specified type, returning null if the conversion isn't possible.

```csharp
// Attempting safe conversions
object obj1 = "Hello, World!";
string str = obj1 as string;    // str = "Hello, World!"
int? num = obj1 as int?;        // num = null (conversion not possible)

// With collections
IEnumerable<int> numbers = new List<int>() { 10, 20, 30 };
IList<int> list = numbers as IList<int>;
if (list != null)
{
    Console.WriteLine(list[0] + list[list.Count - 1]);  // 40
}

// With class hierarchies
Base baseObj = new Derived();
Derived derivedObj = baseObj as Derived;  // Works because baseObj is actually a Derived instance
```

Key facts about the as operator:

* Only works with reference types and nullable value types
* Never throws exceptions (unlike cast expressions)
* Returns null when conversion isn't possible
* More efficient than using `is` followed by a cast
* Doesn't work with non-nullable value types

{% hint style="warning" %}
Always check for null after using the as operator unless you're certain the conversion will succeed.
{% endhint %}

### Cast expressions

Cast expressions are like forceful security guards who either let you through or throw you out. They attempt to convert an object to a specified type and throw an exception if the conversion fails.

```csharp
// Basic casting
object obj = "Hello, World!";
string str = (string)obj;       // Works fine
// int num = (int)obj;          // Would throw InvalidCastException

// Numeric conversions
double x = 1234.7;
int a = (int)x;                 // a = 1234 (truncated)
Console.WriteLine(a);

// Collection conversions
int[] ints = [10, 20, 30];
IEnumerable<int> numbers = ints;
IList<int> list = (IList<int>)numbers;
Console.WriteLine(list.Count);  // 3
Console.WriteLine(list[1]);     // 20
```

When to use cast expressions:

* When you're certain the cast will succeed
* When you want exceptions for invalid casts (fail-fast approach)
* For value type conversions (where as operator can't be used)
* When performing numeric conversions with potential data loss

{% hint style="danger" %}
Cast expressions can throw InvalidCastException at runtime. Use the is or as operators for safer type conversions when the type isn't guaranteed.
{% endhint %}

### The `typeof` operator

The typeof operator is like looking up a building's blueprint. It obtains the System.Type instance for a type, which contains metadata about that type.

```csharp
// Getting Type information
Type stringType = typeof(string);
Console.WriteLine(stringType.FullName);  // System.String
Console.WriteLine(stringType.IsClass);   // True

// Comparing types
object obj = "Hello";
Console.WriteLine(obj.GetType() == typeof(string));  // True

// With generic types
Console.WriteLine(typeof(List<string>));  // System.Collections.Generic.List`1[System.String]

// With generic methods
void PrintType<T>() => Console.WriteLine(typeof(T));

PrintType<int>();                      // System.Int32
PrintType<Dictionary<int, char>>();    // System.Collections.Generic.Dictionary`2[System.Int32,System.Char]
```

The typeof operator:

* Works with any type, including generics
* Returns a Type object at compile time
* Cannot be used with type variables (use GetType() instead)
* Is often used for reflection and dynamic programming
* Cannot be used with nullable reference types (use the underlying type)

{% hint style="info" %}
Unlike GetType(), which works on objects at runtime, typeof() works on type names at compile time.
{% endhint %}

### Type testing with the typeof operator

The typeof operator can be combined with GetType() to perform precise type checking:

```csharp
public class Animal { }
public class Giraffe : Animal { }

public static void Main()
{
    object b = new Giraffe();
    
    // is operator checks compatibility
    Console.WriteLine(b is Animal);                 // True
    Console.WriteLine(b is Giraffe);                // True
    
    // typeof checks exact type
    Console.WriteLine(b.GetType() == typeof(Animal));    // False
    Console.WriteLine(b.GetType() == typeof(Giraffe));   // True
}
```

Key differences:

* `is` checks if an object is compatible with a type (inheritance, interfaces)
* `GetType() == typeof()` checks if an object is exactly of a specific type
* Use `is` for polymorphic behavior
* Use `GetType() == typeof()` when exact type matters

### Key Points

* The `is` operator checks if an object is compatible with a specified type
* Pattern matching with `is` combines type checking and variable declaration
* The `as` operator performs safe conversions, returning null if conversion fails
* Cast expressions `(Type)obj` perform explicit conversions but may throw exceptions
* The `typeof` operator obtains the System.Type instance for a type
* Type-testing operators cannot be overloaded
* Cast expressions can be customized through user-defined conversion operators

### FAQs

**Q: When should I use the is operator versus the as operator?**\
A: Use `is` when you only need to check the type without converting. Use `as` when you need the converted object and prefer null over exceptions for failed conversions.

**Q: Can I use the as operator with value types like int or double?**\
A: No, the `as` operator only works with reference types and nullable value types. For non-nullable value types, you must use cast expressions.

**Q: What's the difference between is and GetType() == typeof()?**\
A: The `is` operator checks for type compatibility (including inheritance), while `GetType() == typeof()` checks for exact type equality.

**Q: Can type-testing operators be overloaded?**\
A: No, the `is`, `as`, and `typeof` operators cannot be overloaded. However, you can define custom conversion operators that will be used by cast expressions.

**Q: When should I use pattern matching with the is operator?**\
A: Use pattern matching when you need to both check the type and use the converted object, as it reduces code by combining these operations.

**Q: Does the typeof operator work at runtime or compile time?**\
A: The `typeof` operator is evaluated at compile time, while `GetType()` is evaluated at runtime.

**Q: What happens if a cast expression fails?**\
A: It throws an `InvalidCastException`, which can crash your application if not caught. Use `as` or `is` for safer conversions.
