# Boxing & Unboxing

### Boxing

Boxing is the process of converting a **value type** into a **reference type** by wrapping it inside an `object`.

```csharp
int x = 42;
object obj = x; // Boxing
```

### Unboxing

Unboxing is the reverse—**extracting** the value type from an object.

```csharp
int y = (int)obj; // Unboxing
```

### How It Works

Boxing copies the value into a **new object** on the **heap**. Unboxing requires **explicit casting** and retrieves the value from that heap object.

{% hint style="warning" %}
Boxing and unboxing involve memory allocation and type casting, which can affect performance in tight loops or high-frequency operations.
{% endhint %}

#### When to Avoid

* Inside performance-critical code
* When dealing with collections (use `List<int>` instead of `List<object>`)
