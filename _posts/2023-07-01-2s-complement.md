---
layout: post
title:  "How computers represent integers: Two's complement notation"
date:   2023-07-01 06:04
author: ibrahim isa jajere
---

#### Prerequisites

You should know the things listed below if you want 0 WTFs as you read.

- [Binary](https://en.wikipedia.org/wiki/Binary_number)
- [Why or how 5 bits can be used to represent 32 different numbers](https://chat.openai.com/share/e2a5c658-a6b5-438b-9760-9fb5a6b39971)   

Integers simply refer to the whole spectrum of negative and positive whole numbers. 10, -4, 3.4(nope!), 19, etc etc. You as a human normally understand and perform operations on numbers represented in decimal (base ten) form; 10+4=14 8-3=5 etc etc. But computers don't understand base ten, for example there is no way a computer will see thirteen as a 1 followed by a 3. Nope. It has to be converted to binary (1s and 0s), which is the representation that modern computers like yours and mine understand.

OK fine let's convert the number (say, 13) to binary (1101) and that's it problem solved. Well, not entirely. Now the challenge is how do we represent negative integers like -3,-5 etc etc? Well, there are many approaches to solving that. And we'll look at them momentarily.

You see, with 5 bits we can represent 32 different numbers. If we are to use all of them to represent positive integers, we can represent a range of 0 to +31. This is called **unsigned integer representation**. However, we also need to represent negative integers as well, so we allocate half of the binary to positive integers and the other half to negative integers.

But now the problem is which integer do we allocate to which binary pattern. I mean what pattern should we represent 5 with? `0101`? OK, how about -5? For positive integers it is straightforward; we simply allocate `00000` - `01111` to 0 - +15. For the negative integers on the other hand, we allocate `10000` - `11111`. Now the "which binary pattern to which integer" problem narrows to the negative integers. Which binary pattern to which negative integer? or in other words: how do we compute the magnitude of a negative integer from the binary. I mean pick any binary from the negative range say 10110, how do we know WHICH negative integer it represents? Is it -6, -9,...? How do we know?

Now if you observe closely the binaries of all integers in the positive spectrum start with 0 while negative integers start with 1, so we can use the first bit of a binary string to know if it is positive (0) or negative (1). Let's call it the sign bit. We now know a binary represents a negative integer if the sign bit is one, but how do we compute its value? The thought that first come to mind is to use the rest of the digits to compute the magnitude. So `10000` is -0 (use the first bit to know the sign and the rest `0000` the magnitude), `10011` is -3 (again, 1 to know that its negative and `0011` to compute 3), etc etc. You guessed it, there is a representation that does that and it is called the **signed magnitude notation**.

That's not all. We can still come up with another method. Observe more closely...if you invert the binary of any positive integer (e.g. 00101=>11010) you get a negative number! and vice versa. That's it, simply invert a negative binary and compute the resulting positive binary as its magnitude. So the value of `11010` is -5. How? Well, -ve, because it starts with 1 and 5 because the inverse (or better, complement) of `11010` is `00101` which is equal to 5. The notation that works like this is called the **1's complement notation**.

Computing 2's complement representation isn't as straightforward as the ones we've discussed so far. To find the 2's complement of a negative integer, you first invert all the bits of its corresponding positive integer and add 1. This works the other way too; subtract 1 from the binary of a negative integer and then invert the bits to get the corresponding positive integer. For example, to find the representation of -6, you invert the binary of +6 and add 1. Let's do that step by step; What is the 2’s complement representation for -6?

- The representation of 6 is `00110`
- Invert the binaries of 6 and you get `11001`
- Now add 1 to `11001` we get `11010`. Therefore, the 2's complement of -6 is `11010`.

2's complement is the most commonly used notation to represent integers in modern computers. But why?

### Why 2's complement?

We know that subtraction is equivalent to the **addition** of a negative number; 5-3 ≣ 5 + (-3). But does the equivalence hold with binary numbers too? Subtraction to be equal to the addition of a negative binary? We have three different ways to represent negative integers in binary as we have seen, so the result is going to be different for each of them. Let's look at how the result of 5 + (-3) is computed using each of the notations:

#### 5 + (-3) in signed magnitude notation

In signed magnitude, 5 is `00101` and -3 is `10011`. The result of the addition:

```JavaScript
//  00101 => 5
// +10011 => -3
// ------------
//  11000 => -8
```

is -8🤯, but 5+(-3) is two!

#### 5 + (-3) in 1's complement

In 1's complement, 5 is `00101` and -3 is `11100`. the result of the addition is:

```JavaScript
//  00101 => 5
// +11100 => -3
// ------------
// 100001 => 1
```

is 1. At this point, you may wonder why 100001 (6 bits) is 1 not -1. Since the binary starts with a 1, shouldn't that make the result a negative number? Well, the reason is that we are dealing with 5-bit binaries here. Remember? That's why we truncate all bits after the fifth one, counting from right to left. Any extra bit that gets truncated is said to overflow. Now back to the main discussion. The result is still not two😞.

#### 5 + (-3) in 2's complement

In 2's complement, 5 is `00101` as usual and -3 is `11101`. the result of the addition:

```JavaScript
//  00101 => 5
// +11101 => -3
// ------------
// 100010 => 2 (again we delete the overflowed bit)
```

is 2! Yes finally!🎉

The addition of a negative binary as a means to achieve subtraction gives the correct result when the numbers are represented in 2's complement. And that means a computer can perform both addition and subtraction with only a circuitry of addition when the numbers are represented in 2's complement.

### Let's have some fun😁

Add one to the binary of the largest positive number that can be represented using 5 bits in 2's complement (+15 + 1); `01111` + `00001`. BUH hahaha you get `10000` which is -16! Boom and that's how you instantly go from the richest guy to the poorest –it only takes a single bit.


### I have a confession to make...

I mentioned that computers can only understand binary numbers. While that's true for modern computers like yours and mine, it is actually possible to make computers that can compute in decimal (base ten) or any base, but that'll be for another article😌.

### Summary

In this article, we've learned about unsigned integer representation, signed magnitude notation, 1's complement, 2's complement & why modern computers use it to represent integers and we also had a little fun with it.

✌️!