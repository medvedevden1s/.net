# Iteration statements - for, foreach, do, and while

### Introduction

Iteration statements let us **repeat code** until a condition is met or until all items in a collection are processed.\
The four main iteration statements are:

* **for** – repeat with a counter.
* **foreach** – repeat over a collection.
* **do** – repeat at least once.
* **while** – repeat while a condition is true.

You can stop a loop early with **break** or skip to the next iteration with **continue**.

***

### The For Statement

A `for` loop runs while a condition is true, usually with a counter variable.

```csharp
for (int i = 0; i < 3; i++)
{
    Console.Write(i);
}
// Output: 012
```

* **Initializer** → runs once before the loop (`int i = 0`).
* **Condition** → checked before each iteration (`i < 3`).
* **Iterator** → runs after each iteration (`i++`).
* **Body** → code that executes each time.

#### What can go into initializer and iterator?

Besides declaring variables, you can use expressions like:

* **Prefix/postfix increment** → `++i`, `i++`
* **Prefix/postfix decrement** → `--i`, `i--`
* **Assignment** → `i = 0`
* **Method calls** → `Console.WriteLine("step")`
* **Await expression** → `await DoSomethingAsync()`
* **Object creation** → `new MyClass()`

```csharp
int i, j = 3;
for (i = 0, Console.WriteLine("Start"); i < j; i++, j--, Console.WriteLine($"Step: i={i}, j={j}"))
{
    // loop body
}
```

{% hint style="info" %}
**Info:** All three parts (`initializer`, `condition`, `iterator`) are optional.&#x20;

For example, `for(;;)` is an infinite loop.
{% endhint %}

***

### The Foreach Statement

A `foreach` loop processes **every element in a collection**.

```csharp
List<int> numbers = new() { 1, 2, 3, 4 };
foreach (int n in numbers)
{
    Console.Write($"{n} ");
}
// Output: 1 2 3 4
```

* Works with arrays, lists, dictionaries, and any type that supports enumeration.
* Cleaner than `for` when you don’t need indexes.

You can also modify elements by reference:

```csharp
Span<int> data = stackalloc int[5];
int counter = 1;
foreach (ref int item in data)
{
    item = counter++;
}

foreach (var item in data)
    Console.Write($"{item} ");

// Output: 1 2 3 4 5
```

***

#### Await Foreach (Asynchronous Loop)

`await foreach` is used with asynchronous streams:

```csharp
await foreach (var item in GenerateSequenceAsync())
{
    Console.WriteLine(item);
}
```

***

### The Do Statement

A `do` loop always runs **at least once**, because the condition is checked **after** the loop body.

```csharp
int n = 0;
do
{
    Console.Write(n);
    n++;
} while (n < 3);

// Output: 012
```

{% hint style="success" %}
Use `do` when you need the code to run at least once before checking.
{% endhint %}

***

### The While Statement

A `while` loop checks its condition **before** running.

```csharp
int n = 0;
while (n < 3)
{
    Console.Write(n);
    n++;
}
// Output: 012
```

{% hint style="warning" %}
A `while` loop may run **zero times** if the condition is false initially.
{% endhint %}

***

### Key Points

* `for` → best when you know how many times to repeat.
* `foreach` → best for looping through collections.
* `do` → always runs at least once.
* `while` → may run zero or more times.
* `for` loop parts (`initializer`, `iterator`) can include increments, assignments, method calls, `await`, or even object creation.

***

### FAQs

**Q: What’s the difference between `do` and `while`?**\
A: `do` runs at least once; `while` may run zero times.

**Q: When should I use `for` vs. `foreach`?**\
A: Use `for` when you need an index; `foreach` when you just need items.

**Q: Can I break out of any loop?**\
A: Yes, use `break`. Use `continue` to skip the rest of the current iteration.

**Q: Can loops be infinite?**\
A: Yes, e.g. `for(;;)` or `while(true)`. Always ensure a way to exit.
