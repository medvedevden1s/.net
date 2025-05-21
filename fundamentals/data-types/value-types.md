# Value Types

### Introduction

Value types are fundamental in C#, directly storing data within their memory locations. Understanding value types enhances your grasp of memory management, application performance, and efficient programming in C#.

### What are Value Types?

Value types store their values directly within the variable itself. These are allocated on the stack memory, offering faster data access and efficient memory management. They differ from reference types, which store references to data located on the heap.

### Built-in Value Types

### Numeric Types

* **Integral types:** `byte`, `short`, `int`, `long`
* **Floating-point types:** `float`, `double`
* **Decimal type:** `decimal`

### Non-numeric Types

* **Boolean type:** `bool`
* **Character type:** `char`

```csharp
int age = 30;
double salary = 55000.75;
bool isActive = true;
char initial = 'A';
```

### Custom Value Types: Structs and Enums

### Structs

Structs allow the creation of custom value types for encapsulating related data. They're ideal for lightweight data structures.

```csharp
struct Point
{
    public int X;
    public int Y;
}

Point p = new Point { X = 10, Y = 20 };
```

### Enums

Enums define named integral constants, useful for clear, maintainable code.

```csharp
enum Days
{
    Monday,
    Tuesday,
    Wednesday
}

Days today = Days.Tuesday;
```

### Summary

* Value types store data directly in their variables.
* Structs and enums are powerful custom value types.
* Understanding the distinction between value and reference types is essential for efficient programming.
* Minimize boxing/unboxing operations for optimal performance.

| `bool`            | Built-in     | Stack      | Flags, toggles (`isEnabled = true`)                  |
| ----------------- | ------------ | ---------- | ---------------------------------------------------- |
| `byte`            | Built-in     | Stack      | Low-level data, networking (1-byte data fields)      |
| `sbyte`           | Built-in     | Stack      | Signed 8-bit numbers                                 |
| `short`           | Built-in     | Stack      | Memory-efficient integer ranges                      |
| `ushort`          | Built-in     | Stack      | Positive-only 16-bit counters                        |
| `int`             | Built-in     | Stack      | Most general-purpose integer arithmetic              |
| `uint`            | Built-in     | Stack      | File sizes, IDs where negatives don't apply          |
| `long`            | Built-in     | Stack      | Timestamps, large counters                           |
| `ulong`           | Built-in     | Stack      | Non-negative long values                             |
| `float`           | Built-in     | Stack      | 3D graphics, math with low precision                 |
| `double`          | Built-in     | Stack      | Scientific computations, average calculation         |
| `decimal`         | Built-in     | Stack      | Money, financial apps (exact decimal precision)      |
| `char`            | Built-in     | Stack      | Single character input (`char key = 'A';`)           |
| `DateTime`        | Struct       | Stack      | Timestamps, scheduling                               |
| `TimeSpan`        | Struct       | Stack      | Measuring durations                                  |
| `Guid`            | Struct       | Stack      | Unique IDs for records (`Guid.NewGuid()`)            |
| `Enum`            | Struct       | Stack      | Named constants (`enum Status { Active, Inactive }`) |
| `struct` (custom) | User-defined | Stack/Heap | Lightweight data models (e.g., `Point`, `Color`)     |
