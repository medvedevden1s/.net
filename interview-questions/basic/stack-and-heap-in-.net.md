# Stack and Heap in .NET

***

In .NET, memory is managed in two main areas:

| Memory Area | Purpose                                         |
| ----------- | ----------------------------------------------- |
| **Stack**   | For short-lived data: method calls, value types |
| **Heap**    | For long-lived objects: reference types, `new`  |

***

### 📦 Stack — Fast, Small, and Scoped

#### 🔍 What it stores:

* Method call information
* Local variables (`int`, `bool`, `struct`)
* Value type parameters

#### 📏 Stack Size:

| Thread Type                      | Default Stack Size |
| -------------------------------- | ------------------ |
| Main thread                      | \~1 MB             |
| Background thread (Task, Thread) | \~256 KB           |

#### ⚙️ Managed By:

* Compiler and .NET runtime
* Automatically pushes/pops stack frames per method call

#### 🧠 Stack Overflow:

Occurs when:

* You recurse deeply
* You allocate large local value types

```csharp
csharp
CopyEdit
void Infinite() => Infinite(); // StackOverflowException

```

> The stack is fast but small and not garbage collected.

***

### 💾 Heap — Large, Flexible, GC-managed

#### 🔍 What it stores:

* Objects created via `new` (reference types)
* Arrays, strings, classes, delegates

#### 📏 Heap Size:

| Platform | Limit (practical)             |
| -------- | ----------------------------- |
| 32-bit   | \~1.5–2 GB                    |
| 64-bit   | Practically unlimited (8+ TB) |

* Grows dynamically as needed
* No fixed size; governed by **GC and OS**

#### ⚙️ Managed By:

* The **.NET Garbage Collector**

When memory runs low:

* GC finds unreachable objects
* Frees memory
* Compacts heap if needed (except Large Object Heap)

#### ❌ Heap Overflow:

There's no `HeapOverflowException`, but you may get:

```csharp
csharp
CopyEdit
System.OutOfMemoryException

```

If the process exceeds physical memory limits.

***

### ⚖️ Comparison Table: Stack vs Heap

| Feature               | **Stack**                 | **Heap**                  |
| --------------------- | ------------------------- | ------------------------- |
| Use Case              | Value types, method calls | Objects, reference types  |
| Speed                 | ⚡ Very fast               | 🐢 Slower                 |
| Lifetime              | Ends with method          | Until GC collects         |
| Size                  | \~1 MB (main thread)      | GBs (up to system memory) |
| Managed by            | Compiler/runtime          | Garbage Collector         |
| GC Involved           | ❌ No                      | ✅ Yes                     |
| Exception on overflow | `StackOverflowException`  | `OutOfMemoryException`    |
| Thread Scope          | One per thread            | Shared across threads     |

***

### 🛠️ Example: Stack vs Heap in C\#

```csharp
csharp
CopyEdit
struct Point { public int X, Y; }
class Person { public string Name; }

void Demo()
{
    int a = 5;                   // Stack
    Point p = new Point();       // Stack (struct)
    var person = new Person();   // Reference lives on Stack, object on Heap
}

```

***

### 🔥 Key Insights

* Stack is **LIFO** (Last-In-First-Out)
* Stack memory is **freed immediately** on method return
* Heap memory is **collected later** by GC
* Stack is for **small, short-lived data**
* Heap is for **large, long-lived data**

***

### 🔐 Security & Performance Notes

| Concern               | Stack                | Heap                                |
| --------------------- | -------------------- | ----------------------------------- |
| Buffer overflow risk  | Low in managed .NET  | Higher in native interop            |
| Allocation time       | Constant time        | Varies (depends on GC)              |
| Multithreading impact | One stack per thread | Shared heap (needs synchronization) |

***

### 🧠 Interview Soundbite

> "The stack is a fast, fixed-size memory region used for method calls and short-lived value types. It's automatically managed by the compiler and runtime. A StackOverflowException occurs if you exceed its limit, typically \~1 MB. The heap is a large, flexible memory area managed by the garbage collector, used for reference types and objects created with new. If memory runs out, you get an OutOfMemoryException. Choosing between stack and heap affects performance, GC pressure, and memory usage."
