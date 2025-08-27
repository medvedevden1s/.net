# GitBook Article Writing Guidelines

## Introduction

All articles must balance **simplicity** and **depth**.  
We write so that:
- A junior developer can **understand immediately**.
- A senior developer or even the **creator of C# would still learn something new**.

We achieve this by explaining topics in **layers**:
1. **Beginner Layer** – analogy or plain explanation (like teaching a kid).
2. **Developer Layer** – practical code with clear comments.
3. **Expert Layer** – deep dive into internals, runtime behavior, compiler details, pitfalls, and advanced use cases.

This way, articles remain **beginner-friendly**, but **technically complete** with no gaps.

---

## Structure

1. **Always start with `## Introduction`**
    - Short, clear description of why the topic matters.
    - Use analogies (e.g., compare memory to a bookshelf, methods to recipes, etc.).

2. **Section Headings**
    - Use H2 (`##`) for main sections.
    - Use H3 (`###`) for subsections if needed.
    - Capitalize only the first word: `## Main method (traditional)`

3. **Code Samples**
    - Always in C# code blocks.
    - No “Example:” word before code — just provide the snippet.
    - Every snippet must have **comments** so even a junior dev understands it.

   ```csharp
   using System;

   class Program
   {
       static void Main(string[] args)
       {
           // Print all arguments passed into the program
           Console.WriteLine("Arguments passed:");
           foreach (var arg in args)
           {
               Console.WriteLine(arg);
           }
       }
   }
   ```

4. **Hints**  
   Use GitBook hints for emphasis:

   ```markdown
   {% hint style="info" %}
   The runtime calls `Main` directly, so it must always be static.
   {% endhint %}
   ```

   Styles:
    - `info` → general notes
    - `success` → best practices
    - `warning` → common mistakes
    - `danger` → critical pitfalls

5. **Formatting**
    - Inline images allowed:  
      `<img src="../../.gitbook/assets/command_icon_light.svg" alt="Command Icon" data-size="line">`
    - Use lists for clarity.
    - You may use `<mark style="color:orange;background-color:purple;">highlighted text</mark>` for emphasis.

---

## Content Rules

- **Never say “In C#”** – it’s obvious.
- **Concise but deep** – short sentences, layered depth.
- **Cover everything that could confuse juniors.**
- **Always end with**:
    - `## Key Points` (summary bullets)
    - `## FAQs` (interview-style questions + answers)

---

## Writing Style

- **Like teaching a kid** → “The Main method is like the starting point of a video game. When you press play, that’s where the action begins.”
- **Like teaching a developer** → “The Main method must be static because the runtime calls it before any objects exist.”
- **Like teaching a guru** → “If multiple `Main` methods exist, the compiler requires an explicit entry point, resolved via the `/main` option. Also, async Main is transformed by the compiler into a state machine returning a Task.”

Each article should **flow naturally**:
- First: easy explanation.
- Then: real code.
- Then: advanced internals & pitfalls.
- Close with summary + FAQs.

---

## Article Flow Template

```markdown
## Introduction  
Brief, analogy-rich explanation.

## Section 1 (concept)  
Explanation + code + hints.

## Section 2 (deeper dive)  
Runtime details, compiler behavior, advanced scenarios.

{% hint style="warning" %}
Only one file in a project may contain top-level statements.
{% endhint %}

## Key Points  
- Bullet summary of the topic.  
- Must be static.  
- Can return void, int, Task, or Task<int>.  
- Top-level statements generate Main implicitly.  

## FAQs  
- **Why must Main be static?**  
  Because the runtime calls it directly without creating an instance.  

- **Can a program have multiple Main methods?**  
  Yes, but you must specify the entry point explicitly.  

- **Can Main be async?**  
  Yes, starting with C# 7.1, you can return `Task` or `Task<int>`.  

- **Is args mandatory?**  
  No, use it only if you need command-line arguments.  
```
