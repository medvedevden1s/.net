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
    - Use H2 (`##`) for main sections. Use code style where applicable code blocks eg. ## The `is` operator
    - Use H3 (`###`) for subsections if needed.
    - Capitalize only the first word: `## Main method (traditional)`

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

Important: Use GitBook Markdown Syntax for Elements

## Paragraphs
Simply write text as usual. Separate paragraphs with a blank line.

## Headings
Use `#` for headings:
```markdown
# Heading 1
## Heading 2
### Heading 3
```

## Lists
- Unordered: start with `-` or `*`
- Ordered: use numbers
- Task lists: `- [ ]` unchecked, `- [x]` checked

```markdown
- Item 1
- Item 2
  - Sub-item

1. First
2. Second

- [ ] Task not done
- [x] Task done
```

## Quotes
```markdown
> This is a blockquote.
```

## Code Blocks
Basic:
````markdown
```csharp
Console.WriteLine("Hello, World!");
```
````

Advanced with options:
````markdown
{% code title="Example.cs" lineNumbers="true" overflow="wrap" %}
```csharp
Console.WriteLine("Hello, World!");
```
{% endcode %}
````

## Hints
```markdown
{% hint style="info" %}
Some useful tip.
{% endhint %}
```
Styles: `info`, `success`, `warning`, `danger`

## Tables
```markdown
| Column A | Column B |
| -------- | -------- |
| Value 1  | Value 2  |
| Value 3  | Value 4  |
```

## Cards
```markdown
<table data-view="cards">
  <thead>
    <tr>
      <th></th>
      <th></th>
      <th data-hidden data-card-target data-type="content-ref"></th>
      <th data-hidden data-card-cover data-type="files"></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>Card Title</strong></td>
      <td>Card description text.</td>
      <td><a href="https://example.com">https://example.com</a></td>
      <td><a href="image.png">image.png</a></td>
    </tr>
  </tbody>
</table>
```

## Columns
```markdown
{% columns %}
{% column width="50%" %}
Column 1 content
{% endcolumn %}

{% column %}
Column 2 content
{% endcolumn %}
{% endcolumns %}
```

## Tabs
```markdown
{% tabs %}
{% tab title="Windows" %}
Windows content here
{% endtab %}

{% tab title="macOS" %}
macOS content here
{% endtab %}
{% endtabs %}
```

## Stepper
```markdown
{% stepper %}
{% step %}
### Step 1
Do this
{% endstep %}

{% step %}
### Step 2
Do that
{% endstep %}
{% endstepper %}
```

## Expandable
```markdown
<details>
<summary>More info</summary>

Hidden text here.
</details>
```

## Images
```markdown
![Alt text](https://example.com/image.png)
```

With caption:
```markdown
<figure>
  <img src="https://example.com/image.png" alt="Description">
  <figcaption>Caption for image</figcaption>
</figure>
```
