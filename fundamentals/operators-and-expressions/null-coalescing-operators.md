## Introduction

The null-coalescing operators in C# are like having a backup plan. Just as you might have a spare key in case you lose your main one, these operators provide a way to handle null values gracefully. They allow you to specify alternative values or actions when dealing with potentially null variables, making your code more robust and concise.

These operators are particularly valuable in a world where null references are a common source of errors, helping you write safer code with less defensive programming.

## The null-coalescing operator (`??`)

The null-coalescing operator `??` returns the value of its left-hand operand if it isn't null; otherwise, it evaluates the right-hand operand and returns its result. It's like having a fallback option ready to use.

```csharp
// Basic usage with reference types
string name = null;
string displayName = name ?? "Guest";
Console.WriteLine(displayName);  // Output: Guest

// With nullable value types
int? nullable = null;
int nonNullable = nullable ?? 0;
Console.WriteLine(nonNullable);  // Output: 0

// Chaining multiple ?? operators
string firstName = null;
string middleName = null;
string lastName = "Smith";
string fullName = firstName ?? middleName ?? lastName ?? "Unknown";
Console.WriteLine(fullName);  // Output: Smith
```

Key characteristics of the `??` operator:
- It doesn't evaluate its right-hand operand if the left-hand operand is non-null
- It works with both nullable value types and reference types
- It can be chained to provide multiple fallback options
- It's right-associative, meaning expressions are evaluated from right to left

{% hint style="info" %}
The `??` operator is particularly useful when working with APIs that might return null, allowing you to provide sensible defaults without verbose if-else statements.
{% endhint %}

## The null-coalescing assignment operator (`??=`)

The null-coalescing assignment operator `??=` assigns the value of its right-hand operand to its left-hand operand only if the left-hand operand evaluates to null. It's like filling in a blank only if it's actually blank.

```csharp
// Basic usage
List<int>? numbers = null;
// If numbers is null, initialize it. Then, add 5 to numbers
(numbers ??= new List<int>()).Add(5);
Console.WriteLine(string.Join(" ", numbers));  // Output: 5
Console.WriteLine(numbers is null);  // Output: False

// Another example with nullable value types
int? a = null;
Console.WriteLine(a is null);  // Output: True
// If a is null then assign 0 to a and add a to the list
numbers.Add(a ??= 0);
Console.WriteLine(a is null);  // Output: False
Console.WriteLine(string.Join(" ", numbers));  // Output: 5 0
Console.WriteLine(a);  // Output: 0
```

Key characteristics of the `??=` operator:
- Introduced in C# 8.0
- The left-hand operand must be a variable, a property, or an indexer element
- It only evaluates and assigns the right-hand operand if the left-hand operand is null
- It returns the final value of the left-hand operand (whether it was assigned or not)
- It's particularly useful for lazy initialization patterns

{% hint style="warning" %}
The type of the left-hand operand of the `??=` operator can't be a non-nullable value type, as these can never be null.
{% endhint %}

## Working with unconstrained type parameters

The null-coalescing operators can be used with unconstrained type parameters, which is useful in generic methods:

```csharp
private static void Display<T>(T a, T backup)
{
    Console.WriteLine(a ?? backup);
}

// Usage
Display<string>("Hello", "World");  // Output: Hello
Display<string>(null, "Backup");    // Output: Backup
```

This works because the compiler allows the `??` operator with unconstrained type parameters, even though some type arguments might be non-nullable value types.

## Common use cases

### Providing default values

One of the most common uses is to provide default values for potentially null variables:

```csharp
// Without null-coalescing operator
string message;
if (errorMessage != null)
{
    message = errorMessage;
}
else
{
    message = "No error";
}

// With null-coalescing operator
string message = errorMessage ?? "No error";
```

### Lazy initialization

The `??=` operator is perfect for lazy initialization patterns:

```csharp
private List<Customer>? _customers;

public List<Customer> Customers
{
    get => _customers ??= new List<Customer>();
}
```

### Working with null-conditional operators

The null-coalescing operator pairs well with the null-conditional operators (`?.` and `?[]`):

```csharp
double SumNumbers(List<double[]> setsOfNumbers, int indexOfSetToSum)
{
    return setsOfNumbers?[indexOfSetToSum]?.Sum() ?? double.NaN;
}

var sum = SumNumbers(null, 0);
Console.WriteLine(sum);  // Output: NaN
```

### Throwing exceptions for required values

You can use a throw expression as the right-hand operand of the `??` operator:

```csharp
public string Name
{
    get => name;
    set => name = value ?? throw new ArgumentNullException(nameof(value), "Name cannot be null");
}
```

### Simplifying null checks

The `??=` operator can replace common null-checking patterns:

```csharp
// Without null-coalescing assignment
if (variable is null)
{
    variable = expression;
}

// With null-coalescing assignment
variable ??= expression;
```

## Operator overloadability

The operators `??` and `??=` can't be overloaded. They have built-in behavior that works with nullable types and reference types.

## Key Points

- The null-coalescing operator `??` returns the left-hand operand if it's not null, otherwise it returns the right-hand operand
- The null-coalescing assignment operator `??=` assigns the right-hand operand to the left-hand operand only if the left-hand operand is null
- Both operators short-circuit evaluation - they don't evaluate the right-hand operand if the left-hand operand is non-null
- These operators work with both nullable value types and reference types
- They can be chained together and are right-associative
- They can't be used with non-nullable value types
- Neither operator can be overloaded

## FAQs

**Q: What's the difference between the null-coalescing operator (`??`) and the conditional operator (`?:`)?**  
A: The null-coalescing operator (`??`) specifically checks for null, while the conditional operator (`?:`) can evaluate any boolean expression. The `??` operator is more concise when you only need to check for null.

**Q: Can I use the null-coalescing operators with value types like `int` or `double`?**  
A: You can only use them with nullable value types (like `int?` or `double?`) or reference types. Non-nullable value types can never be null, so these operators don't apply to them.

**Q: Are there any performance considerations when using these operators?**  
A: Both operators short-circuit evaluation, meaning they don't evaluate the right-hand operand if the left-hand operand is non-null. This can improve performance, especially if the right-hand operand is expensive to compute.

**Q: Can I use the null-coalescing operators in older versions of C#?**  
A: The `??` operator has been available since C# 2.0, but the `??=` operator was introduced in C# 8.0. If you're using an older version, you'll need to use alternative patterns for the functionality of `??=`.

**Q: How do these operators behave with nested nulls?**  
A: The operators only check the immediate operand for null, not nested properties. For example, `obj ?? defaultObj` only checks if `obj` is null, not any properties within `obj`.

**Q: Can I use these operators in expression trees?**  
A: Yes, both operators are supported in expression trees, allowing you to build dynamic queries that handle null values gracefully.