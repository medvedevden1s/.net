## Introduction

Pointer operators in C# are like special tools that let you work directly with memory addresses. Just as a physical address helps you locate a specific house, pointer operators help you locate and manipulate specific memory locations. These operators allow you to peek behind the curtain of C#'s memory management, giving you direct access to the raw memory where your data lives.

While most C# code doesn't require pointers, they become essential when you need to interact with unmanaged code or optimize performance-critical sections. Understanding pointer operators is like having a master key to the inner workings of your program's memory.

## Address-of operator (`&`)

The unary `&` operator returns the memory address of its operand. It's like asking for the exact location where a value is stored.

```csharp
unsafe
{
    int number = 27;
    int* pointerToNumber = &number;

    Console.WriteLine($"Value of the variable: {number}");
    Console.WriteLine($"Address of the variable: {(long)pointerToNumber:X}");
}
// Output is similar to:
// Value of the variable: 27
// Address of the variable: 6C1457DBD4
```

The operand of the `&` operator must be a fixed variable - one that resides in a storage location unaffected by the garbage collector. Local variables (like `number` in the example above) are fixed because they reside on the stack.

For variables that can be moved by the garbage collector (like object fields or array elements), you must use the `fixed` statement to temporarily pin them in place:

```csharp
unsafe
{
    byte[] bytes = { 1, 2, 3 };
    fixed (byte* pointerToFirst = &bytes[0])
    {
        // The address stored in pointerToFirst
        // is valid only inside this fixed statement block.
    }
}
```

{% hint style="warning" %}
You can't get the address of a constant or a value using the `&` operator.
{% endhint %}

## Pointer indirection operator (`*`)

The unary `*` operator, also known as the dereference operator, obtains the value stored at the memory location pointed to by its operand. If a pointer is like an address, dereferencing is like visiting that address to see what's there.

```csharp
unsafe
{
    char letter = 'A';
    char* pointerToLetter = &letter;
    Console.WriteLine($"Value of the `letter` variable: {letter}");
    Console.WriteLine($"Address of the `letter` variable: {(long)pointerToLetter:X}");

    *pointerToLetter = 'Z';  // Change the value at the pointed location
    Console.WriteLine($"Value of the `letter` variable after update: {letter}");
}
// Output is similar to:
// Value of the `letter` variable: A
// Address of the `letter` variable: DCB977DDF4
// Value of the `letter` variable after update: Z
```

The dereference operator allows you to both read from and write to the memory location pointed to by a pointer.

{% hint style="danger" %}
You can't apply the `*` operator to an expression of type `void*`.
{% endhint %}

## Pointer member access operator (`->`)

The `->` operator combines pointer indirection and member access in a single operation. It's a shorthand that makes working with structures through pointers more convenient.

For a pointer `p` of type `T*` and a member `m` of type `T`, the expression `p->m` is equivalent to `(*p).m`:

```csharp
public struct Coords
{
    public int X;
    public int Y;
    public override string ToString() => $"({X}, {Y})";
}

public class PointerMemberAccessExample
{
    public static unsafe void Main()
    {
        Coords coords;
        Coords* p = &coords;
        p->X = 3;  // Same as (*p).X = 3;
        p->Y = 4;  // Same as (*p).Y = 4;
        Console.WriteLine(p->ToString());  // output: (3, 4)
    }
}
```

This operator is particularly useful when working with structures through pointers, as it reduces the need for parentheses and makes the code more readable.

## Pointer element access operator (`[]`)

The pointer element access operator allows you to access elements at a specific offset from a pointer, similar to array indexing. For a pointer `p`, the expression `p[n]` is evaluated as `*(p + n)`.

```csharp
unsafe
{
    char* pointerToChars = stackalloc char[123];

    for (int i = 65; i < 123; i++)
    {
        pointerToChars[i] = (char)i;
    }

    Console.Write("Uppercase letters: ");
    for (int i = 65; i < 91; i++)
    {
        Console.Write(pointerToChars[i]);
    }
}
// Output:
// Uppercase letters: ABCDEFGHIJKLMNOPQRSTUVWXYZ
```

In this example, `stackalloc` allocates memory on the stack, and the `[]` operator is used to access individual elements.

{% hint style="danger" %}
The pointer element access operator doesn't check for out-of-bounds errors, which can lead to unpredictable behavior or crashes if you access memory outside the allocated block.
{% endhint %}

## Pointer arithmetic operators

Pointers support several arithmetic operations that allow you to navigate through memory:

### Addition or subtraction of an integral value

Adding an integer to a pointer advances the pointer by that many elements of the pointed type:

```csharp
unsafe
{
    const int Count = 3;
    int[] numbers = new int[Count] { 10, 20, 30 };
    fixed (int* pointerToFirst = &numbers[0])
    {
        int* pointerToLast = pointerToFirst + (Count - 1);

        Console.WriteLine($"Value {*pointerToFirst} at address {(long)pointerToFirst}");
        Console.WriteLine($"Value {*pointerToLast} at address {(long)pointerToLast}");
    }
}
```

The actual memory offset is calculated as `n * sizeof(T)`, where `n` is the integer value and `T` is the pointed type.

### Pointer subtraction

Subtracting one pointer from another gives you the number of elements between them:

```csharp
unsafe
{
    int* numbers = stackalloc int[] { 0, 1, 2, 3, 4, 5 };
    int* p1 = &numbers[1];
    int* p2 = &numbers[5];
    Console.WriteLine(p2 - p1);  // output: 4
}
```

The result is of type `long` and represents the difference in elements, not bytes.

### Pointer increment and decrement

The `++` and `--` operators increment or decrement a pointer by one element:

```csharp
unsafe
{
    int* numbers = stackalloc int[] { 0, 1, 2 };
    int* p1 = &numbers[0];
    int* p2 = p1;
    
    Console.WriteLine($"Before operation: p1 - {(long)p1}, p2 - {(long)p2}");
    Console.WriteLine($"Postfix increment of p1: {(long)(p1++)}");
    Console.WriteLine($"Prefix increment of p2: {(long)(++p2)}");
    Console.WriteLine($"After operation: p1 - {(long)p1}, p2 - {(long)p2}");
}
```

The postfix form (`p++`) returns the original value before incrementing, while the prefix form (`++p`) returns the new value after incrementing.

## Pointer comparison operators

Pointers can be compared using the standard comparison operators: `==`, `!=`, `<`, `>`, `<=`, and `>=`. These operators compare the actual memory addresses as if they were unsigned integers.

```csharp
unsafe
{
    int a = 10;
    int b = 20;
    int* pa = &a;
    int* pb = &b;
    
    if (pa < pb)
        Console.WriteLine("Pointer to 'a' comes before pointer to 'b' in memory");
    else
        Console.WriteLine("Pointer to 'b' comes before pointer to 'a' in memory");
        
    // Check if two pointers point to the same location
    int* pc = pa;
    Console.WriteLine(pa == pc);  // True
    Console.WriteLine(pa == pb);  // False
}
```

## Operator precedence

When working with multiple pointer operators, understanding precedence is crucial:

1. Highest precedence: Postfix operators (`x++`, `x--`) and the `->` and `[]` operators
2. Prefix operators (`++x`, `--x`) and the address-of (`&`) and dereference (`*`) operators
3. Arithmetic operators (`+`, `-`)
4. Comparison operators (`<`, `>`, `<=`, `>=`)
5. Equality operators (`==`, `!=`)

You can always use parentheses to override the default precedence and make your code more readable.

{% hint style="info" %}
A user-defined type cannot overload the pointer-related operators `&`, `*`, `->`, and `[]`.
{% endhint %}

## Key Points

- Pointer operators require an `unsafe` context and the `AllowUnsafeBlocks` compiler option.
- The `&` operator gets the address of a variable (must be a fixed variable).
- The `*` operator dereferences a pointer to access the value it points to.
- The `->` operator combines dereferencing and member access (`p->m` is equivalent to `(*p).m`).
- The `[]` operator provides element access at an offset from a pointer.
- Pointer arithmetic allows adding/subtracting integers, subtracting pointers, and incrementing/decrementing.
- Pointer comparison operators compare memory addresses as unsigned integers.

## FAQs

- **Why do I need to use the `unsafe` keyword with pointers?**  
  Pointers bypass C#'s type safety and memory management, which can lead to memory corruption or security vulnerabilities if used incorrectly. The `unsafe` keyword signals these risks.

- **What's the difference between fixed and movable variables?**  
  Fixed variables have stable memory locations that won't be changed by the garbage collector. Movable variables (like object fields) can be relocated during garbage collection and must be "pinned" with the `fixed` statement before taking their address.

- **Can I use pointer arithmetic with any pointer type?**  
  No, you cannot perform arithmetic operations on `void*` pointers because the size of the pointed element is unknown.

- **What happens if I access memory outside the bounds of an allocated block?**  
  This results in undefined behavior - it might corrupt memory, crash your program, or silently produce incorrect results. Pointer operations don't include bounds checking.

- **When should I use pointers in C#?**  
  Pointers should be used sparingly, primarily for interoperability with unmanaged code, accessing hardware directly, or in performance-critical code where managed alternatives are too slow.