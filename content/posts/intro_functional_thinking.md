---
title: "An Introduction to Functional Thinking: Actions, Calculations, and Data"
date: 2023-08-12T17:22:17-06:00
draft: false
---

# Background

## I'm Learning Functional Programming

Lately, I've started to dip my toes into the fountain of functional programming, and it has been really interesting and awesome!

It feels like a breath of fresh air and more natural compared to typical OOP, and I want to share what I've learned with you!

## Grokking Simplicity
To fill in my knowledge gaps on functional programming, I'm reading an [amazing book by Eric Normand called Grokking Simplicity](https://www.manning.com/books/grokking-simplicity). 

The book takes an industrious perspective on teaching functional programming with an emphasis on the book being "practical for working software engineers."

This post will recap the foundational building blocks of functional thinking that Normand describes in [Grokking Simplicity](https://www.manning.com/books/grokking-simplicity): *actions*, *calculations*, and *data*.

# Actions, Calculations, and Data

## Data

Data is "recorded facts about events". For example, it could be a user's provided email address or even how many frozen pizzas are in your freezer. 

Data should feel familiar because you interact with data all the time: __data is any variable in your code.__


{{< highlight python >}}
user_email_addresses = ["test@gmail.com", "fp_fan@outlook.com"]
# user_email_addresses is a variable, so it is data

def get_first_email(user_email_addresses: List[str]) -> str:
    first_email_address = user_email_addresses[0]
    # first_email_address is a variable, so it is data
    return first_email_address
{{< / highlight >}}

## Calculations 

Simply put, a calculation is **any function that given the same inputs will ALWAYS return the same output**.

In the example below, the function `get_first_email` is a calculation: regardless of what order of email addresses we append to the lsit, the function will *always* return the first email.

{{< highlight python >}}
user_email_addresses = ["test@gmail.com", "fp_fan@outlook.com"]

# this function has the same output given any input,
# so it is a calculation
def get_first_email(user_email_addresses: List[str]) -> str:
    first_email_address = user_email_addresses[0]
    return first_email_address
{{< / highlight >}}

## Actions

Actions can be one of two things:
1. Functions whose code depends on when or how they are called
2. Functions that contain side-effects, called [impure functions](https://www.freecodecamp.org/news/pure-function-vs-impure-function/)

For example, a function that reads from a database is an action. 

Think about it: depending on when the function reads from the database, the output could be different. If a user registered their email overnight, the output of the function would change.

{{< highlight python >}}
# pretend I have a function that establishes and handles 
# a DB connection

def get_registered_users() -> List[str]:
    # this function's output is not guaranteed
    # to be the same each day
    db_conn = get_db_connection()
    registered_users = db_conn.execute("SELECT email from users")
    return registered_users 
{{< / highlight >}}

# Why is Functional Thinking Useful?
How is learning about actions, calculations, and data useful? What's the point of functional thinking?

## Functional Thinking Isolates Actions 
**The goal of designing software in a functional manner is to isolate actions from calculations and data**. 

## Functional Thinking Makes Modifying Code Easy
Because the behavior of calculations and data is consistent, modifying and testing that code is simple.

Modifying and testing actions, though? Sometimes not too bad, but other times a nightmare :). 

## Functional Thinking Helps Manage Complexity

Applying functional thinking to software design is a way of [managing complexity](https://youtu.be/X2FYGgOH1sQ) which if managed wrong can create a maintenance nightmare of a codebase.
