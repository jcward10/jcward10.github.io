---
title: Round 1 Day 3
published: true
description: Finished https://elixirschool.com/en/lessons/basics
tags: 100daysofcode, elixir
cover_image: https://cdn.hashnode.com/res/hashnode/image/upload/v1613355044450/ggGLGvRqH.png?auto=compress
datePosted: '2021-02-14'
devToId: 603919
---

# Keep Moving Forward

Three days ago I started the #100DaysOfCode Challenge, and today I finished elixirschool.com basics for elixir. This feels great because I started those basics over a year ago and stopped.

Quick TL;DR; for today.

## Testing

- ExUnit is built in to elixir, so I'll prolly be using that for my testing framework.
- Test Mocks. The elixir community has an interesting take on mocking. The whole idea revolves around using "mocks" as a noun and not a verb. The concept is to keep parts of the code isolated and easier to test.
- If you want to read more, check out the original blog post (here)[http://blog.plataformatec.com.br/2015/10/mocks-and-explicit-contracts/]

## Comprehensions

- Generators:
  - are neato
  - multiple generators can be used at a time
- Filters can be added to generators as you iterate over an Enum
  - Ex. `is_even` => `for x <- 1..10, is_even(x), do: x`

## Strings

- Prolly never gonna need to use Graphemes or Codepoints

## Time

- `Time.utc_now` returns a "Time Struct" `~T[19:39:31.056226]`
- `Date.utc_today` returns a "Date Struct" `~D[2021-02-15]`
- `NaiveDateTime` feels .... useless. Its Date + Time with no timezone /shrug
- I'll prolly most likly use `DateTime` instead.

## Custom Mix Tasks

- Are located in `lib/mix/tasks/*.ex`
- When creating a Task you have to use the macro `use Mix.Task`
- Mix tasks do not autoload dependencies

## iEX Helpers (cool things about the REPL)

- Tab autocomplete
- `.iex.exs` is a local file to configure the repl
- `h Module or Function` renders the docs
- `r Module` will recompile the module

Tada. Day 3. Fin.
If you want to reach out, you can find me on (The Tweeters)[https://twitter.com/basicbrogrammer]
