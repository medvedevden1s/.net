# Pointers

### 3. Pointer Type in C\#

Pointer type variables store the memory address of another type. Pointers in C# have the same capabilities as the pointers in C or C++.

Syntax for declaring a pointer type is −

```
type* identifier;
```

**Example**

```
using System;

unsafe class Program
{
    static void Main()
    {
        int grade = 90;
        int* ptr = &grade;

        Console.WriteLine("Original Grade: " + grade);
        Console.WriteLine("Memory Address: " + (ulong)ptr);

        *ptr = 95; // Modifying value using pointer
        Console.WriteLine("Updated Grade: " + grade);
    }
}
```

This example will produce the following output:

```
main.cs(3,1): error CS0227: Unsafe code requires the `unsafe' command line option to be specified
Compilation failed: 1 error(s), 0 warnings
```
