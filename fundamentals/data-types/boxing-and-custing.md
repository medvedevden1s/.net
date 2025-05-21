# Boxing & Custing

### 🔖 Boxing and Unboxing

Boxing converts a value type to an object, while unboxing extracts the value type from an object.

**Example:**

```
int number = 100;
object boxedNumber = number; // Boxing
int unboxedNumber = (int)boxedNumber; // Unboxing
```

> ⚠️ Boxing/unboxing involves performance overhead. Minimize unnecessary boxing to improve performance.
