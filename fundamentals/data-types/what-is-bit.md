# What is bit?

### Introduction

Everything in a computer is built from the simplest unit of information: the **bit**.\
Understanding what a bit is, and how multiple bits work together, is key to understanding memory, numbers, and bitwise operations.

***

### What Is a Bit?

A **bit** (short for **binary digit**) is the smallest piece of data in computing.

* A bit can only be **0** or **1**.
* Inside the computer, 0 often means **off**, and 1 means **on** (like an electrical switch).

{% hint style="info" %}
Think of a bit as a light switch: it’s either **off (0)** or **on (1)**.
{% endhint %}

***

### From Bits to Bytes

* **8 bits = 1 byte**.
* A byte can represent values from **0 to 255** in binary.

Example:

```
Binary:  0100 0001
Decimal: 65
ASCII:   'A'
```

So the letter **A** is stored as bits!

***

### Why Do We Need Bits?

Computers work with electricity.

* A high voltage can mean `1`.
* A low voltage can mean `0`.

Bits are a way to represent these signals in **data and instructions**.

{% hint style="success" %}
All data — text, numbers, images, videos — is just billions of bits in memory.
{% endhint %}

***

### Numbers in Binary

Humans use **decimal** (base 10), but computers use **binary** (base 2).

* Decimal digits: `0–9`
* Binary digits: `0–1`

Example: number 5 in binary:

```
Decimal: 5
Binary:  101
```

(= 1×2² + 0×2¹ + 1×2⁰ = 4 + 0 + 1 = 5)

***

### Bits and Bitwise Operations

Once you know numbers are stored as **bits**, you can manipulate them directly.

Example:

```
a = 0101 (5 in decimal)
b = 0011 (3 in decimal)

a & b = 0001 (1 in decimal)
```

That’s why **bitwise operators** exist — they allow you to control and inspect **individual bits**.

***

### Key Points

* A **bit** is the smallest unit of information: 0 or 1.
* **8 bits = 1 byte**, the building block of storage.
* Computers use bits because they match the on/off nature of electrical circuits.
* Binary numbers are just bits arranged in order.
* Bitwise operators let you **manipulate individual bits** for efficiency.

***

### FAQs

**Q: Why 8 bits in a byte?**\
It’s a historical design choice that became standard in modern computing.

**Q: Can we have more than 0 and 1 in bits?**\
No — bits are always binary. Larger structures (bytes, words) represent bigger numbers.

**Q: Do images and videos also use bits?**\
Yes! Pixels in images and frames in videos are ultimately stored as bits.\
