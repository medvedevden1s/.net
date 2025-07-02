# The new (member modifier)

### Introduction

The `new` keyword, when used as a modifier in C#, explicitly hides inherited members from a base class. It clearly indicates your intention to replace an inherited member with a new implementation, making your code easier to understand and maintain.

### Understanding Member Hiding

When a derived class member shares the same name as a member in its base class, the derived member hides the base class member. The `new` modifier clearly indicates that this hiding is intentional.

Without the `new` modifier, the code compiles but generates a compiler warning, making the intention unclear. Consider a simple example with animals that make different sounds:

```csharp
public class Animal
{
    public void MakeSound()
    {
        Console.WriteLine("Animal makes a sound.");
    }
}

public class Dog : Animal
{
    new public void MakeSound()
    {
        Console.WriteLine("Dog barks.");
    }
}

class Program
{
    static void Main()
    {
        Animal generalAnimal = new Animal();
        generalAnimal.MakeSound(); // Output: Animal makes a sound.

        Dog dog = new Dog();
        dog.MakeSound(); // Output: Dog barks.

        Animal animalDog = new Dog();
        animalDog.MakeSound(); // Output: Animal makes a sound.
    }
}
```

Here, the `Dog` class hides the `MakeSound` method from the `Animal` class, clearly defined by using the `new` modifier.

Imagine a scenario involving different levels of user permissions:

```csharp
public class User
{
    public static string permissions = "Read";
}

public class Admin : User
{
    new public static string permissions = "Read, Write, Delete";

    static void Main()
    {
        Console.WriteLine(Admin.permissions);      // Output: Read, Write, Delete
        Console.WriteLine(User.permissions);       // Output: Read
    }
}
```

The `permissions` field in the `Admin` class intentionally hides the inherited field from the `User` class.

### Notes

* The `new` modifier prevents compiler warnings about hidden inherited members.
* It explicitly clarifies your intent to hide inherited members.
* You cannot use `new` and `override` together, as they serve opposite purposes.



### FAQs

**What if I don't use the `new` keyword when hiding members?**

You will receive a compiler warning, indicating the hiding is not clearly intentional.

**Can I use `new` and `override` at the same time?**

No, because `override` extends the base implementation, whereas `new` hides it.

**When should I use the `new` modifier?**

Use it when you specifically want to hide inherited members from the base class to provide a clear, new implementation.
