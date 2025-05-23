# Access Modifiers

### Introduction

Access modifiers define how visible a type or a member is from other parts of the program or even other assemblies.

Every **class**, **struct**, **interface**, **method**, **field**, or **property** has an _accessibility level_, which determines where it can be accessed.

They help enforce **encapsulation**, protect your APIs, and control **exposure** across components in your application or library.

***

### Access Modifiers

| **Modifier**         | **Scope / Meaning**                                                                                    |
| -------------------- | ------------------------------------------------------------------------------------------------------ |
| `public`             | Accessible from **any code** in **any assembly**. No restrictions.                                     |
| `private`            | Accessible **only within the containing type** (class, struct, or record). Not visible to other types. |
| `protected`          | Accessible within within **its class and by derived class instances**.                                 |
| `internal`           | Accessible to any code **within the same assembly**. Not accessible from another assembly.             |
| `protected internal` | Accessible from **the same assembly** or from **derived types in other assemblies**. (OR logic)        |
| `private protected`  | Accessible only within the **same assembly** and only to **derived types**. (AND logic)                |
| `file`               | Accessible **only within the same source file**. Useful for helper types in C# 11+                     |

***

### Summary Table

This table shows where members are accessible based on modifier and caller's location:

| Caller’s Location                      | `public` | `protected internal` | `protected` | `internal` | `private protected` | `private` | `file` |
| -------------------------------------- | -------- | -------------------- | ----------- | ---------- | ------------------- | --------- | ------ |
| Within the same file                   | ✔        | ✔                    | ✔           | ✔          | ✔                   | ✔         | ✔      |
| Within the same class                  | ✔        | ✔                    | ✔           | ✔          | ✔                   | ✔         | ❌      |
| Derived class (same assembly)          | ✔        | ✔                    | ✔           | ✔          | ✔                   | ❌         | ❌      |
| Non-derived class (same assembly)      | ✔        | ✔                    | ❌           | ✔          | ❌                   | ❌         | ❌      |
| Derived class (different assembly)     | ✔        | ✔                    | ✔           | ❌          | ❌                   | ❌         | ❌      |
| Non-derived class (different assembly) | ✔        | ❌                    | ❌           | ❌          | ❌                   | ❌         | ❌      |

***

### Member Accessibility

Members of classes and structs (fields, methods, properties, etc.) can use any of the access modifiers listed above, with some limitations:

| Member Type       | Valid Modifiers                                                                                                |
| ----------------- | -------------------------------------------------------------------------------------------------------------- |
| Class Members     | All (`public`, `private`, `protected`, `internal`, `protected internal`, `private protected`)                  |
| Struct Members    | Cannot use `protected`, `protected internal`, or `private protected` because structs don't support inheritance |
| Enum Members      | Always `public`, no modifier needed                                                                            |
| Finalizers        | Cannot use any access modifiers                                                                                |
| Interface Members | `public` by default (even if not explicitly stated)                                                            |
| File-Scoped Types | The `file` access modifier is allowed only on top-level (non-nested) type declarations.                        |

