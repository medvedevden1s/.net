# .NET Owerview

The .NET Framework consists of next key components:

<table><thead><tr><th>Component</th><th width="447.9998779296875">Description</th></tr></thead><tbody><tr><td><strong>Common Language Runtime (CLR)</strong></td><td>Manages code execution, memory, thread management, type safety, and security.</td></tr><tr><td><strong>Base Class Library (BCL)</strong></td><td>Provides fundamental types and APIs (strings, dates, numbers, collections, I/O).</td></tr><tr><td><strong>Common Type System (CTS)</strong></td><td>Defines how types are declared, used, and managed consistently across all .NET languages.</td></tr><tr><td><strong>Common Language Specification (CLS)</strong></td><td>Ensures interoperability among all .NET-supported programming languages.</td></tr></tbody></table>

We will disscuss them in details in future articles, fow now you&#x20;

#### Execution Process:

1. **Source Code Compilation**
   * Your source code (written in C#, F#, or VB) is compiled into a platform-neutral Intermediate Language (CIL).
2. **Assembly Generation**
   * Compiled CIL is stored in assemblies, typically `.dll` or `.exe` files.
3. **Assembly Loading**
   * Upon application launch, the CLR loads your assemblies into memory.
4. **Just-in-Time (JIT) Compilation**
   * CLR dynamically compiles CIL into optimized machine-specific code using the JIT compiler.
5. **Execution**
   * The resulting machine code executes directly on your hardware for efficient performance.

<figure><img src="../.gitbook/assets/image (2).png" alt="Architecture of .NET Framework Diagram" width="375"><figcaption></figcaption></figure>

***

### FAQ

#### What is the difference between .NET and .NET Framework?

| Aspect             | .NET                             | .NET Framework                           |
| ------------------ | -------------------------------- | ---------------------------------------- |
| **Cross-Platform** | ✔️ Linux, macOS, Windows         | ❌ Windows only                           |
| **Open Source**    | ✔️ Open-source, community-driven | ❌ Source-available, no community changes |
| **Innovation**     | ✔️ Actively updated and enhanced | ❌ Maintenance updates only               |
| **Distribution**   | Independently distributed        | Bundled with Windows and Windows Updates |

***

#### Can multiple .NET Framework versions coexist?

Yes. Multiple versions can exist side-by-side without conflict, while some perform in-place upgrades:

* Installing **4.8 over 4.7.2** updates in place.
* Installing **3.5 and 4.8** side-by-side keeps both functional independently.

***

#### Which version of .NET Framework should I choose?

* **Use the latest stable release** (currently **.NET Framework 4.8.1**) for best compatibility and performance.
* Applications targeting earlier versions will run smoothly on 4.8.1.
* For legacy requirements, explicitly install the necessary older version.

***

#### How long will .NET Framework be supported?

* **.NET Framework 4.8.1** receives continuous support, tied directly to Windows OS lifecycle.
* Updates are regularly delivered through Windows Update. You can also check the history
