# Value and Reference Types

## ✅ Value Types in .NET (Stored on Stack\*)

> \*Unless boxed or used inside a class — then stored on heap.

| Type              | Category     | Stored In  | Example Use Case                                     |
| ----------------- | ------------ | ---------- | ---------------------------------------------------- |
| `bool`            | Built-in     | Stack      | Flags, toggles (`isEnabled = true`)                  |
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
| `Enum` (any enum) | Struct       | Stack      | Named constants (`enum Status { Active, Inactive }`) |
| `struct` (custom) | User-defined | Stack/Heap | Lightweight data models (e.g., `Point`, `Color`)     |

***

## 🔗 Reference Types in .NET (Always on Heap)

| Type                    | Category          | Stored In | Example Use Case                                     |
| ----------------------- | ----------------- | --------- | ---------------------------------------------------- |
| `string`                | Class (immutable) | Heap      | Text, JSON keys, labels, search terms                |
| `object`                | Base class        | Heap      | Generic containers, reflection, boxing/unboxing      |
| `class` (custom)        | User-defined      | Heap      | Business logic, domain models (`Order`, `User`)      |
| `array[]`               | Reference type    | Heap      | Any list of values (`int[]`, `string[]`)             |
| `List<T>`               | Collection        | Heap      | Dynamic-size list (`List<string>`)                   |
| `Dictionary<K,V>`       | Collection        | Heap      | Key-value lookup (`userId → User`)                   |
| `Queue<T>`              | Collection        | Heap      | FIFO buffer/processing queues                        |
| `Stack<T>`              | Collection        | Heap      | LIFO undo buffer, state management                   |
| `HashSet<T>`            | Collection        | Heap      | Uniqueness enforcement                               |
| `Task` / `Task<T>`      | Async task        | Heap      | Async operation results                              |
| `Stream`                | Abstract class    | Heap      | File, network, memory stream abstraction             |
| `HttpClient`            | Class             | Heap      | Making web requests                                  |
| `delegate`              | Function pointer  | Heap      | Event handlers, plugin callbacks                     |
| `Func<T>` / `Action<T>` | Delegate          | Heap      | LINQ, functional-style methods                       |
| `dynamic`               | Runtime bound     | Heap      | Runtime-typed data from JSON, scripting, Excel, etc. |
| `Exception`             | Class             | Heap      | Handling errors (`try-catch`)                        |

***

## 📌 Summary Table: Value vs Reference Types

| Feature               | Value Types                           | Reference Types                    |
| --------------------- | ------------------------------------- | ---------------------------------- |
| Stored in             | Stack (usually)                       | Heap                               |
| Lifetime              | Until method ends                     | Until GC collects                  |
| Passed by             | Value (copied)                        | Reference (pointer shared)         |
| GC involvement        | ❌ No                                  | ✅ Yes                              |
| Inheritance           | ❌ No (except from `System.ValueType`) | ✅ Yes                              |
| Custom types allowed? | ✅ `struct`                            | ✅ `class`, `interface`, `delegate` |

***

## 🧠 Interview Soundbite

> "Value types are stored on the stack and are fast and lightweight — perfect for small, short-lived data like numbers or coordinates. Reference types are stored on the heap and are better suited for complex or long-lived data like objects, collections, and strings. Understanding this distinction helps you write performant and memory-efficient .NET code, especially under GC pressure."
