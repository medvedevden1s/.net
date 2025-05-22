# Keywords

### Introduction

In C#, **keywords** are special reserved words that have predefined meanings to the compiler. They are the fundamental building blocks of the language syntax and cannot be used as variable names, method names, or identifiers—unless prefixed with `@`.

### Reserved Keywords

Reserved keywords are always recognized by the compiler as having specific syntax-related meaning and cannot be redefined or reused.

Here's the full list of reserved keywords:

|          |           |            |           |
| -------- | --------- | ---------- | --------- |
| abstract | event     | namespace  | static    |
| as       | explicit  | new        | string    |
| base     | extern    | null       | struct    |
| bool     | false     | object     | switch    |
| break    | finally   | operator   | this      |
| byte     | fixed     | out        | throw     |
| case     | float     | override   | true      |
| catch    | for       | params     | try       |
| char     | foreach   | private    | typeof    |
| checked  | goto      | protected  | uint      |
| class    | if        | public     | ulong     |
| const    | implicit  | readonly   | unchecked |
| continue | in        | ref        | unsafe    |
| decimal  | int       | return     | ushort    |
| default  | interface | sbyte      | using     |
| delegate | internal  | sealed     | virtual   |
| do       | is        | short      | void      |
| double   | lock      | sizeof     | volatile  |
| else     | long      | stackalloc | while     |
| enum     |           |            |           |



You can use a reserved keyword as an identifier by prefixing it with `@`.

```csharp
int @class = 10; // Valid, but discouraged for readability
```

***

### Contextual Keywords

Unlike reserved keywords, **contextual keywords** are only treated as keywords in specific contexts. This design makes it easier to introduce new language features without breaking existing code.

Contextual keywords **can be used as identifiers**, except when they appear in their special context.

Here are some common contextual keywords:

|            |                                               |                  |                                                 |
| ---------- | --------------------------------------------- | ---------------- | ----------------------------------------------- |
| add        | field                                         | nint             | scoped                                          |
| allows     | file                                          | not              | select                                          |
| alias      | from                                          | notnull          | set                                             |
| and        | get                                           | nuint            | unmanaged (function pointer calling convention) |
| ascending  | global                                        | on               | unmanaged (generic type constraint)             |
| args       | group                                         | or               | value                                           |
| async      | init                                          | orderby          | var                                             |
| await      | into                                          | partial (type)   | when (filter condition)                         |
| by         | join                                          | partial (member) | where (generic type constraint)                 |
| descending | let                                           | record           | where (query clause)                            |
| dynamic    | managed (function pointer calling convention) | remove           | with                                            |
| equals     | nameof                                        | required         | yield                                           |
| extension  |                                               |                  |                                                 |



```csharp
public record Person(string Name, int Age); // "record" is a contextual keyword

var people = from p in persons
             where p.Age > 18
             select p.Name;                // "from", "where", and "select" are contextual
```

These keywords **can still be used as identifiers**:

```csharp
int select = 5; // Valid, because not used in a query expression
```

### Summary

| Type                   | Can be used as identifier? | Reserved always? | Examples                 |
| ---------------------- | -------------------------- | ---------------- | ------------------------ |
| **Reserved Keyword**   | No                         | Yes              | `class`, `int`, `static` |
| **Contextual Keyword** | Yes (outside context)      | No               | `record`, `var`, `async` |



***

### FAQs

**Can I use `class` as a variable name?**\
Only if you prefix it: `int @class = 1;` (but it's discouraged for clarity).

**Are contextual keywords reserved forever?**\
No. They're reserved only in certain contexts and are intentionally designed this way to avoid breaking older code.

**How do I know if a word is reserved or contextual?**\
If using the word causes a compile-time syntax error in most positions, it's reserved. If not, it's likely contextual.
