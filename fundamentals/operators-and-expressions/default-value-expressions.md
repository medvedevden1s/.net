## Introduction

Default value expressions are like the factory reset button on your devices. Just as pressing that button returns your device to its original, out-of-the-box state, default value expressions reset a type to its most basic, initial value. They provide a clean slate, a starting point that's consistent and predictable for any type in the language.

In programming, we often need these "starting values" - whether it's initializing collections, providing fallback values, or creating new instances. Default value expressions give us a standardized way to obtain these values without having to remember the specific default for each type.

## The `default` operator

The default operator is the original way to obtain a default value for any type. It requires you to explicitly specify the type whose default value you want:

```csharp
int defaultInt = default(int);  // 0
bool defaultBool = default(bool);  // false
string defaultString = default(string);  // null
DateTime defaultDate = default(DateTime);  // 01/01/0001 00:00:00
```

The default operator works with any type, including:
- Value types (numbers, booleans, structs) - initialized to zero/false
- Reference types (strings, classes) - initialized to null
- Nullable value types - initialized to null
- Generic type parameters - initialized appropriately based on the type argument

Here's an example showing how the default operator works with different types:

```csharp
Console.WriteLine(default(int));  // output: 0
Console.WriteLine(default(object) is null);  // output: True

void DisplayDefaultOf<T>()
{
    var val = default(T);
    Console.WriteLine($"Default value of {typeof(T)} is {(val == null ? "null" : val.ToString())}.");
}

DisplayDefaultOf<int?>();
DisplayDefaultOf<System.Numerics.Complex>();
DisplayDefaultOf<System.Collections.Generic.List<int>>();
// Output:
// Default value of System.Nullable`1[System.Int32] is null.
// Default value of System.Numerics.Complex is (0, 0).
// Default value of System.Collections.Generic.List`1[System.Int32] is null.
```

The default operator is particularly useful in generic code, where you don't know the specific type at design time:

```csharp
public static T GetDefaultValue<T>()
{
    return default(T);
}

int zero = GetDefaultValue<int>();  // 0
string nullString = GetDefaultValue<string>();  // null
```

{% hint style="info" %}
The default value of a type is the same value that would be assigned to a field of that type in a class if no initializer is provided.
{% endhint %}

## The `default` literal

Introduced in C# 7.1, the default literal is a more concise way to express the same concept as the default operator. The key difference is that the compiler infers the type from the context, eliminating the need to specify it explicitly:

```csharp
// Using default literal
int defaultInt = default;  // 0
bool defaultBool = default;  // false
string defaultString = default;  // null
```

The default literal can be used in any context where the type can be inferred:

```csharp
// In variable initialization
int count = default;

// In method parameters with default values
void Process(string text, int count = default)

// In return statements
return default;

// In expressions
int? nullable = condition ? value : default;

// In array initialization
int[] numbers = new int[3] { default, default, default };
```

Here's a more complete example from the issue description:

```csharp
T[] InitializeArray<T>(int length, T initialValue = default)
{
    if (length < 0)
    {
        throw new ArgumentOutOfRangeException(nameof(length), "Array length must be nonnegative.");
    }

    var array = new T[length];
    for (var i = 0; i < length; i++)
    {
        array[i] = initialValue;
    }
    return array;
}

void Display<T>(T[] values) => Console.WriteLine($"[ {string.Join(", ", values)} ]");

Display(InitializeArray<int>(3));  // output: [ 0, 0, 0 ]
Display(InitializeArray<bool>(4, default));  // output: [ False, False, False, False ]

System.Numerics.Complex fillValue = default;
Display(InitializeArray(3, fillValue));  // output: [ (0, 0), (0, 0), (0, 0) ]
```

{% hint style="success" %}
The default literal makes your code more concise and less redundant, especially when the type is already obvious from the context.
{% endhint %}

## When to use each form

While both forms produce the same result at runtime, there are scenarios where one is more appropriate than the other:

### Use the default operator when:

1. The type cannot be inferred from the context
2. You're working with a type parameter in a generic method without constraints
3. You need to be explicit about the type for code readability

```csharp
// Type can't be inferred here
var defaultInt = default(int);

// In a generic method where T could be anything
public T GetDefault<T>() => default(T);
```

### Use the default literal when:

1. The type can be inferred from the context
2. You're initializing a variable
3. You're providing a default parameter value
4. You're returning a default value from a method with a known return type

```csharp
// Type is inferred from the variable declaration
int number = default;

// Default parameter
void Process(string input, CancellationToken cancellationToken = default)

// Return type is known
public Customer GetCustomer(int id)
{
    if (id <= 0)
        return default;
    // ...
}
```

## Compiler internals

Both forms of default value expressions are resolved at compile time when possible. For primitive types and other known types, the compiler can directly emit the appropriate default value (0, false, null, etc.).

For generic type parameters, the runtime behavior depends on whether the type parameter is constrained:

```csharp
// T could be any type, so the runtime needs to check
public T GetDefault<T>() => default;

// T must be a class, so the default is always null
public T GetDefault<T>() where T : class => default;

// T must be a struct, so a zero-initialized value is created
public T GetDefault<T>() where T : struct => default;
```

The IL code generated for default expressions is straightforward:
- For value types: appropriate zero-initialization instructions
- For reference types: `ldnull` instruction
- For generic type parameters: `initobj` instruction with the runtime type

## Common use cases

### 1. Providing default values for optional parameters

```csharp
public void ProcessData(string data, ILogger logger = default, CancellationToken token = default)
{
    logger ??= NullLogger.Instance;
    // ...
}
```

### 2. Initializing collections

```csharp
public Dictionary<string, int> CreateDictionary()
{
    return new Dictionary<string, int>
    {
        ["one"] = 1,
        ["two"] = 2,
        ["unknown"] = default
    };
}
```

### 3. Implementing the Null Object pattern

```csharp
public class NullCustomer : ICustomer
{
    public int Id => default;
    public string Name => default;
    public decimal Balance => default;
    
    public void UpdateBalance(decimal amount) { /* Do nothing */ }
}
```

### 4. Resetting values

```csharp
public void Reset()
{
    _count = default;
    _name = default;
    _isActive = default;
    _lastUpdated = default;
}
```

### 5. Conditional logic with default values

```csharp
public int GetValue(bool hasValue, int value)
{
    return hasValue ? value : default;
}
```

## Key Points

- Default value expressions produce the default value of a type
- The default operator (`default(T)`) requires explicitly specifying the type
- The default literal (`default`) infers the type from the context
- Default values are: 0 for numeric types, false for bool, null for reference types
- The default literal was introduced in C# 7.1
- Both forms generate the same IL code at runtime
- Default expressions are particularly useful in generic code
- Use the default operator when the type cannot be inferred
- Use the default literal for more concise code when the type is obvious

## FAQs

**Q: What is the default value for common types?**  
A: For numeric types (int, double, etc.) it's 0, for bool it's false, for char it's '\0', for reference types (string, classes) it's null, for structs it's a struct with all fields initialized to their default values.

**Q: Can I use default with custom structs and classes?**  
A: Yes, for custom structs, all fields will be initialized to their default values. For custom classes, the default is null.

**Q: Is there a performance difference between default(T) and default?**  
A: No, they compile to the same IL code. The difference is purely syntactic.

**Q: When was the default literal introduced?**  
A: The default literal was introduced in C# 7.1, while the default operator has been available since the beginning of C#.

**Q: Can I use default in attribute arguments?**  
A: Yes, but you must use the default operator form with an explicit type, as attribute arguments must be constants that can be evaluated at compile time.

**Q: How does default work with generics and constraints?**  
A: With generic type parameters, default produces a value appropriate for whatever type is provided. Constraints can help the compiler determine what the default value will be (e.g., null for reference types, zero-initialized for value types).

**Q: Is there a way to customize the default value for my custom types?**  
A: No, the default value is fixed by the language. However, you can create static factory methods like `MyType.Empty()` or `MyType.Default()` that return custom "default" instances.