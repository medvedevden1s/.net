# Conditional operator - the ternary conditional operator

### Introduction

The conditional operator `?:` in C# is like a mini decision-maker in your code. Just as you might quickly choose between two options based on a condition in everyday life, this operator lets your program make a choice between two values based on whether a condition is true or false.

Think of it as a compact if-else statement that fits on a single line - perfect for those situations where you need to make a simple choice without the verbosity of multiple lines of code.

### Conditional operator `?:`

The conditional operator, also known as the ternary conditional operator, is a concise way to evaluate a Boolean expression and return one of two values depending on whether the expression is true or false.

```csharp
// Basic syntax of the conditional operator
// condition ? valueIfTrue : valueIfFalse

// Example: Determine weather display based on temperature
string GetWeatherDisplay(double tempInCelsius) => tempInCelsius < 20.0 ? "Cold." : "Perfect!";

// Using the method
Console.WriteLine(GetWeatherDisplay(15));  // Output: Cold.
Console.WriteLine(GetWeatherDisplay(27));  // Output: Perfect!

// Inline usage without a method
int age = 17;
string status = age >= 18 ? "Adult" : "Minor";  // Result: "Minor"
```

The conditional operator evaluates the condition before the `?` symbol. If the condition is true, it returns the value between the `?` and `:` symbols. If the condition is false, it returns the value after the `:` symbol.

{% hint style="info" %}
A helpful mnemonic for remembering how the conditional operator works: "is this condition true ? yes : no"
{% endhint %}

### Target-typed conditional expressions

Conditional expressions are target-typed, meaning if the target type is known, the types of the true and false expressions must be implicitly convertible to that target type.

```csharp
// Target typing with nullable int
int? x = new Random().NextDouble() > 0.5 ? 12 : null;  // x is int?

// Target typing with interface
IEnumerable<int> numbers = x is null ? new List<int>() { 0, 1 } : new int[] { 2, 3 };
```

When the target type is unknown (for example, when using the `var` keyword), the types of both the true and false expressions must be the same, or there must be an implicit conversion from one type to the other:

```csharp
// When using var, both expressions must be compatible
var condition = new Random().NextDouble() > 0.5;

// This works because there's an implicit conversion to int?
var result = condition ? 12 : (int?)null;

// This would not compile without the cast:
// var error = condition ? 12 : null;  // Error: Type of conditional expression cannot be determined
```

### Nesting conditional operators

The conditional operator is right-associative, which means that when multiple conditional operators are chained together, they are evaluated from right to left.

```csharp
// Nested conditional operators
int score = 85;
string grade = score >= 90 ? "A" : score >= 80 ? "B" : score >= 70 ? "C" : "F";
Console.WriteLine(grade);  // Output: B

// The above is equivalent to:
string gradeExpanded = score >= 90 
    ? "A" 
    : (score >= 80 
        ? "B" 
        : (score >= 70 
            ? "C" 
            : "F"));
```

While nesting conditional operators can create concise code, it can also reduce readability if overused. Consider using if-else statements for complex conditions.

{% hint style="warning" %}
Deeply nested conditional operators can make code difficult to read and maintain. Use them judiciously and consider readability.
{% endhint %}

### Conditional ref expressions

A conditional ref expression conditionally returns a reference to a variable, allowing you to choose between two variables and then operate on the chosen reference.

```csharp
// Arrays to demonstrate conditional ref
int[] smallArray = {1, 2, 3, 4, 5};
int[] largeArray = {10, 20, 30, 40, 50};

// Choose which array element to reference based on index
int index = 7;
ref int refValue = ref ((index < 5) ? ref smallArray[index] : ref largeArray[index - 5]);
refValue = 0;  // This modifies the selected array element

// Another example with a different index
index = 2;
((index < 5) ? ref smallArray[index] : ref largeArray[index - 5]) = 100;  // Direct assignment

// Display the results
Console.WriteLine(string.Join(" ", smallArray));  // Output: 1 2 100 4 5
Console.WriteLine(string.Join(" ", largeArray));  // Output: 10 20 0 40 50
```

The syntax for a conditional ref expression requires the `ref` keyword before each possible reference:

```csharp
condition ? ref consequentVariable : ref alternativeVariable
```

Like the standard conditional operator, a conditional ref expression evaluates only one of the two expressions: either the consequent or the alternative.

{% hint style="info" %}
In a conditional ref expression, the type of both the consequent and alternative must be the same. Unlike regular conditional expressions, conditional ref expressions aren't target-typed.
{% endhint %}

### Conditional operator vs. if-else statement

The conditional operator can often replace simple if-else statements, resulting in more concise code:

```csharp
// Using if-else statement
int input = new Random().Next(-5, 5);
string classify;

if (input >= 0)
{
    classify = "nonnegative";
}
else
{
    classify = "negative";
}

// Using conditional operator - more concise
classify = (input >= 0) ? "nonnegative" : "negative";
```

The conditional operator is particularly useful for simple assignments based on conditions. However, for complex logic or multiple statements, an if-else structure is often more appropriate and readable.

{% hint style="success" %}
Use the conditional operator for simple, single-value selections. Use if-else statements for more complex logic or when multiple statements need to be executed conditionally.
{% endhint %}

### Operator overloadability

A user-defined type cannot overload the conditional operator. This is one of the few operators in C# that cannot be overloaded.

### Key Points

* The conditional operator (`?:`) evaluates a Boolean expression and returns one of two values based on the result
* The syntax is `condition ? valueIfTrue : valueIfFalse`
* Only one of the two possible values is evaluated, based on the condition
* Conditional expressions are target-typed, meaning they can adapt to the expected return type
* The conditional operator is right-associative, so nested operators are evaluated from right to left
* Conditional ref expressions allow you to conditionally reference different variables
* The conditional operator can make code more concise compared to if-else statements for simple conditions
* The conditional operator cannot be overloaded by user-defined types

### FAQs

**Q: Can the conditional operator handle more than two choices?**\
A: No, the conditional operator only handles two choices (true or false). For multiple choices, you can nest conditional operators or use a switch statement/expression.

**Q: Does the conditional operator evaluate both possible return values?**\
A: No, it only evaluates the value that corresponds to the condition result. This is called "short-circuit evaluation" and can be important for performance and avoiding potential exceptions.

**Q: Can I use the conditional operator for complex operations with multiple statements?**\
A: No, each part of the conditional operator must be an expression, not a statement block. For complex operations, use if-else statements instead.

**Q: What's the difference between a conditional operator and a conditional ref expression?**\
A: A regular conditional operator returns a value, while a conditional ref expression returns a reference to a variable, allowing you to modify the variable through that reference.

**Q: Can I use the conditional operator in LINQ queries?**\
A: Yes, the conditional operator works well in LINQ queries and is often used for filtering and transforming data based on conditions.

**Q: Why can't I overload the conditional operator in my custom types?**\
A: The conditional operator is one of the few operators that C# doesn't allow to be overloaded, along with operators like `&&`, `||`, `??`, and others that have special evaluation behavior.
