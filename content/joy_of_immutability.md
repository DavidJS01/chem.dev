+++
title = "The Joys of Immutability and Concurrency in Elixir"
date = 2024-08-17
draft = false
[extra]
summary = "immutable data structures + actor model = easy concurrency"

[taxonomies]

tags = ["development", "fp", "concurrency"]
+++ 

# Background
Lately, I have been programming a lot in [Elixir](https://elixir-lang.org/).

Elixir is a functional programming language with first class concurrency primitives and immutable data structures running on the [BEAM VM](https://www.erlang.org/blog/a-brief-beam-primer/).

## Immutability in Elixir
In Elixir, every data structure is immutable, so instead of modifying a variable directly, functions return a new memory location with a shallow copy reflecting your changes.

You can see an example below, from Saša Jurić's [Elixir in Action](https://www.manning.com/books/elixir-in-action):

![diagram showing shallow copy in Elixir](/tuple-shared-memory.png)

This approach is powerful: rather than tracking state changes over an object's lifetime, you can focus on composing functions that operate with straightforward inputs and outputs.

## Cognitive Benefits of Immutability
Immutability makes reasoning through state changes easier to manage. Each change in state becomes a distinct, independent value:

```elixir
email = "USER@example.com"

transformed_email = 
  email
  |> Email.change_domain("newdomain.com")
  # "USER@newdomain.com"
  |> Email.add_tag("newsletter") 
  # "USER+newsletter@newdomain.com"
  |> String.downcase() 
  # "user+newsletter@newdomain.com"
  
IO.inspect(email)
# Output: "USER@example.com"
IO.inspect(transformed_email)
# Output: "user+newsletter@newdomain.com"
```

Personally, I like this model of programming because changes to the data structure is very explicit.

### On Immutability and Debugging
I worked in a few codebases where one object's values mutated so many times throughout the object's lifecycle.

Debugging issues with objects like that was a challenge -- it felt like I was working with a black box as I had to constantly track the implicit state.

I appreciate how the immutable perspective makes each transformation clear, reducing the mental overhead of tracking changes in state.

### On Immutability and Testing
With immutable data structures, testing becomes simpler because we only need to verify that the functions produce the correct output for a given input.

```elixir
defmodule EmailTest do
  use ExUnit.Case

  test "changing domain" do
    email = "USER@example.com"
    transformed_email = Email.change_domain(email, "newdomain.com")
    assert transformed_email == "USER@newdomain.com"
  end

  test "adding tag" do
    email = "USER@example.com"
    transformed_email = Email.add_tag(email, "newsletter")
    assert transformed_email == "USER+newsletter@example.com"
  end

  test "multiple transformations" do
    email = "USER@example.com"
    transformed_email =
      email
      |> Email.change_domain("newdomain.com")
      |> Email.add_tag("newsletter")
      |> String.downcase()

    assert transformed_email == "user+newsletter@newdomain.com"
  end
end 
```

## Concurrency
### In Traditional Languages
A problem with programming concurrent applications in traditional, mutable languages is that data races can happen without explicit planning and oversight.

This is also known as the [shared state model of concurrency](https://wiki.c2.com/?SharedStateConcurrency), and it implements some challenges with concurrent read/writes to shared state, like a global variable.

In the C++ program below, we need to [add a mutex](https://en.cppreference.com/w/cpp/thread/mutex) to avoid a data race.

What could go wrong if we didn't have the mutex?
```cpp
#include <iostream>
#include <thread>
#include <mutex>

std::mutex mtx;
int counter = 0;

void increment() {
    for (int i = 0; i < 1000; i++) {
        std::lock_guard<std::mutex> lock(mtx);
        counter++;
    }
}

int main() {
    std::thread t1(increment);
    std::thread t2(increment);

    t1.join();
    t2.join();

    std::cout << counter << std::endl;
    return 0;
}
```
### In Erlang/Elixir
Erlang and Elixir take a different approach to concurrency: they adopt the [actor model](https://en.wikipedia.org/wiki/Actor_model). Each process is very lightweight and managed by the [BEAM VM](https://en.wikipedia.org/wiki/BEAM_(Erlang_virtual_machine)).

The actor model includes:
- Process isolation
  - Each process is completely isolated with its own memory and state (no memory sharing = good)
  - This means processes can't interfere with each other's state directly, avoiding data races
- Message passing
  - Processes communicate through asynchronous message passing
  - Processes can choose how to handle each message

This is starting to sound a [lot like the message-passing essence of Smalltalk](https://en.wikipedia.org/wiki/Smalltalk) :)

```elixir
defmodule Example do
  def listen do
    receive do
      {:ok, "hello"} -> IO.puts("World")
    end
    # Keep listening for messages
    listen() 
  end
end

# Spawn a new process that runs the
# `listen` function
iex> pid = spawn(Example, :listen, [])
#PID<0.108.0>

# Send a message to the spawned process
iex> send(pid, {:ok, "hello"})
World # <- given the hello message, output is 'World'

# The process will only handle messages
# matching the pattern {:ok, "hello"}
iex> send(pid, :ok)
:ok

```

By utilizing the actor model, Erlang and Elixir enable safe, concurrent programming **because there is no concept of sharing mutable state**. On the application level, it becomes a joy to write concurrent programs in Elixir without fear of data races :).

# Closing Notes
- Immutable data structures = good
  - Because making a change to something results in a new thing, you can model program as straightforward transformation pipelines
  - Easier to reason through than in messy code in most languages
- Developing correct concurrent software is really hard, especially when reading/writing to shared state
  - Elixir/Erlang's actor model implementation makes this a lot better!
- Shared, mutable state = evil and headaches

Joe Armstrong (Erlang creator) has so many amazing talks. Here are some of my favorites:
- [How and Why of Fitting Things Together](https://www.youtube.com/watch?v=ed7A7r6DBsM)
- [The Mess We're In](https://www.youtube.com/watch?v=lKXe3HUG2l4)
- [Forgotten ideas in CS](https://www.youtube.com/watch?v=-I_jE0l7sYQ)
- [How we Program Multicores](https://www.youtube.com/watch?v=bo5WL5IQAd0)

If you want to learn more about Elixir and Erlang, this is a great talk: [The Soul of Erlang & Elixir](https://www.youtube.com/watch?v=JvBT4XBdoUE)
