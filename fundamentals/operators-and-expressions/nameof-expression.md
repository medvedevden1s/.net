## Introduction

The `nameof` expression is like a magical mirror that reflects the names of code elements. Imagine if you could point at any part of your code—a variable, a method, a class—and ask, "What's your name?" That's exactly what the `nameof` expression does. It takes a code element and returns its name as a string, all at compile time.

In the world of programming, names are often needed as string literals—for error messages, logging, property change notifications, and more. Before `nameof`, developers had to manually type these names as strings, which created a disconnect between the code and its references. If you renamed a variable but forgot to update a string that referenced it, you'd end up with a mismatch that the compiler couldn't detect. The `nameof` expression bridges this gap, ensuring that names stay synchronized throughout your codebase.

## How it works

The `nameof` expression evaluates to the name of a variable, type, or member as a string constant. The key thing to understand is that this happens entirely at compile time—the compiler replaces the `nameof` expression with the appropriate string literal in the compiled code.

```csharp
Console.WriteLine(nameof(System.Collections.Generic));  // output: Generic
Console.WriteLine(nameof(List<int>));  // output: List
Console.WriteLine(nameof(List<>));  // output: List (C# 14 and later)
Console.WriteLine(nameof(List<int>.Count));  // output: Count
Console.WriteLine(nameof(List<int>.Add));  // output: Add

List<int> numbers = new List<int>() { 1, 2, 3 };
Console.WriteLine(nameof(numbers));  // output: numbers
Console.WriteLine(nameof(numbers.Count));  // output: Count
Console.WriteLine(nameof(numbers.Add));  // output: Add
```

As you can see from these examples, `nameof` produces different results depending on what you pass to it:

- For a namespace, it returns the last segment of the namespace
- For a type, it returns the name of the type without any generic parameters
- For a member (property, method, etc.), it returns just the name of that member
- For a variable, it returns the name of the variable as it appears in your code

{% hint style="info" %}
The `nameof` expression is evaluated at compile time, so it has no runtime performance impact. The compiled code contains only the resulting string literals.
{% endhint %}

## Common use cases

### Argument validation

One of the most common uses of `nameof` is in argument validation code:

```csharp
public void ProcessOrder(Order order)
{
    if (order == null)
        throw new ArgumentNullException(nameof(order));
    
    // Process the order...
}
```

Using `nameof` here ensures that if you rename the parameter, the error message will automatically stay in sync.

### Property change notification

In classes that implement `INotifyPropertyChanged`, `nameof` helps ensure property names are correct:

```csharp
private string _name;
public string Name
{
    get => _name;
    set
    {
        if (_name != value)
        {
            _name = value;
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(nameof(Name)));
        }
    }
}
```

### Defensive programming

The `nameof` expression can make your code more robust by preventing string literals from getting out of sync:

```csharp
public string Name
{
    get => name;
    set => name = value ?? throw new ArgumentNullException(nameof(value), $"{nameof(Name)} cannot be null");
}
```

## Advanced features

### Using nameof with attributes

Starting with C# 11, you can use `nameof` with method parameters inside attributes on a method or its parameters:

```csharp
[ParameterString(nameof(msg))]
public static void Method(string msg)
{
    [ParameterString(nameof(T))]
    void LocalFunction<T>(T param) { }

    var lambdaExpression = ([ParameterString(nameof(aNumber))] int aNumber) => aNumber.ToString();
}
```

This feature is particularly useful with attributes like `[CallerArgumentExpression]` and the nullable analysis attributes.

### Verbatim identifiers

When using `nameof` with verbatim identifiers (identifiers prefixed with `@` to allow the use of keywords as identifiers), the `@` character is not included in the resulting string:

```csharp
var @new = 5;
Console.WriteLine(nameof(@new));  // output: new
```

### Generic type parameters

You can use `nameof` with generic type parameters:

```csharp
public class GenericClass<T>
{
    public void Method()
    {
        Console.WriteLine(nameof(T));  // output: T
    }
}
```

## Compiler internals

The `nameof` expression is resolved entirely at compile time. When the compiler encounters a `nameof` expression, it:

1. Validates that the argument is a valid code element
2. Determines the appropriate name string
3. Replaces the `nameof` expression with a string literal in the compiled code

This means there's no runtime overhead—the compiled code contains only the resulting string literals.

## Key Points

- The `nameof` expression returns the name of a code element as a string constant
- It's evaluated at compile time, with no runtime impact
- It helps maintain consistency when code is refactored or renamed
- Common uses include argument validation, property change notification, and attribute parameters
- When used with a namespace or type with nested parts, it returns only the last segment
- Starting with C# 11, it can be used with parameters in attributes
- The `@` character in verbatim identifiers is not included in the result
- In C# 14 and later, it can be used with open generic types like `List<>`

## FAQs

**Q: When was the nameof expression introduced in C#?**  
A: The `nameof` expression was introduced in C# 6.0, which was released with Visual Studio 2015.

**Q: Does nameof work with expressions?**  
A: No, `nameof` only works with simple names, qualified names, and member access expressions. You cannot use it with arbitrary expressions like `nameof(x + y)`.

**Q: What happens if I use nameof with a string literal?**  
A: Using `nameof` with a string literal is a compile-time error. For example, `nameof("Hello")` will not compile.

**Q: Can I use nameof with a namespace?**  
A: Yes, but it will only return the last segment of the namespace. For example, `nameof(System.Collections.Generic)` returns "Generic".

**Q: Is there a performance impact when using nameof?**  
A: No, since `nameof` is evaluated at compile time, there is no runtime performance impact. The compiled code contains only the resulting string literals.

**Q: Can I use nameof with dynamic objects?**  
A: No, `nameof` requires compile-time knowledge of the names, so it doesn't work with dynamic objects where the members are resolved at runtime.

**Q: How does nameof handle inheritance?**  
A: When using `nameof` with an inherited member, it returns the name of the member as it appears in the derived class, not the base class name.