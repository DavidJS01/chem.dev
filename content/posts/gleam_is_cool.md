---
title: "Gleam: A Neat Type Safe FP Language!"
date: 2023-10-29T13:44:27-06:00
draft: false
tags: ["Functional Programming"]
---

# Discovering Gleam: Compile Time Type Safety!
I recently found this [Hackernews post](https://news.ycombinator.com/item?id=38031342) talking about [Gleam](https://gleam.run), a FP language built on the Beam. It seems like Gleam is trying to be the performant, type-safety choice for backend systems.


In the past, I dabbled with Elixir (also on the Beam), and I really liked it! Once I understood the basics
of Elixir, it felt like a very ergonomic and productive language.

## Types
**But there is one thing I didn't like about Elixir**: the [dynamic typing](https://elixir-lang.org/getting-started/typespecs-and-behaviours.html). 

This is where Gleam comes in: Gleam is a statically typed language that checks types at compile time.

The compiler includes very useful error messages. It sometimes feels like I have someone providing me guidance,
despite my silly mistakes.

![gleam_error](/gleam_error.png)

# Interesting Things About Gleam
This is from the perspective of a procedural and OO developer :D 

## Small and Concise
Gleam is a small and concise language. It felt like I was able to start developing a [project](https://github.com/DavidJS01/gleam_rfaker) after walking through the [Tour of Gleam](https://gleam.run/book/tour/)

## No If Statements, Only Cases
In Gleam, there are no if statements. You handle the if/else branches through a case statement. The nice
thing about this is that the compiler forces you to handle all branches.

In the code below, we simply take in a type of data the user wants generated as a string, and we return
the correct type of data. 

NOTE: I have a function that sanitizes the input to this code below :) 
{{< highlight gleam >}}
pub fn route_data_generator(data_type: String) -> String {
  case data_type {
    "address" -> addresses.get_fake_address()
    "name" -> names.get_fake_name("full")
    "ssn" -> ssn.get_fake_ssn()
  }
}

{{< / highlight >}}

## No While Loops, Just Recursion
Gleam does not have while loops. Instead, you simply use recursion until you meet a condition.

In the code below, for each number we add to the generated SSN, we check if the SSN length is 9.
If not, we simply recall `generate_fake_ssn` again until we get to a length of 9.

{{<highlight gleam >}}
fn build_ssn(ssn: String) -> String {
  let random_num: Int = placeholder.generate_random_int(0, 10)
  string.append(ssn, int.to_string(random_num))
}

fn generate_fake_ssn(ssn: String, counter: Int) -> String {
  // todo: use StringBuilder
  // starting with an empty string, "", add a random
  // num to the string until the length is 9 (valid ssn)

  let generated_ssn: String = build_ssn(ssn)
  case counter {
    9 -> ssn
    _ -> generate_fake_ssn(generated_ssn, counter + 1)
  }
}

/// Return a random SSN as a string
pub fn get_fake_ssn() -> String {
  generate_fake_ssn("", 0)
}
  {{< / highlight >}}

## First Class Documentation Generation from Comments
I am a SUCKER for good documentation. It makes everyone's lives so much easier. In Gleam,
Gleam comes with tooling `gleam docs build` that will automatically build a documentation
web page for you using special comments you write in code.

For example, writing
{{<highlight gleam >}}
/// Return a random SSN as a string
pub fn get_fake_ssn() -> String {
  generate_fake_ssn("", 0)
}
  {{< / highlight >}}

Results in 

![gleam documentation is great](/gleam_docs.png)
