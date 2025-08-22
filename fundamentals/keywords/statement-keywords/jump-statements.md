# Jump statements

### Introduction

Jump statements **transfer control immediately** to another point in code. They are powerful but should be used carefully, since they can make logic harder to follow if overused.

The main jump statements are:

* **break** – exits a loop or switch.
* **continue** – skips to the next loop iteration.
* **return** – exits a method (optionally returning a value).
* **goto** – jumps to a labeled statement.

***

### Break Statement

`break` exits the nearest enclosing **loop** (`for`, `foreach`, `while`, `do`) or **switch**.

```csharp
int[] numbers = [0, 1, 2, 3, 4];
foreach (int n in numbers)
{
    if (n == 3)
        break;  // stop loop when n == 3

    Console.Write($"{n} ");
}
Console.WriteLine("End");
// Output: 0 1 2 End
```

* In **nested loops**, only the innermost loop stops.
* Inside a **switch**, `break` leaves only the switch, not the loop around it.

***

### Continue Statement

`continue` skips the rest of the current iteration and moves to the **next iteration** of the loop.

```csharp
for (int i = 0; i < 5; i++)
{
    if (i < 3)
    {
        Console.WriteLine($"Iteration {i}: skip");
        continue; // go straight to next loop
    }
    Console.WriteLine($"Iteration {i}: done");
}
// Output:
// Iteration 0: skip
// Iteration 1: skip
// Iteration 2: skip
// Iteration 3: done
// Iteration 4: done
```

***

### Return Statement

`return` exits a method immediately.

* With **no value**, it just ends the method.
* With a **value**, it provides a result back to the caller.

```csharp
void PrintOdd(int n)
{
    if (n % 2 == 0)
        return; // exit early

    Console.WriteLine($"{n} is odd");
}

PrintOdd(4); // nothing
PrintOdd(5); // "5 is odd"
```

With a return value:

```csharp
double CylinderSurface(double r, double h)
{
    return 2 * Math.PI * r * (r + h);
}

Console.WriteLine(CylinderSurface(1, 1)); // 12.57
```

#### Ref Returns

You can return a **reference** instead of a value, letting the caller modify the original data.

```csharp
int[] data = { 10, 20, 30 };
ref int found = ref FindFirst(data, x => x == 20);
found = 0; // modifies array element
Console.WriteLine(string.Join(" ", data)); // 10 0 30

ref int FindFirst(int[] arr, Func<int, bool> match)
{
    for (int i = 0; i < arr.Length; i++)
        if (match(arr[i]))
            return ref arr[i];

    throw new InvalidOperationException("Not found");
}
```

{% hint style="warning" %}
Returning a reference ties the caller to the original variable’s lifetime. You cannot safely return references to local variables.
{% endhint %}

***

### Goto Statement

`goto` jumps to a **label** inside the same method.

```csharp
for (int i = 0; i < 5; i++)
{
    if (i == 3)
        goto Found;  // jump to label
    Console.WriteLine(i);
}

Found:
Console.WriteLine("Jumped here!");
// Output:
// 0
// 1
// 2
// Jumped here!
```

* Often used to **exit nested loops**.
* Also works inside a **switch** to jump to another case:

```csharp
enum Coffee { Plain, WithMilk, WithIceCream }

decimal GetPrice(Coffee choice)
{
    decimal price = 0;
    switch (choice)
    {
        case Coffee.Plain:
            price += 10;
            break;
        case Coffee.WithMilk:
            price += 5;
            goto case Coffee.Plain; // reuse logic
        case Coffee.WithIceCream:
            price += 7;
            goto case Coffee.Plain;
    }
    return price;
}
```

{% hint style="warning" %}
`goto` makes code harder to read. Prefer methods or clearer logic when possible.
{% endhint %}

***

### Key Points

* **break** → exits the nearest loop or switch.
* **continue** → skips to next iteration.
* **return** → exits a method, optionally with a value.
* **ref return** → returns a reference instead of a copy.
* **goto** → jumps to a label (use sparingly).

***

### FAQs

**Q: What’s the difference between `break` and `continue`?**\
A: `break` stops the loop entirely, `continue` skips the rest of the current iteration but keeps looping.

**Q: Can `return` be used anywhere?**\
A: No, only inside a method, property, or local function.

**Q: Is `goto` bad practice?**\
A: Often yes. It can reduce readability. Prefer `break`, `return`, or method extraction.

**Q: Can async methods use `ref return`?**\
A: No, because async may return before values are finalized.
