# The extern alias

### Introduction

Sometimes your application might need to reference two assemblies that contain identical fully-qualified type names. For instance, different versions of the same library may be required simultaneously. To manage this scenario clearly, C# provides the **extern alias** feature, enabling unambiguous references through distinct alias names.

### Using Extern Alias

To use external aliases, first declare them at compile-time to distinguish the assemblies clearly:

```bash
/r:LibV1=LibraryV1.dll
/r:LibV2=LibraryV2.dll
```

This defines external aliases (`LibV1`, `LibV2`) that can be explicitly used in your code:

```csharp
extern alias LibV1;
extern alias LibV2;

var item1 = new LibV1::Namespace.Item();
var item2 = new LibV2::Namespace.Item();
```

This ensures your references remain clear and prevents ambiguity when using types from different assemblies.

### Key Points:

* External aliases help resolve conflicts when two assemblies share identical type names.
* Use `extern alias` at compile-time to differentiate assemblies.
* External aliases introduce root-level namespaces parallel to the global namespace.
* In Visual Studio, set aliases through assembly properties.

### FAQs

**Can extern aliases be defined in code only?**\
No. Aliases must be defined at compile-time, typically using compiler options or project properties.

**Are external aliases commonly used?**\
They’re useful in special cases (e.g., maintaining legacy code alongside newer versions of the same library).

**Do extern aliases affect runtime performance?**\
No, extern aliases are resolved at compile-time and don't impact runtime performance.

**What happens if aliases aren’t defined?**\
Ambiguity arises, causing compilation errors, as the compiler can’t distinguish between identical types from different assemblies.
