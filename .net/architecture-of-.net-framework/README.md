# Architecture of .NET Framework

## Architecture of .NET Framework

The .NET Framework consists of two major components:

* **Common Language Runtime (CLR)**
* **.NET Framework Base Class Library (BCL)**

<table><thead><tr><th>Component</th><th width="447.9998779296875">Description</th></tr></thead><tbody><tr><td><strong>Common Language Runtime (CLR)</strong></td><td>Manages code execution, memory, thread management, type safety, and security.</td></tr><tr><td><strong>Base Class Library (BCL)</strong></td><td>Provides fundamental types and APIs (strings, dates, numbers, collections, I/O).</td></tr><tr><td><strong>Common Type System (CTS)</strong></td><td>Defines how types are declared, used, and managed consistently across all .NET languages.</td></tr><tr><td><strong>Common Language Specification (CLS)</strong></td><td>Ensures interoperability among all .NET-supported programming languages.</td></tr></tbody></table>

#### How .NET Applications Execute

Applications built on the .NET Framework typically use programming languages such as:

* **C#**
* **F#**
* **Visual Basic**

When compiled, source code is transformed into a language-neutral format called **Common Intermediate Language (CIL)**, which is packaged into files known as assemblies (`.dll` or `.exe`).

#### Runtime Execution Process:

1. **Assembly Loading**: When the application starts, CLR loads the compiled assemblies.
2. **Just-in-Time (JIT) Compilation**: CLR uses the JIT compiler to convert the CIL code into platform-specific machine code dynamically, optimizing execution for the current hardware.
3. **Execution**: The resulting machine code runs directly on the target computer’s processor, delivering optimized and responsive application performance.

<figure><img src="../../.gitbook/assets/image (2).png" alt="Architecture of .NET Framework Diagram" width="375"><figcaption></figcaption></figure>

### FAQ

### What is the difference between .NET and .NET Framework?

.NET and .NET Framework share many core components but differ in key aspects:

* **Cross-Platform:** .NET runs on Linux, macOS, and Windows, while .NET Framework only runs on Windows.
* **Open Source:** .NET is open-source and actively accepts community contributions, whereas .NET Framework source code is available but does not accept direct contributions.
* **Innovation:** All new innovation occurs in .NET.
* **Distribution:** .NET Framework is included with Windows and updated through Windows Update; .NET is independently distributed.

### Can I have multiple .NET Framework versions installed?

Yes. Some .NET Framework versions install side-by-side, while others perform in-place updates when they share the same CLR version. For example:

* Installing .NET Framework 4.8 over 4.7.2 performs an in-place update but leaves separate installations (like 3.5) unaffected.

### Which .NET Framework version should I use?

Typically, use the latest stable release, currently .NET Framework 4.8.1. Applications built on earlier 4.x versions run seamlessly on 4.8.1. For applications targeting older versions, install the specific required version.

### What is the support policy for .NET Framework?

.NET Framework 4.8.1 is the latest version and will continue to be included with supported Windows versions. It remains supported as long as it is installed on a supported Windows release. For more information, see the [.NET Framework support policy](https://dotnet.microsoft.com/en-us/platform/support/policy/dotnet-framework).
