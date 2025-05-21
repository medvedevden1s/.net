# Boxing & Unboxing

### Boxing

Boxing converts a value type to an object, while unboxing extracts the value type from an object.

```
int number = 100;
object boxedNumber = number; // Boxing
int unboxedNumber = (int)boxedNumber; // Unboxing
```

{% hint style="warning" %}
Boxing/unboxing involves performance overhead. Minimize unnecessary boxing to improve performance.
{% endhint %}
