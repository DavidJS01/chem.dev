+++
title = "Isolating Stateful Changes for the OO Developer"
date = "2024-06-26"
draft = false
[extra]
summary = "TL;DR: More functions with consistent input/output, push state management, IO, etc, into edges of the application."

[taxonomies]

tags = ["fp", "development"]
+++


# Overview
There is a great book titled "Grokking Simplicity" that teaches how to think in a functional matter. 

By creating a clear division from Actions and Calculations, the software will be easier to test, debug, and reason about.

Essentially, you want:

- less code that depends on when or how it runs (Actions)
- more code with consistent inputs and outputs (Calculations)
- isolate impure functions/code (Actions) to the edge of the system



<center><a href="https://livebook.manning.com/book/grokking-simplicity/chapter-3/10"><p>https://livebook.manning.com/book/grokking-simplicity/chapter-3/10</p></a></center>

> For what it is worth, this title is opinionated. This content is pretty simple, but I think it is very valuable for unfamiliar developers.


# Actions
Actions are code whose behavior or output changes depending when or how it is ran. This is also known as "impure functions", functions with side-effects.

| Example                               | Explanation                                                                                                     |
|---------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| Reading from a DB                     | Data in the DB might change between read calls |
| Reading user input from a CLI calculator program | A CLI calculator depends on reading inputs from an external source, the user.                                                                                                               |

```py
# main has a side effect because it interacts with the external environment,
# accepting input from a user.
def main() -> None:
	num1 = int(input("Enter the first number: "))
	num2 = int(input("Enter the second number: "))
	
	print(num1 + num2)
```

# Calculations
Calculations are functions with consistent outputs given the same inputs.

Calculations do not contain side-effects (IO, system interactions, etc), and are known as "pure functions".

Because calculations are consistent in output and behavior, they are very easy to test. 

How harder would it be to unit test the `input` calls above vs the functions below?

```py
def add_two(num1: int, num2: int) -> int:
	return num1 + num2

def multiply_two(num1: int, num2: int) -> int:
	return num1 * num2
```
# Functional Core, Imperative Shell
Here is a great talk on functional core, imperative shell: <a href="https://www.youtube.com/watch?v=P1vES9AgfC4"> https://www.youtube.com/watch?v=P1vES9AgfC4 </a> 

The FP perspective accepts that we **need** side effects to produce useful software, but it strives to create more Calculations than Actions.

In this context, we can have an imperative shell (the function main responsible for handling user input) that orchestrates the user's calculations using a functional core (our calculate functions):

```py
# add_two is a calculation, a pure function
def add_two(num1: int, num2: int) -> int:
	return num1 + num2
	
# multiply_two is a calculation, a pure function
def multiply_two(num1: int, num2: int) -> int:
	return num1 * num2

# main is an action because it handles external state from user input
def main() -> None:
	num1 = int(input("Enter the first number: "))
	num2 = int(input("Enter the second number: "))
	
    print(f"The sum of {num1} + {num2} is {add_two(num1, num2)}")
    print(f"The product of {num1} * {num2} is {multiply_two(num1, num2)}")
		
```

# Wrapping It Up
By promoting the use of Calculations for core logic and pushing side effects to Actions, your codebase becomes easier to test, reason about, and debug. This approach reduces the surface area for inconsistent behavior.

Embracing these concepts encourages a mindset shift toward building robust and scalable applications that remain coherent and manageable as they evolve over time.

> Above all, as you work, ask yourself: "What can I do to make this file contain more calculations and fewer actions?"
