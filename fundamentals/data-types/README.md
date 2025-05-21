# Data types

C# is a strongly typed language, meaning every variable, constant, and expression has a well-defined type. The type system is fundamental for ensuring type safety, code clarity, and optimal performance. This article provides a detailed overview of the type system in C#.

### Unified Type System (Common Type System)

C# employs the **CTS**, which ensures interoperability between different .NET languages by standardizing types. CTS categorizes types into three primary groups:

1. **Value Types**
2. **Reference Types**
3. **Pointer Types**

Both value and reference types can be generic, meaning they can have parameters specifying the type arguments.

## Value vs Reference Types

| Feature               | Value Types                           | Reference Types                    |
| --------------------- | ------------------------------------- | ---------------------------------- |
| Stored in             | Stack (usually)                       | Heap                               |
| Lifetime              | Until method ends                     | Until GC collects                  |
| Passed by             | Value (copied)                        | Reference (pointer shared)         |
| GC involvement        | ❌ No                                  | ✅ Yes                              |
| Inheritance           | ❌ No (except from `System.ValueType`) | ✅ Yes                              |
| Custom types allowed? | ✅ `struct`                            | ✅ `class`, `interface`, `delegate` |



<table data-view="cards"><thead><tr><th></th><th data-type="content-ref"></th></tr></thead><tbody><tr><td>Value</td><td><a href="value-types.md">value-types.md</a></td></tr><tr><td>Reference</td><td></td></tr></tbody></table>
