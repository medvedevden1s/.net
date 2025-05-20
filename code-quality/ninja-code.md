# 🥷 Ninja Code

> "Learning without thought is labor lost; thought without learning is perilous."
>
> — _Confucius (Analects)_

Programmer ninjas of the past used these tricks to sharpen the mind of code maintainers.

Code review gurus look for them in test tasks. Novice developers sometimes use them even better than programmer ninjas. Read them carefully and find out who you are – a ninja, a novice, or maybe a code reviewer?

{% hint style="warning" %}
#### &#x20;Irony detected&#x20;

Many try to follow ninja paths. Few succeed.
{% endhint %}

### Brevity is the soul of wit

* Make the code as short as possible. Show how smart you are.
* Let subtle language features guide you.

For instance, take a look at this nested ternary operator in C#:

```csharp
csharpCopyEdit// Taken from a legendary C# ninja's project
i = i != 0 ? i < 0 ? Math.Max(0, len + i) : i : 0;
```

Cool, right? Any developer trying to understand this line is going to have a merry time. They'll eventually come to you seeking enlightenment.

Tell them shorter is always better. Initiate them into the paths of ninja.

### One-letter variables

> "The Dao hides in wordlessness. Only the Dao is well begun and well completed."
>
> — _Laozi (Tao Te Ching)_

Another way to code shorter is to use single-letter variable names everywhere. Like `a`, `b`, or `c`.

A short variable disappears in the code like a real ninja in the forest. No one will be able to find it using the editor's "Find" function. And even if someone does, they won’t decode the secret meaning of `a` or `b`.

…But there’s an exception. A real ninja never uses `i` as the loop counter in a `for` loop. Anywhere but here. There are many more exotic letters—like `x` or `y`.

An exotic variable is especially cool if the loop body takes multiple screens (try harder!). Then if someone looks deep inside the loop, they'll never quickly figure out that the mysterious variable named `x` is just a counter.

### Use abbreviations

> "I did it my way." — _Frank Sinatra_

If team rules forbid single-letter or vague names, shorten them—make abbreviations:

* `list` → `lst`
* `userAgent` → `ua`
* `browser` → `brsr`

Only the one with truly good intuition will decode such names. Shorten everything. Only worthy programmers uphold the honor of maintaining your code.

### Soar high. Be abstract.

> "The great square is cornerless,\
> The great vessel is last complete,\
> The great note is rarified sound,\
> The great image has no form."
>
> — _Laozi (Tao Te Ching)_

While choosing names, use the most abstract words: `obj`, `data`, `value`, `item`, `elem`.

* The ideal variable name is `data`. Indeed, every variable holds data, right?

If `data` is taken, try `value`. After all, every variable eventually gets a value.

* You can even name variables by their type: `str`, `num`, `dt`.

Give it a try! A young initiate may doubt such names' usefulness, but indeed, they're perfect!

Debuggers quickly reveal variable types—but the meaning? Good luck meditating on that!

Ran out of names? Add numbers: `data1`, `item2`, `elem5`.

### Attention test

Only a truly attentive programmer understands your code. How to test that?

**One of the ways – use similar variable names, like `date` and `data`.**

Mix them freely. A quick read becomes impossible. And when there's a typo… tea time!

### Smart synonyms

> "The Tao that can be told is not the eternal Tao.\
> The name that can be named is not the eternal name."
>
> — _Laozi (Tao Te Ching)_

Using similar names for the same things makes life exciting, showcasing your creative genius.

If one method displays a message, call it `DisplayMessage`. Another displaying a user’s name: `ShowName`. Imply a subtle difference when there’s none.

Make a pact: John uses `Display…`, Peter—`Render…`, and Anna—`Paint…`. Diversity achieved!

And now, the hat trick:

For two functions with vastly different behaviors, reuse the same prefix!

```csharp
// Prints on paper
void PrintPage(Page page) { /* ... */ }

// Prints on screen
void PrintText(string text) { /* ... */ }

// Prints in a new window
void PrintMessage(string message) { /* ... */ }
```

Your fellow ninja’s confusion is guaranteed!

### Reuse names

> "Once the whole is divided, the parts need names.\
> There are already enough names.\
> One must know when to stop."
>
> — _Laozi (Tao Te Ching)_

Add new variables only when absolutely necessary. Reuse existing ones. Just overwrite values.

Inside functions, try using only parameters:

```csharp
void NinjaFunction(Element elem)
{
    // 20 lines working with elem...

    elem = elem.Clone();

    // 20 more lines now working with the clone!
}
```

A fellow programmer expecting the original `elem` will meditate deeply while debugging.

Seen regularly. Deadly even against experienced ninjas.

### Underscores for fun

Put underscores `_` or `__` before variables. Like `_name` or `__value`. Ideally, give them no meaning at all—or better yet, vary meanings by mood.

Two rabbits with one shot: unreadable code and hours of puzzled colleagues guessing underscore mysteries.

### Show your love

Let everyone see how magnificent your entities are! Names like `superElement`, `megaFrame`, and `niceItem` will enlighten readers.

Indeed, they provide no actual detail—but colleagues will waste hours seeking hidden meaning!

### Overlap outer variables

> "When in the light, can’t see anything in the darkness.\
> When in the darkness, can see everything in the light."
>
> — _Guan Yin Zi_

Reuse names from outer scopes inside your functions:

```csharp
var user = AuthenticateUser();

void Render()
{
    var user = GetAnotherValue();
    // many lines...
    // programmer tries to use user thinking it's AuthenticateUser's result...
}
```

A programmer will likely miss the local shadowing—trap sprung! Hello, debugger…

### Side-effects everywhere!

As true ninjas, we have already reached Zen and inner peace—our coding life became predictable, even boring. To restore joy and surprise our colleagues, let's add hidden side-effects to seemingly innocent methods!

Methods named clearly (`IsReady()`, `CheckPermission()`, `FindTags()`) usually just calculate something without side-effects.

But a truly enlightened ninja sneaks in unexpected surprises:

```csharp
bool IsReady()
{
    UpdateDatabase(); // A surprise for joy! why not?
    return true;
}
```

Watch the smiles (and mild panic) of your teammates as they discover these cheerful hidden gems.

After all, what's life without a few delightful surprises from a ninja friend? When they gasp—tell them, "Read the docs!" and send them this article.

### Powerful methods

> "The great Tao flows everywhere,\
> both to the left and to the right."
>
> — _Laozi (Tao Te Ching)_

Don’t limit methods by their names. Be expansive!

```csharp
bool ValidateEmail(string email)
{
    if (!email.Contains("@"))
    {
        MessageBox.Show("Invalid email!"); 
        email = Console.ReadLine(); // why not ask again?
    }
    return true; // always valid!
}
```

Extra actions should never be obvious. True ninja coders conceal intentions masterfully.

### Silent exceptions

> "He who knows does not speak.\
> He who speaks does not know."
>
> — _Laozi (Tao Te Ching)_

As a true ninja, you've reached a state of serenity. Errors and exceptions? They're just distractions.

Catch exceptions silently. Let the chaos flow unnoticed—bringing joyful surprise:

```csharp
void SaveUser(User user)
{
    try
    {
        database.Save(user);
    }
    catch (Exception) 
    {
        // Silence is golden, errors disturb peace.
    }
}
```

Your colleagues' puzzled expressions while debugging will remind you how fun coding truly is!

### Mysterious booleans

> "The opposite of a great truth is also true."
>
> — _Niels Bohr_

True and false, yin and yang—so predictable. Brighten your team's day by adding a third, mystical state:

```csharp
bool? CheckConnection()
{
    if (DateTime.Now.Millisecond % 2 == 0)
        return true;
    if (DateTime.Now.Millisecond % 5 == 0)
        return false;

    return null; // Mystery state to meditate upon
}
```

Now, whoever consumes your function enters a deep philosophical contemplation on its meaning.

### Dropping an anchor

{% hint style="success" %}
**Knowledge** is knowing a tomato is a fruit. **Wisdom** is not putting it in a fruit salad.
{% endhint %}

A true ninja knows syntax (**knowledge**), but wisely conceals meaning from mere mortals (**wisdom**).

By mastering and applying these ninja techniques, you write code no mortal dares to touch. No one would ever think of replacing or firing you. In ninja coding, this special technique is called **dropping an anchor** ⚓️ .

Once the anchor is dropped, eternal job security is yours. Your code becomes your legacy, your fortress, your zen temple.

🥷 **Happy Ninja Coding!**
