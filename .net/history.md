# History

### Origins of .NET

The idea behind .NET began in the **late 1990s**, as part of Microsoft’s **Next Generation Windows Services (NGWS)** strategy. Microsoft envisioned a platform to simplify application development across multiple languages and systems.

In **2000**, Microsoft released the first beta of **.NET Framework**, which officially launched on **February 13, 2002**. It introduced key concepts still used today:

* **Common Language Runtime (CLR)** for managed code execution
* **Framework Class Library (FCL)** for consistent APIs
* Support for multiple languages via **Common Language Infrastructure (CLI)**

{% hint style="info" %}
.NET Framework was originally proprietary but quickly aligned with standards. CLI and C# were ratified by **ECMA** in 2001 and by **ISO** in 2003. The latest standard is [ECMA-334](https://ecma-international.org/wp-content/uploads/ECMA-334_7th_edition_december_2023.pdf)\

{% endhint %}

***

### .NET Framework Era (2002–2022)

The original **.NET Framework** ran **exclusively on Windows** and powered Windows Forms, ASP.NET, WPF, and more.

#### Notable Versions

| Version | Year      | Key Features                          |
| ------- | --------- | ------------------------------------- |
| 1.0     | 2002      | Initial release with CLR and FCL      |
| 2.0     | 2005      | Generics, nullable types              |
| 3.0     | 2006      | WPF, WCF, WF                          |
| 3.5     | 2007      | LINQ, lambda expressions              |
| 4.0     | 2010      | Dynamic Language Runtime              |
| 4.5–4.7 | 2012–2017 | Async/await, better performance       |
| 4.8     | 2019      | Final major release                   |
| 4.8.1   | 2022      | Security & accessibility improvements |

***

### Shift to Cross-Platform: .NET Core (2016–2020)

By 2016, Microsoft recognized the need for a **modern, modular, open-source** version of .NET. Thus, **.NET Core** was born:

* Fully cross-platform: Windows, Linux, macOS
* Faster and more lightweight
* Command-line-first development model
* Emphasis on cloud-native and containerization

#### Key Releases

| Version  | Year | Notable Additions                    |
| -------- | ---- | ------------------------------------ |
| Core 1.0 | 2016 | First cross-platform release         |
| Core 2.0 | 2017 | Expanded APIs                        |
| Core 3.1 | 2019 | LTS, WPF/WinForms support on Windows |

***

### Unified Modern .NET: .NET 5 and Beyond

In **November 2020**, Microsoft introduced **.NET 5**, unifying the previously fragmented ecosystem:

* One SDK, one runtime
* Unified base class libraries
* Improved AOT, cloud, and performance tooling
* MAUI for cross-platform UI

#### Modern .NET Versions

| Version | Year | Highlights                                                                                   |
| ------- | ---- | -------------------------------------------------------------------------------------------- |
| .NET 5  | 2020 | First unified platform                                                                       |
| .NET 6  | 2021 | LTS, minimal APIs, Hot Reload                                                                |
| .NET 7  | 2022 | MAUI stable, cloud-native focus                                                              |
| .NET 8  | 2023 | Blazor streaming, AI & WASM improvements                                                     |
| .NET 9  | 2024 | AOT enhancements, web and MAUI updates                                                       |
| .NET 10 | 2025 | **(Latest)** Full Native AOT, new SDK patterns, AI integration, improved Blazor/MAUI tooling |

***

### From Proprietary to Open Source

Initially met with scepticism by open-source communities due to licensing concerns and patent ambiguities, Microsoft gradually opened up the .NET ecosystem:

* In **2007**, .NET 3.5 libraries were published under a reference license.
* In **2014**, Microsoft extended its **patent promise** to cover more .NET technologies.
* In **2016**, after acquiring Xamarin, Microsoft **open-sourced Mono** under the **MIT License**.
* By **2018**, the source code for **WPF, WinForms, and WinUI** was released publicly.



{% hint style="info" %}
The entire modern .NET stack is now hosted on [GitHub](https://github.com/dotnet), governed by the .NET Foundation, and built with community collaboration.\

{% endhint %}

***

### Timeline Summary

| Year | Event                                   |
| ---- | --------------------------------------- |
| 2002 | .NET Framework 1.0 released             |
| 2005 | Generics introduced in .NET 2.0         |
| 2016 | .NET Core 1.0 released (cross-platform) |
| 2020 | .NET 5 unifies .NET Core & Mono         |
| 2023 | .NET 8 delivers cloud-native and WASM   |
| 2025 | .NET 10 with AI, full Native AOT        |



* **.NET Framework** – Windows-only legacy framework
* **.NET Core** – Modular, fast, cross-platform (now legacy)
* **.NET (5+)** – Unified, future-facing, LTS support
* **Mono** – Lightweight, open-source runtime
* **Xamarin & MAUI** – Cross-platform mobile and desktop
* **Blazor** – C# in the browser via WebAssembly
* **Orleans** – Actor-based cloud services framework
* **Aspire** - Opinionated stack for building cloud-native, observable, and composable distributed apps.



Support

| Version                                                  | Release Date       | Latest Update   | Latest Update Date | Support Ends      | Support Lifetime      |
| -------------------------------------------------------- | ------------------ | --------------- | ------------------ | ----------------- | --------------------- |
| <mark style="background-color:red;">.NET Core 1.0</mark> | June 27, 2016      | 1.0.16          | May 14, 2019       | June 27, 2019     | 3 years               |
| <mark style="background-color:red;">.NET Core 1.1</mark> | November 16, 2016  | 1.1.13          | May 14, 2019       | June 27, 2019     | 2.5 years             |
| <mark style="background-color:red;">.NET Core 2.0</mark> | August 14, 2017    | 2.0.9           | July 10, 2018      | October 1, 2018   | 1.25 years            |
| <mark style="background-color:red;">.NET Core 2.1</mark> | May 30, 2018       | 2.1.30 (LTS)    | August 19, 2021    | August 21, 2021   | 3.25 years            |
| <mark style="background-color:red;">.NET Core 2.2</mark> | December 4, 2018   | 2.2.8           | November 19, 2019  | December 23, 2019 | 0.9 years             |
| <mark style="background-color:red;">.NET Core 3.0</mark> | September 23, 2019 | 3.0.3           | February 18, 2020  | March 3, 2020     | 0.5 years             |
| <mark style="background-color:red;">.NET Core 3.1</mark> | December 3, 2019   | 3.1.32 (LTS)    | December 13, 2022  | December 13, 2022 | 3 years               |
| <mark style="background-color:red;">.NET 5</mark>        | November 10, 2020  | 5.0.17          | May 10, 2022       | May 10, 2022      | 1.5 years             |
| <mark style="background-color:red;">.NET 6</mark>        | November 8, 2021   | 6.0.36 (LTS)    | November 12, 2024  | November 12, 2024 | 3 years               |
| <mark style="background-color:red;">.NET 7</mark>        | November 8, 2022   | 7.0.20          | May 28, 2024       | May 14, 2024      | 1.5 years             |
| <mark style="background-color:yellow;">.NET 8</mark>     | November 14, 2023  | 8.0.13 (LTS)    | February 11, 2025  | November 10, 2026 | 3 years               |
| <mark style="background-color:green;">**.NET 9**</mark>  | November 12, 2024  | 9.0.2           | February 11, 2025  | May 12, 2026      | 1.5 years             |
| <mark style="background-color:blue;">**.NET 10**</mark>  | November 2025\*    | _(will be LTS)_ | –                  | November 2028\*   | 3 years (projected)   |
| <mark style="background-color:blue;">**.NET 11**</mark>  | November 2026\*    | –               | –                  | May 2028\*        | 1.5 years (projected) |

**Legend**:\
🟥 Old version, not maintained\
🟨 Old version, still maintained\
🟩 Latest LTS version\
🟦 Future version (projected)
