# Method parameters and modifiers

### Introduction

Method parameters in programming determine how arguments are passed to methods, influencing whether methods modify original data or operate on copies. In this article, we'll explore key parameter passing concepts, including passing by value, passing by reference, and parameter modifiers like `ref`, `out`, `in`, and `params`.

### Pass by value

By default, arguments are passed by value, meaning methods receive a copy of the argument:

* **Value types (struct)**: Methods receive a copy of the data.
* **Reference types (class)**: Methods receive a copy of the reference to the object.



```csharp
public struct Point { public int X, Y; }
public class Point3D { public int X, Y, Z; }

void Mutate(Point p) { p.X = 10; p.Y = 20; }
void Mutate(Point3D p) { p.X = 10; p.Y = 20; p.Z = 30; }

Point point = new Point { X = 1, Y = 2 };
Point3D point3D = new Point3D { X = 1, Y = 2, Z = 3 };

Mutate(point);  // point remains unchanged
Mutate(point3D);  // point3D properties change

Console.WriteLine($"Point: {point.X}, {point.Y}");       // Output: Point: 1, 2
Console.WriteLine($"Point3D: {point3D.X}, {point3D.Y}, {point3D.Z}");  // Output: Point3D: 10, 20, 30
```

### Pass by reference&#x20;

Passing arguments by reference allows methods to modify the original variable directly:

```csharp
void Mutate(ref Point p) { p.X = 10; p.Y = 20; }

Point point = new Point { X = 1, Y = 2 };
Mutate(ref point);  // point is modified directly

Console.WriteLine($"Point: {point.X}, {point.Y}");  // Output: Point: 10, 20
```

### Parameter Modifiers

#### `ref` Parameter Modifier

To use a `ref` parameter, both the method definition and calling method must explicitly use the `ref` keyword. The argument must be initialized before passing.

```csharp
void AddTen(ref int number)
{
    number += 10;
}

int num = 5;
AddTen(ref num);
Console.WriteLine(num);  // Output: 15
```

#### `out` Parameter Modifier

An `out` parameter allows methods to return values through parameters. The calling method doesn't need to initialize the argument, but the method must assign it before returning.

```csharp
void Initialize(out int number)
{
    number = 20;
}

int result;
Initialize(out result);
Console.WriteLine(result);  // Output: 20
```

#### `in` Parameter Modifier

The `in` modifier passes arguments by reference for read-only purposes. It's beneficial for large structs to avoid the overhead of copying.

```csharp
void DisplayValue(in int number)
{
    Console.WriteLine(number);
    // number = 10; // Error: number is read-only
}

int num = 30;
DisplayValue(in num);  // Output: 30
```

#### `ref readonly` Modifier

`ref readonly` explicitly specifies that a struct is passed by reference and is strictly read-only. This modifier is primarily used for performance optimization.

```csharp
public struct LargeStruct { public int Data; }

void ProcessData(ref readonly LargeStruct data)
{
    Console.WriteLine(data.Data);
    // data.Data = 10; // Error: Cannot modify because it's readonly
}

LargeStruct structData = new LargeStruct { Data = 50 };
ProcessData(ref structData); // Output: 50
```

#### `params` Modifier

The `params` modifier allows methods to accept a variable number of arguments. No other parameters can follow `params`, and only one `params` modifier is allowed per method.

```csharp
void Sum(params int[] numbers)
{
    int total = 0;
    foreach (var number in numbers)
        total += number;

    Console.WriteLine($"Sum: {total}");
}

Sum(1, 2, 3, 4); // Output: Sum: 10
Sum(); // Output: Sum: 0
```

### Key Points

* **Pass by Value**: Default, safe, method receives a copy.
* **Pass by Reference (`ref`)**: Original variable modified.
* **`out`**: For method-defined initialization.
* **`in` and `ref readonly`**: Performance optimization for large structs.
* **`params`**: Flexible argument lists.

Understanding these parameter modifiers helps write clear, efficient, and safe code.
