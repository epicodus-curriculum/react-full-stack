**Goal:** Get comfortable with the basics of functional programming. Practice writing pure functions, closures, curried functions, and recursive functions.

## Warm Up
---

* What is a pure function? What are some of the benefits of pure functions?
* What is immutability? Why is it important in functional programming?
* What is a closure?
* Why would we want to curry a function?

## Code
---

### Coin Counter

Create a coin counter application that takes X amount of money (such as $4.99) and determines the exact amount of change needed in quarters, dimes, nickels and pennies. Do not worry about adding a user interface to the application. Instead, focus on writing good tests and functional code.

#### Part 1

Create a coin counter function that uses recursion to solve the problem.

#### Part 2

Create a coin counter function that uses a closure that can be used with functions for each type of change (quarters, nickels, dimes and pennies). You can use recursion if you like.

### Roman Numerals

You've already had a chance to create an application to convert numbers into Roman numerals. This time, solve the problem recursively!

Roman numerals are based on seven symbols:

```
Symbol  Value
I       1
V       5
X       10
L       50
C       100
D       500
M       1,000
```

The most basic rule is that you add the value of all the symbols: so II is 2, LXVI is 66, etc.

The exception is that there may not be more than three of the same characters in a row. Instead, you switch to subtraction. So instead of writing IIII for 4, you write IV (for 5 minus 1); and instead of writing LXXXX for 90, you write XC.

You also have to separate ones, tens, hundreds, and thousands. In other words, 99 is XCIX, not IC.  You cannot count higher than 3,999 in Roman numerals.

**Bonus:** Can you solve this problem using closures and currying?

### Prime Sifting

You may have solved this problem in C# or Ruby. This time, your goal is to solve it using recursion!

Given a number, write a method that returns all of the prime numbers less than that number.

This is a tricky problem and you should use the Sieve of Eratosthenes to solve it. Here's how the Sieve of Eratosthenes works to find a number up to a given `number`:

* Create a list of numbers from 2 through n: 2, 3, 4, ..., `number`.
* Initially, let `prime` equal 2, the first prime number.
* Starting from `prime`, remove all multiples of `prime` from the list.
* Increment `prime` by 1.
* When you reach `number`, all the remaining numbers in the list are primes.

You also might find [this video](https://www.youtube.com/watch?v=V08g_lkKj6Q) helpful in explaining the Sieve.

## Peer Code Review
---

* Code uses functional programming and does not mutate state.
* Code is well tested.
* Code demonstrates an understanding of closures, recursion, and other functional concepts.
* Application works as expected.
