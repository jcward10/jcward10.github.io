---
title: Round 1 Day 1
published: true
description: Back to school. Back to school. Learning Elixir.
tags: 100daysofcode, elixir
cover_image: https://cdn.hashnode.com/res/hashnode/image/upload/v1613181428296/AE6zOKS_p.jpeg?auto=compress
datePosted: '2021-02-12'
devToId: 602094
---

## TIL
Currently during the 100DaysOfCode Challenge, I'm learning elixir. I'll be documenting everything I learn or build as I go. I'm starting off with learning the elixir basics at [ElixirSchool](http://elixirschool.com).
 
## Basics
- Everything is truthy except for `false` and `nil`
- Booleans are atoms (:true, :false)
- Modules are atoms 
- You can access erlang libraries using atoms
- Aka atoms are important :) 
- Strings use double quotes
- No modulo operator, but there are the `div` and `rem` functions.
- Each basic type has a rank :/     
    - number < atom < reference < function < port < pid < tuple < map < list < bistring
    - I'll never memorize this lawl 
- String interpolation "hello #{world}" or "hello " <> "world" ... Pretty Basic.

## Collections
- Lists
    - Mixed types
    - Not unique
    - Linked List → faster to prepend than append
        > In computer science, a linked list is a linear collection of data elements whose order is not given by their physical placement in memory. Instead, each element points to the next. It is a data structure consisting of a collection of nodes which together represent a sequence.
    - List concat `++/2` - cool
    - List sub `--/2` - cool cool
    - Head/Tail - cool cool cool
        - By pattern matching `[head | tail] = list` #=> head is the first element. tail will be all the others
        - `hd list` #=> returns the first element
        - `tl list` #=> returns all elements after the first
- Tuples
    - Stored with a set amount of memory
    - Access = fast; modification = slow
    - Common for return info from function
- Keyword lists
    - commonly used to pass options to functions
- Maps
    - The Go-to key-value store
    - Similar to ruby's hash
    - Updating syntax is cool?
    - useMap.put/3 to create a new key

## Enum
- Not much to go over here if you are familiar with any other languages Enum methods

## Pattern Matching
- The Holy Grail of Elixir
- If you ask any dev what they love about elixir chances are they will mention pattern matching
- Curveball `=` is actually a match operator similar to the equals sign in algebra :exploading-head:
- [1| tail] = list #=> tail has not been matched so it binds to the last elements of the list

## Control Structures

- `if` and `unless` => straight forward
- `case/2`
    - Similar to `switch`(in javascript) or `case` (ruby) except it uses pattern matching
    - `_` is a catch all like `default` in JavaScript
- `Cond`
    - Used for matching multiple cases similar to an `else if`
- `With`
    - This one is bit trickier coming from other languages
    - Can be used to replace multiple `case` statements