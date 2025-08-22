# The unsafe keyword

### Introduction

`unsafe` opens a special context where you can work with pointers and do low-level memory operations. It’s powerful—and risky. Use it only when you have a clear performance or interop need (e.g., talking to native APIs) and when safe alternatives aren’t enough.

{% hint style="warning" %}
`unsafe` code is _unverifiable_ by the runtime. Bugs here can corrupt memory, crash your process, or open security holes. Keep it minimal, isolated, and well‑tested.
{% endhint %}

### What Is An Unsafe Context

There are two ways to enable it:

*   **Unsafe modifier on a member or type**

    ```csharp
    public class Blitter
    {
        public unsafe static void FastCopy(byte* src, byte* dst, int count)
        {
            for (int i = 0; i < count; i++)
                dst[i] = src[i];
        }
    }
    ```
*   **Unsafe block inside a safe method**

    ```csharp
    public static void Demo()
    {
        unsafe
        {
            int x = 10;
            int* px = &x;   // address-of
            *px = 25;       // dereference
        }
    }
    ```

Scope of the unsafe context covers parameters, local variables, and everything inside that member/block.

### Enabling Unsafe Code

*   **Project file**

    ```xml
    <PropertyGroup>
      <AllowUnsafeBlocks>true</AllowUnsafeBlocks>
    </PropertyGroup>
    ```
*   **Compiler switch**

    ```
    csc /unsafe Program.cs
    // or
    dotnet build -p:AllowUnsafeBlocks=true
    ```



{% hint style="info" %}
Prefer safe constructs first: `Span<T>`, `Memory<T>`, `ArrayPool<T>`, `Buffer.MemoryCopy`, `MemoryMarshal` APIs.&#x20;

Use `unsafe` only when measurements prove it necessary.
{% endhint %}

### Core Building Blocks

#### Pointers, Address-of, Dereference

```csharp
unsafe
{
    int value = 7;
    int* p = &value; // address-of
    *p = *p * *p;    // dereference and read/write
    System.Console.WriteLine(value); // 49
}
```

#### Fixed To Pin Managed Memory

The GC can move managed arrays/strings. `fixed` pins them to a stable address so you can take a pointer.

```csharp
public static unsafe int Sum(int[] data)
{
    if (data is null || data.Length == 0) return 0;

    fixed (int* p = data)          // pin the array
    {
        int sum = 0;
        for (int i = 0; i < data.Length; i++)
            sum += p[i];           // pointer indexing
        return sum;
    } // unpinned here
}
```

{% hint style="danger" %}
Pinning too much or for too long fragments the heap and hurts GC performance. Keep `fixed` scopes as short as possible.
{% endhint %}

#### Stackalloc For Fast Temporary Buffers

Allocates memory on the stack (auto-freed when the scope ends).

```csharp
public static unsafe int ParseTwoDigits(ReadOnlySpan<byte> utf8)
{
    // expects ASCII digits like "42"
    byte* buf = stackalloc byte[2];
    for (int i = 0; i < 2; i++) buf[i] = utf8[i];

    int tens = buf[0] - (byte)'0';
    int ones = buf[1] - (byte)'0';
    return tens * 10 + ones;
}
```

You can also use `Span<T>` over a `stackalloc` buffer:

```csharp
Span<byte> temp = stackalloc byte[256];
```

#### Pointer Arithmetic And Member Access

```csharp
public struct Pair { public int A; public int B; }

public static unsafe int SumPairs(Pair[] items)
{
    int sum = 0;
    fixed (Pair* p = items)
    {
        Pair* cur = p;
        for (int i = 0; i < items.Length; i++, cur++)
        {
            // (*cur).A is equivalent to cur->A
            sum += cur->A + cur->B;
        }
    }
    return sum;
}
```

#### Interop: Passing Buffers To Native Code

```csharp
using System.Runtime.InteropServices;

internal static class Native
{
    [DllImport("mylib")]
    public static extern unsafe void Process(byte* data, int length);
}

public static unsafe void CallNative(Span<byte> buffer)
{
    fixed (byte* p = buffer)
    {
        Native.Process(p, buffer.Length);
    }
}
```

#### Sizeof For Built-in Types And Unmanaged Structs

```csharp
unsafe
{
    int bytes = sizeof(long); // 8
}
```

### A Safe-Oriented Fast Copy With An Unsafe Fallback

```csharp
using System;
using System.Runtime.InteropServices;

public static class Copier
{
    public static void FastCopy(ReadOnlySpan<byte> src, Span<byte> dst)
    {
        if (src.Length > dst.Length)
            throw new ArgumentException("Destination too small.");

        // Safe, fast path
        src[..dst.Length].CopyTo(dst);

        // Optional: measured hotspot where an unsafe path wins
        // Uncomment only if profiling shows benefit:
        /*
        unsafe
        {
            fixed (byte* ps = src, pd = dst)
            {
                Buffer.MemoryCopy(ps, pd, dst.Length, dst.Length);
            }
        }
        */
    }
}
```

{% hint style="success" %}
Start with the _safe_ `Span<T>` approach. Add an `unsafe` path only if a profiler proves it’s meaningfully faster on your target workloads.
{% endhint %}

### Common Pitfalls

* **Dangling pointers**: Never store a pointer outside the `fixed` scope that produced it.
* **Wrong lifetimes**: `stackalloc` memory dies when the scope exits—don’t return pointers to it.
* **Type punning**: Reinterpreting bits across unrelated types can break alignment/endianness and the type system—use `MemoryMarshal` helpers if you must.
* **Overflow**: Pointer arithmetic doesn’t do bounds checks. Validate lengths manually.

### Key Points

* `unsafe` enables pointers, `&`, `*`, `->`, pointer arithmetic, `stackalloc`, `fixed`, and `sizeof`.
* Use `unsafe` via a modifier on a member/type or an `unsafe` block.
* Turn it on with `<AllowUnsafeBlocks>true</AllowUnsafeBlocks>` or `/unsafe`.
* Keep `fixed` scopes tight; minimize pinning time.
* Prefer `Span<T>`, `Memory<T>`, and BCL intrinsics first. Add `unsafe` only when profiling proves a win.
* Treat `unsafe` sections like you would C code: review carefully, test thoroughly, and keep them small.

### FAQs

**Is unsafe code “unmanaged”?**\
No. It still runs in the managed process, uses the GC, and P/Invokes the same. It’s just _unverifiable_ and bypasses some safety checks.

**Do I need administrator rights to compile unsafe code?**\
No. You only need the project/compile setting enabled (AllowUnsafeBlocks or `/unsafe`).

**Can I use pointers with any type?**\
Pointers are meant for _unmanaged_ types (built-in numerics and structs containing only unmanaged fields). Managed references (`object`, arrays, classes) can only be used via `fixed` to get a temporary pinned pointer to their data.

**When should I prefer `stackalloc` vs `ArrayPool<T>`?**\
Use `stackalloc` for very small, short‑lived buffers (typically up to a few hundred bytes) inside tight scopes. Use `ArrayPool<T>` for larger or longer‑lived temporary buffers without pinning.

**Is pointer arithmetic faster than indexed loops?**\
Not automatically. Modern JITs optimize bounds‑checked loops heavily. Measure before assuming `unsafe` is faster.

**Can I mix safe and unsafe code in the same method?**\
Yes—wrap the minimal part in an `unsafe` block. Keep the rest safe.

**How do I debug unsafe code?**\
Use the same debugger; step carefully, watch memory windows, and consider adding assertions and extra checks in debug builds.
