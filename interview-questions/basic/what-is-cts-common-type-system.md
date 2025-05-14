# What is CTS (Common Type System)?

***

### 🧠 Why does CTS exist?

Because .NET supports **multiple languages**, and they all need to:

* Understand each other's **types** (e.g., a `class` in C# should work with F# code)
* Share the **same runtime behavior** (value/reference types, memory layout, etc.)
* Avoid **type mismatches** across languages

So CTS ensures a **common standard** for:

| Concept         | Example                          |
| --------------- | -------------------------------- |
| Type categories | Value types, reference types     |
| Type rules      | Inheritance, casting, boxing     |
| Memory layout   | How structs/classes are laid out |
| Type safety     | Ensures predictable behavior     |

***

### 📦 Key Components of CTS

#### 1. ✅ **Value Types vs Reference Types**

| Type           | Stored In | Example                     |
| -------------- | --------- | --------------------------- |
| Value Type     | Stack     | `int`, `bool`, `struct`     |
| Reference Type | Heap      | `class`, `string`, `object` |

CTS defines **how they're passed**, copied, and garbage collected.

***

#### 2. ✅ **Primitive Types (Cross-language compatibility)**

CTS standardizes the type system so that:

| C#       | CTS Type         | [VB.NET](http://vb.net) Equivalent |
| -------- | ---------------- | ---------------------------------- |
| `int`    | `System.Int32`   | `Integer`                          |
| `string` | `System.String`  | `String`                           |
| `bool`   | `System.Boolean` | `Boolean`                          |

No matter the language, they **map to the same underlying CLR type**.

***

#### 3. ✅ **Rules for Inheritance & Polymorphism**

CTS enforces:

* Single inheritance for classes
* Multiple interface implementation
* Method override rules (`virtual`, `override`, etc.)
* Type visibility (`public`, `internal`, etc.)

***

### 🔗 CTS vs CLS vs CLR

| Concept | Description                                                                                          |
| ------- | ---------------------------------------------------------------------------------------------------- |
| **CTS** | Defines all supported types and how they behave at runtime                                           |
| **CLS** | (Common Language Specification) — A **subset** of CTS that defines rules all .NET languages agree on |
| **CLR** | (Common Language Runtime) — The actual engine that **executes code and manages memory**              |

***

### 🧠 Real-World Use Case

If you write a `class` in C# and another team writes F# code, **CTS ensures** that your types and theirs work together without runtime type confusion.

***

### 📌 Interview Summary

> "CTS, or Common Type System, defines the rules for how types are defined and behave in .NET. It allows different languages like C#, F#, and [VB.NET](http://vb.net) to interoperate safely by sharing a unified type system. CTS defines value vs reference types, inheritance rules, method signatures, and how types map to the CLR. It’s the foundation that enables cross-language compatibility in .NET."
