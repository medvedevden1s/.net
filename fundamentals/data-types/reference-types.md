# Reference types

### Introduction

Reference types don't store their actual data within the variable itself. Instead, they store a reference (pointer) to the location in memory where the actual data is held, typically allocated on the heap.

Built-in reference types include:

* **Object**
* **Dynamic**
* **String**
* **Arrays**

Custom reference types include:

* **Class**
* **Interface**
* **Delegate**

***

### Built-in Reference Types

&#x20;**Object Type**

The `object` type (`System.Object`) is the ultimate base class for all types (value and reference) in the .NET Common Type System (CTS). An `object` can store values of any type, though conversion (boxing/unboxing) may be required.

**Boxing and Unboxing**:

* **Boxing**: converting a value type to an object type.
* **Unboxing**: converting an object back into a value type.

```csharp
object obj = 1001; // Boxing (integer into object)
Console.WriteLine("Student ID: " + obj);

obj = "Sudhir Sharma"; // Storing string
Console.WriteLine("Student Name: " + obj);
```

_Output:_

```
yamlCopyEditStudent ID: 1001
Student Name: Sudhir Sharma
```

***

**Dynamic Type**

The `dynamic` type allows variables to bypass compile-time type checking; type checking happens at runtime. It is particularly useful when interacting with dynamically typed languages or COM objects.

```csharp
dynamic value = 10;
Console.WriteLine("Dynamic value: " + value);

value = "Hello, World!";
Console.WriteLine("Dynamic now contains: " + value);
```

_Output:_

```
sqlCopyEditDynamic value: 10
Dynamic now contains: Hello, World!
```

**Note:**

* `dynamic` is similar to `object`, but `object` checks types at compile-time, while `dynamic` does so at runtime.

***

**String Type**

The `string` type (`System.String`) stores text as a sequence of Unicode characters. Strings are immutable in C#—changes to a string create new string instances.

```csharp
csharpCopyEditstring firstName = "Sudhir";
string lastName = "Sharma";
string fullName = firstName + " " + lastName;

Console.WriteLine("Full Name: " + fullName);
```

_Output:_

```
pgsqlCopyEditFull Name: Sudhir Sharma
```

**Using verbatim strings (with `@`)**:

```csharp
csharpCopyEditstring path = @"C:\Users\Sudhir";
```

**Note:** Use `StringBuilder` for efficient string modifications.

***

**Array Type**

Arrays store multiple values of the same data type in contiguous memory locations.

**Example:**

```csharp
csharpCopyEditstring[] students = { "Zoya", "Yashna", "Olivia", "Naomi" };

Console.WriteLine("Student List:");
foreach (string student in students)
{
    Console.WriteLine(student);
}
```

_Output:_

```
yamlCopyEditStudent List:
Zoya
Yashna
Olivia
Naomi
```

***

#### Custom Reference Types

**1. Classes**

Classes define custom reference types, serving as blueprints for creating objects (instances).

**Declaring a class:**

```csharp
csharpCopyEditpublic class Customer
{
    // Fields, properties, methods, and events go here
}
```

**Creating Objects:**

Objects (instances) are created using the `new` keyword.

```csharp
csharpCopyEditCustomer customer1 = new Customer(); // Creating a new object
Customer customer2 = customer1;      // Both refer to the same object
```

***

**2. Constructors and Initialization**

Constructors initialize class instances.

* **Default Initialization:** Variables get default values (`0` for numbers, `null` for references).
* **Field Initialization:** Explicitly initializing fields.
* **Constructor Parameters:** Explicitly setting values through constructor parameters.

```csharp
csharpCopyEdit// Field initializer
public class Container
{
    private int _capacity = 10;
}

// Constructor parameters
public class Container
{
    private int _capacity;
    public Container(int capacity) => _capacity = capacity;
}

// Primary constructor (C# 12+)
public class Container(int capacity)
{
    private int _capacity = capacity;
}
```

***

**3. Class Inheritance**

Inheritance enables one class (derived class) to inherit properties, methods, and behavior from another class (base class).

**Example:**

```csharp
csharpCopyEditpublic class Employee
{
    public string Name { get; set; }
}

public class Manager : Employee
{
    public string Department { get; set; }
}
```

* Classes can inherit from only one base class but can implement multiple interfaces.

***

#### Key Takeaways

* Reference types store references (addresses) to data in heap memory.
* Objects, arrays, strings, and dynamic types are fundamental built-in reference types.
* Classes define user-customized reference types, with inheritance providing flexible object-oriented design.
* Constructors ensure objects are initialized correctly.

***

### FAQs about Reference Types

**Q1: What's the difference between value types and reference types?**

* **Value types** store data directly within the variable's memory.
* **Reference types** store references to the actual data located on the heap.

**Q2: What is boxing and unboxing?**

* **Boxing** converts a value type into an object.
* **Unboxing** extracts the value type from an object.

```csharp
csharpCopyEditint num = 10;        // value type
object obj = num;    // boxing
int num2 = (int)obj; // unboxing
```

**Q3: When should I use `dynamic` instead of `object`?**

* Use `dynamic` when you need runtime type flexibility, such as interoperating with dynamic languages or COM APIs.

**Q4: How do you efficiently modify strings in C#?**

* Use the `StringBuilder` class to efficiently perform numerous or extensive string modifications.

```csharp
csharpCopyEditStringBuilder sb = new StringBuilder();
sb.Append("Hello");
sb.Append(" World");
string result = sb.ToString();
```

**Q5: Can a reference type variable be `null`?**

* Yes, reference types can be assigned `null`, indicating that they don't currently reference any object.

```csharp
csharpCopyEditCustomer customer = null; // valid for reference types
```
