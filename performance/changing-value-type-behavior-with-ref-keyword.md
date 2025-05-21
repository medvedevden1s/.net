# Changing Value-Type Behavior with ref keyword

In C#, **value types** (`struct`) behave differently from **reference types** (`class`). By default, passing structs to methods creates copies, which can introduce significant overhead, especially in performance-critical scenarios like game development or real-time processing.

Let's explore:

* **Why** we might change value-type behavior using the `ref` keyword.
* **How** exactly to do it.
* Clear, measurable **performance benefits** through a real-world example.

***

### **Quick Recap: Value vs Reference**

| Type                       | Stored            | Behavior on Assignment | Behavior on Method Call |
| -------------------------- | ----------------- | ---------------------- | ----------------------- |
| **Value type (struct)**    | Stack _(usually)_ | Copies value           | Copies entire struct    |
| **Reference type (class)** | Heap              | Copies reference       | Copies reference only   |

**Structs** are great for performance, but copying large structs repeatedly in tight loops can slow down your application.

***

### **Real-World Scenario: Updating Millions of Game Objects**

In game development, you often handle large collections of small objects—such as particles, bullets, or positions—updated multiple times per second. Efficient memory use and speed are crucial to maintain smooth performance.

Consider a simple scenario where you update the position of 1,000,000 particles each frame:

```csharp
csharpCopyEditstruct Vector3D
{
    public float X, Y, Z;
}
```

***

### ❌ **Problematic: Passing Structs by Value**

By default, structs are copied when passed to methods:

```csharp
csharpCopyEditvoid MoveVector(Vector3D vector, Vector3D delta)
{
    vector.X += delta.X;
    vector.Y += delta.Y;
    vector.Z += delta.Z;
}

void Main()
{
    const int count = 1_000_000;
    Vector3D[] positions = new Vector3D[count];
    Vector3D delta = new Vector3D { X = 1f, Y = 1f, Z = 1f };

    var sw = Stopwatch.StartNew();

    for (int i = 0; i < count; i++)
    {
        MoveVector(positions[i], delta); // copies struct every iteration
    }

    sw.Stop();
    Console.WriteLine($"Elapsed without ref: {sw.ElapsedMilliseconds} ms");
}
```

**🚨 Issue:**

* Each method call **copies the entire struct** (12 bytes).
* 1,000,000 iterations result in **millions of unnecessary copies**, severely affecting performance.

***

### ✅ **Solution: Using `ref` to Pass Structs by Reference**

Using the `ref` keyword allows us to pass the original struct directly, avoiding copies:

```csharp
csharpCopyEditvoid MoveVector(ref Vector3D vector, Vector3D delta)
{
    vector.X += delta.X;
    vector.Y += delta.Y;
    vector.Z += delta.Z;
}

void Main()
{
    const int count = 1_000_000;
    Vector3D[] positions = new Vector3D[count];
    Vector3D delta = new Vector3D { X = 1f, Y = 1f, Z = 1f };

    var sw = Stopwatch.StartNew();

    for (int i = 0; i < count; i++)
    {
        MoveVector(ref positions[i], delta); // no copies, direct modification
    }

    sw.Stop();
    Console.WriteLine($"Elapsed with ref: {sw.ElapsedMilliseconds} ms");
}
```

**Benefits:**

* No unnecessary copying overhead.
* Significantly faster performance.

***

### **Performance Results (Illustrative):**

Typical measured results clearly demonstrate why using `ref` is advantageous:

| Method               | Approximate elapsed time |
| -------------------- | ------------------------ |
| Without `ref` (copy) | \~30-50 ms ⚠️ slower     |
| With `ref` (no copy) | \~10-15 ms 🚀 faster     |

_(Exact times depend on hardware and system load.)_

***

### **When Should You Use `ref` for Structs?**

Use `ref` keyword when:

* You're passing large structs or even small structs repeatedly (e.g., in tight loops).
* Performance matters greatly (games, graphics, simulations, real-time apps).
* You explicitly want the method to modify the original struct directly.

***

### **Common Mistakes & Misconceptions:**

* **"Always use `ref` for structs."** ❌\
  **Truth:** Use `ref` strategically, only when copying overhead or direct modification is important.
* **"Structs are always faster."** ❌\
  **Truth:** Small structs typically perform better, but unnecessary copying can make them slower than reference types in large-scale operations without `ref`.

***

### **Potential Pitfalls of Using `ref`:**

* Can lead to unexpected side-effects if misused (methods modifying original data unintentionally).
* Reduces readability if overused; clarity and explicitness of intent are important.

***

### **Best Practices:**

* Clearly document methods that use `ref`, indicating they modify the original data.
* Only use `ref` when you have a measurable performance benefit.
* Always prefer clarity and maintainability first; optimize performance second.

***

### **Summary: Why Change Struct Behavior with `ref`?**

| Reason                          | Explanation                                                      |
| ------------------------------- | ---------------------------------------------------------------- |
| **Performance Optimization** 🚀 | Avoid costly copies, improving performance                       |
| **Direct Modification** 🛠️     | Modify original struct directly within methods                   |
| **Efficiency in Loops** 🔄      | Critical in scenarios like games, graphics, real-time processing |

***

### **Final Takeaway**

Using the `ref` keyword to alter struct behavior from copying (value-type) to direct referencing (reference-type-like) is powerful. It can significantly enhance performance and efficiency, particularly in performance-critical scenarios.

Apply this technique thoughtfully and strategically, and you'll optimize your applications effectively!
