# Boolean logical operators - AND, OR, NOT, XOR

### Introduction

Logical operators are essential for decision-making in programs. They allow you to combine and manipulate `bool` values to form conditions. C# provides **logical Boolean operators** like AND, OR, NOT, and XOR, along with their **conditional** (short-circuiting) counterparts.

Let's explore each operator clearly, with code examples and key details.

***

### Logical Negation Operator `!`

The unary `!` operator flips a boolean value:

```csharp
bool passed = false;
Console.WriteLine(!passed);  // True
Console.WriteLine(!true);    // False
```

{% hint style="info" %}
The postfix `!` is the **null-forgiving operator**, not the logical negation
{% endhint %}

***

### Logical AND Operator `&`

* Returns `true` if **both operands are true**.
* Always evaluates **both operands**, even if the result is already known.

```csharp
bool SecondOperand()
{
    Console.WriteLine("Second operand is evaluated.");
    return true;
}

bool a = false & SecondOperand(); // Output: "Second operand is evaluated." False
bool b = true & SecondOperand();  // Output: "Second operand is evaluated." True
```

{% hint style="warning" %}
For integral numeric types, `&` performs **bitwise AND**.\
The unary `&` is also the **address-of operator** in unsafe code.
{% endhint %}

***

### Conditional Logical AND Operator `&&`

* Returns `true` only if **both operands are true**.
* **Short-circuits**: skips evaluating the right-hand side if the left-hand side is `false`.

```csharp
bool SecondOperand()
{
    Console.WriteLine("Second operand is evaluated.");
    return true;
}

bool a = false && SecondOperand(); // Output: False
bool b = true && SecondOperand();  // Output: "Second operand is evaluated." True
```

***

### Logical Exclusive OR Operator `^`

* Returns `true` if operands differ (`true/false` or `false/true`).
* Equivalent to inequality (`!=`) for bools.

```csharp
Console.WriteLine(true ^ true);    // False
Console.WriteLine(true ^ false);   // True
Console.WriteLine(false ^ true);   // True
Console.WriteLine(false ^ false);  // False
```

{% hint style="info" %}
For integers, `^` performs **bitwise XOR**.
{% endhint %}

***

### Logical OR Operator `|`

* Returns `true` if **either operand is true**.
* Always evaluates **both operands**, even if the result is known.

```csharp
bool SecondOperand()
{
    Console.WriteLine("Second operand is evaluated.");
    return true;
}

bool a = true | SecondOperand();   // Output: "Second operand is evaluated." True
bool b = false | SecondOperand();  // Output: "Second operand is evaluated." True
```

***

### Conditional Logical OR Operator `||`

* Returns `true` if **either operand is true**.
* **Short-circuits**: skips evaluating the right-hand side if the left-hand side is `true`.

```csharp
bool SecondOperand()
{
    Console.WriteLine("Second operand is evaluated.");
    return true;
}

bool a = true || SecondOperand();  // Output: True
bool b = false || SecondOperand(); // Output: "Second operand is evaluated." True
```

***

### Nullable Boolean Operators

For `bool?`, operators support **three-valued logic**:

| x     | y     | x & y | x \| y |
| ----- | ----- | ----- | ------ |
| true  | true  | true  | true   |
| true  | false | false | true   |
| true  | null  | null  | true   |
| false | true  | false | true   |
| false | false | false | false  |
| false | null  | false | null   |
| null  | true  | null  | true   |
| null  | false | false | null   |
| null  | null  | null  | null   |

```csharp
bool? test = null;
Display(!test);         // null
Display(test ^ false);  // null
Display(test ^ null);   // null
Display(true ^ null);   // null

void Display(bool? b) => Console.WriteLine(b is null ? "null" : b.Value.ToString());
```

{% hint style="warning" %}
The conditional operators `&&` and `||` **don’t support** `bool?`
{% endhint %}

***

### Compound Assignment

Operators `&`, `|`, and `^` support compound assignments:

```csharp
bool test = true;

test &= false;
Console.WriteLine(test);  // False

test |= true;
Console.WriteLine(test);  // True

test ^= false;
Console.WriteLine(test);  // True
```

{% hint style="warning" %}
`&&` and `||` don’t support compound assignment.
{% endhint %}

***

### Operator Precedence

From **highest** to **lowest**:

1. `!`
2. `&`
3. `^`
4. `|`
5. `&&`
6. `||`

```csharp
Console.WriteLine(true | true & false);   // True
Console.WriteLine((true | true) & false); // False
```

Parentheses `()` change evaluation order:

```csharp
var result = (true || false) && false; // False
```

***

### Operator Overloading

User-defined types can overload:

* `!`, `&`, `|`, `^` (and their compound forms).
* `&&` and `||` cannot be overloaded directly, but they can work if the type overloads `true`/`false` and `&`/`|`.

***

### Truth Tables (Quick Revision)

#### AND (`&`, `&&`) Returns **true only if both are true**.

| A     | B     | A & B |
| ----- | ----- | ----- |
| true  | true  | true  |
| true  | false | false |
| false | true  | false |
| false | false | false |

***

#### OR (`|`, `||`) Returns **true if at least one is true**.

| A     | B     | A \| B |
| ----- | ----- | ------ |
| true  | true  | true   |
| true  | false | true   |
| false | true  | true   |
| false | false | false  |

***

#### XOR (`^`) Returns **true only if operands are different**.

| A     | B     | A ^ B |
| ----- | ----- | ----- |
| true  | true  | false |
| true  | false | true  |
| false | true  | true  |
| false | false | false |

***

#### NOT (`!`) Flips the value.

| A     | !A    |
| ----- | ----- |
| true  | false |
| false | true  |

***

{% hint style="success" %}
Keep this table in mind:

* **AND** = both must be true.
* **OR** = at least one must be true.
* **XOR** = exactly one must be true.
* **NOT** = just flips the value.
{% endhint %}



### Key Points

* `!` flips a bool value.
* `&`, `|`, and `^` always evaluate both operands.
* `&&` and `||` short-circuit, skipping unnecessary evaluation.
* `^` is true only if operands differ.
* Nullable `bool?` follows three-valued logic.
* Compound assignment is supported only for `&`, `|`, `^`.
* Use parentheses to control precedence.

***

### FAQs

**Q: What’s the difference between `&` and `&&`?**\
`&` always evaluates both operands, while `&&` short-circuits (skips the second if the first is false).

**Q: When should I use `|` vs `||`?**\
Use `|` if you need both operands evaluated, otherwise prefer `||` for efficiency.

**Q: Can I use logical operators with integers?**\
Yes — but then they perform **bitwise** operations.

**Q: Do nullable bools (`bool?`) work with `&&` and `||`?**\
No, only with `&`, `|`, `!`, and `^`.

**Q: Can I overload `&&` and `||`?**\
Not directly. But if your type defines `true`/`false` and overloads `&`/`|`, the compiler can use them in conditional expressions.
