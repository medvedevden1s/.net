## Introduction

The lambda expression operator (`=>`) is like a signpost in your code, pointing the way from inputs to outputs. Just as an arrow on a map shows you the direction to travel, the `=>` operator shows the path from parameters to implementation. It's a concise way to express relationships in your code, making it more readable and expressive.

In C#, this versatile operator serves two distinct purposes: defining lambda expressions and creating expression-bodied members. Both uses share the same goal of making your code more concise and focused on what matters.

## Lambda operator

In lambda expressions, the lambda operator `=>` separates the input parameters on the left side from the lambda body on the right side. It's like saying "take these inputs and do this with them."

```csharp
string[] words = { "bot", "apple", "apricot" };
int minimalLength = words
  .Where(w => w.StartsWith("a"))
  .Min(w => w.Length);
Console.WriteLine(minimalLength);   // output: 5

int[] numbers = { 4, 7, 10 };
int product = numbers.Aggregate(1, (interim, next) => interim * next);
Console.WriteLine(product);   // output: 280
```

Input parameters of a lambda expression are strongly typed at compile time. When the compiler can infer the types of input parameters, like in the preceding example, you can omit type declarations. If you need to specify the type of input parameters, you must do that for each parameter, as the following example shows:

```csharp
int[] numbers = { 4, 7, 10 };
int product = numbers.Aggregate(1, (int interim, int next) => interim * next);
Console.WriteLine(product);   // output: 280
```

The following example shows how to define a lambda expression without input parameters:

```csharp
Func<string> greet = () => "Hello, World!";
Console.WriteLine(greet());
```

Lambda expressions are particularly useful when working with LINQ, asynchronous programming, and event handlers, allowing you to write concise, focused code without the overhead of defining separate methods.

{% hint style="info" %}
Lambda expressions create closures, capturing variables from the surrounding scope. This means they can access and modify variables defined outside the lambda.
{% endhint %}

## Expression body definition

An expression body definition has the following general syntax:

```csharp
member => expression;
```

Where `expression` is a valid expression. The return type of `expression` must be implicitly convertible to the member's return type. If the member:

- Has a void return type or
- Is a:
  - Constructor
  - Finalizer
  - Property or indexer set accessor

then `expression` must be a statement expression. Because the expression's result is discarded, the return type of that expression can be any type.

The following example shows an expression body definition for a `Person.ToString` method:

```csharp
public override string ToString() => $"{fname} {lname}".Trim();
```

It's a shorthand version of the following method definition:

```csharp
public override string ToString()
{
   return $"{fname} {lname}".Trim();
}
```

Expression-bodied members were introduced in C# 6.0 for methods and properties, and expanded in C# 7.0 to include constructors, finalizers, and accessors. They provide a more concise syntax for simple member implementations.

### Types of expression-bodied members

You can create expression body definitions for various member types:

#### Methods

```csharp
public double CalculateArea() => Math.PI * radius * radius;
```

#### Properties

```csharp
public string FullName => $"{FirstName} {LastName}";
```

#### Constructors

```csharp
public Person(string name) => Name = name ?? throw new ArgumentNullException(nameof(name));
```

#### Finalizers

```csharp
~ResourceHolder() => ReleaseResource();
```

#### Property and indexer accessors

```csharp
private string _name;
public string Name
{
    get => _name;
    set => _name = value ?? throw new ArgumentNullException(nameof(value));
}

private Dictionary<string, int> _scores = new Dictionary<string, int>();
public int this[string player]
{
    get => _scores.TryGetValue(player, out int score) ? score : 0;
    set => _scores[player] = value;
}
```

{% hint style="warning" %}
Expression-bodied members should only be used for simple, concise implementations. For complex logic, traditional method bodies with multiple statements are more appropriate.
{% endhint %}

## Comparing the two forms

While both forms use the same `=>` operator, they serve different purposes:

| Lambda Expressions | Expression-Bodied Members |
| ------------------ | ------------------------- |
| Create anonymous functions | Provide concise syntax for named members |
| Can be assigned to variables | Are part of a class or struct definition |
| Can capture variables from outer scope | Cannot capture local variables |
| Introduced in C# 3.0 | Introduced in C# 6.0 and expanded in C# 7.0 |
| Used primarily with delegates and LINQ | Used to simplify method, property, and other member definitions |

## Operator overloadability

The `=>` operator can't be overloaded. It's a language construct with fixed semantics that are integral to C#'s syntax.

## Key Points

- The `=>` operator is used in two distinct contexts: lambda expressions and expression-bodied members
- In lambda expressions, it separates input parameters from the lambda body
- In expression-bodied members, it separates the member signature from its implementation
- Lambda expressions create anonymous functions that can be passed as arguments or assigned to variables
- Expression-bodied members provide a concise syntax for simple method, property, and other member implementations
- The `=>` operator cannot be overloaded
- Lambda expressions can capture variables from the surrounding scope
- Expression-bodied members were introduced in C# 6.0 and expanded in C# 7.0

## FAQs

**Q: What's the difference between lambda expressions and expression-bodied members?**  
A: Lambda expressions create anonymous functions that can be assigned to variables or passed as arguments, while expression-bodied members provide a concise syntax for implementing named members like methods and properties.

**Q: Can I use multi-line expressions with the `=>` operator?**  
A: For lambda expressions, yes - you can use a code block with multiple statements. For expression-bodied members, no - they must be a single expression, though that expression can span multiple lines.

**Q: When should I use expression-bodied members instead of traditional syntax?**  
A: Use expression-bodied members when the implementation is simple and concise. If the implementation requires multiple statements or complex logic, traditional syntax is more appropriate.

**Q: Can expression-bodied members access instance variables?**  
A: Yes, expression-bodied members have the same access to instance and static members as traditional member implementations.

**Q: When were these features introduced in C#?**  
A: Lambda expressions were introduced in C# 3.0. Expression-bodied methods and properties were introduced in C# 6.0, and expression-bodied constructors, finalizers, and accessors were added in C# 7.0.

**Q: Can I use the `=>` operator in interfaces?**  
A: Starting with C# 8.0, you can use expression-bodied members in interfaces to provide default implementations for interface members.

**Q: Is there a performance difference between traditional syntax and using the `=>` operator?**  
A: No, there's no performance difference at runtime. The compiler generates the same IL code regardless of which syntax you use. The difference is purely syntactic.