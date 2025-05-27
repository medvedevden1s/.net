# The extern keyword

### Introduction

The `extern` keyword allows you to declare methods implemented outside your managed code, typically in unmanaged libraries like native DLL files. It helps bridge the gap between managed and unmanaged code, enabling your application to leverage existing functionalities from external libraries.

### Why Use the `extern` Keyword?

Sometimes you need functionalities provided by external native libraries, often written in languages like C or C++. The `extern` keyword, together with `[DllImport]`, lets your code call these external functions directly.

### Examples

#### Example 1: Showing a Windows Message Box

Here's how you can display a Windows message box using an external function from `User32.dll`:

```csharp
using System;
using System.Runtime.InteropServices;

class Program
{
    [DllImport("User32.dll", CharSet = CharSet.Unicode)]
    public static extern int MessageBox(IntPtr hWnd, string text, string caption, int options);

    static void Main()
    {
        MessageBox(IntPtr.Zero, "Hello, world!", "My First Extern Call", 0);
    }
}
```

When you run this program, you'll see a message box saying "Hello, world!"

#### Example 2: Calling a Simple C Function

Imagine you have a simple C function:

```c
// mylib.c
int multiplyByTen(int num) {
    return num * 10;
}
```

After compiling it into a DLL (`mylib.dll`), you can call it like this:

```csharp
using System;
using System.Runtime.InteropServices;

class Program
{
    [DllImport("mylib.dll")]
    public static extern int multiplyByTen(int num);

    static void Main()
    {
        int result = multiplyByTen(5);
        Console.WriteLine($"5 multiplied by 10 is: {result}");
    }
}
```

Output:

```bash
5 multiplied by 10 is: 50
```

### External Aliases

External aliases help resolve naming conflicts between different assembly versions:

```csharp
extern alias OldVersion;
extern alias NewVersion;

// Usage example
OldVersion::Library.Class objOld = new OldVersion::Library.Class();
NewVersion::Library.Class objNew = new NewVersion::Library.Class();
```

### Important Notes

* Methods declared with `extern` must be static.
* You can't combine `extern` with `abstract`; these modifiers contradict each other.

### Key Points

* `extern` allows calling methods from unmanaged code.
* `[DllImport]` specifies the external DLL.
* External aliases handle conflicts between multiple assembly versions.

### Conclusion

Using `extern` simplifies integration with external libraries, enhancing your application's functionality and flexibility.

### FAQs

**Why must methods declared as `extern` also be static?**

Because external implementations don't have access to the instance state of managed objects.

**Can a method declared as `extern` also be abstract?**

No, these modifiers are incompatible.

**What's a typical use case for external aliases?**

Resolving conflicts when referencing multiple versions of the same assembly.

