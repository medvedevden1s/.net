# New constraint

### Introduction

The `new` constraint in generics is a simple rule you can add when working with types. It tells your code clearly: _"Hey! The type you're using **must** have an easy way to create new objects without any special instructions."_ This is important because, without this rule, the computer wouldn't know how to create new objects of your generic type.

### Why Use the `new()` Constraint?

Imagine you have a toy factory. You want your factory to create toys, but you need one simple rule: every toy must be easy to create—no special instructions needed. In code, this means every type must have a **public constructor with no parameters** (a simple method used to create a new object).

Here's how you write this clearly in code:

```csharp
// Generic ToyFactory that can create new toys easily
class ToyFactory<T> where T : new()
{
    public T MakeToy()
    {
        // Easy creation because we ensured T has a parameterless constructor
        return new T();
    }
}
```

Let's explain even simpler:

* You tell the factory: _"Make me a toy car!"_ ✅ The factory easily makes a new car because cars are simple to build without special instructions.
* You tell the factory: _"Make me a spaceship—but wait, it needs specific fuel and special parts."_ ❌ The factory can't make it because your rule said it **must** be easy to build.

{% hint style="info" %}
The `new()` constraint simply tells your code: _"I can only handle types that can easily create new objects without extra instructions!"_
{% endhint %}

### Combining `new()` with Other Constraints

Sometimes you have other rules, like a toy needing to have certain features:

* Must be comparable (able to compare with other toys).
* Must be easily created (the `new()` constraint).

Then your code looks like this:

```csharp
// T must be comparable AND easy to create
class ToyComparer<T> where T : IComparable, new()
{
    public T CreateAndCompare(T otherToy)
    {
        T newToy = new T(); // easily create a new toy
        return newToy.CompareTo(otherToy) > 0 ? newToy : otherToy;
    }
}
```

* `IComparable` means your toys can be compared (bigger, smaller, etc.).
* `new()` means they must also be easy to create.

{% hint style="warning" %}
&#x20;Always put the `new()` constraint **last** when you have multiple constraints.
{% endhint %}

### Key Points:

* The `new()` constraint ensures you can create objects of the generic type using a simple constructor (no arguments).
* It’s like a promise to your code: _"this type can always be created easily."_
* You can combine it with other constraints (but always list it last).
* Abstract classes and interfaces **can't** use the `new()` constraint because you can't directly create instances of them.

### FAQs

**Why can't interfaces use the `new()` constraint?**\
Because interfaces just describe behaviors—they don't have constructors, so you can't directly create instances from them.

**What happens if I forget the `new()` constraint?**\
Your code won't compile if you try to use `new T()` because the computer isn't sure if `T` can actually be created.

**Can I create a class without using `new()` constraint?**\
Yes! But if you want to use `new T()` inside your generic class or method, then you **must** use the `new()` constraint.

**Is using the `new()` constraint slow or affects performance?**\
No. It doesn't slow down your program. It's just a rule checked at compile-time (when your code is turned into a program), so it doesn't affect speed.
