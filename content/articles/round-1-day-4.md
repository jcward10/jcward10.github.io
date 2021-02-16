---
title: It's time for war. Code. War
published: true
description: Day 4 of my first round of 100DaysOfCode. Time for practice.
tags: 100daysofcode, elixir
cover_image: https://cdn.hashnode.com/res/hashnode/image/upload/v1613441788636/o9mexGvaa.jpeg?auto=compress
datePosted: '2021-02-15'
devToId: 605183
---

# Round 1. Day 4

Now equiped with the knowledge of elixir basics from [ElixirSchool](https://elixirschool.com/en/lessons/basics/basics/), I figure it is time I start practicing, and building my first elixir app. I started of today's practice with a code challenge from [CodeWars](https://www.codewars.com/kata/54edbc7200b811e956000556/train/elixir). The Instructions were as follows:

<br>

> Consider an array/list of sheep where some sheep may be missing from their place. We need a function that counts the number of sheep present in the array (true means present).
> For example,

```elixir
[true,  true,  true,  false,
  true,  true,  true,  true ,
  true,  false, true,  false,
  true,  false, false, true ,
  true,  true,  true,  true ,
  false, false, true,  true]
```

> The correct answer would be `17`.

<br>
The boilerplate code for this challenge was:

```elixir
defmodule Shepherd do
  def count_sheeps(sheeps) do
    # TODO: for Elixir only true/false values can be presented the in sheeps list
  end
end
```

This challenge didn't seem too hard and it also felt a little familiar. I quickly went through my elixir school notes, and found a recursive counting function that would be a good template for solving this challenge. In the elixir school [functions](https://elixirschool.com/en/lessons/basics/functions/) there was a function that counted the length of an array.

```elixir
defmodule Length do
  def of([]), do: 0
  def of([_ | tail]), do: 1 + of(tail)
end
```

If we combine this logic with a little bit of pattern matching it should do the trick. And it did!

<br>

# Spoiler Alert

WARNING: if you would like to try to solve the same codewar challenge, don't scroll any further.

<br>

## Solution

```elixir
defmodule Shepherd do
  # This form of the function was taken directly from the example at elixir
  # school.
  def count_sheeps([]), do: 0
  # Next, we write a form of the function that pattern matches the first
  # element to see if it is "true". If it is, we add 1.
  def count_sheeps([true | tail]), do: 1 + count_sheeps(tail)
  # All other elements will count as 0.
  def count_sheeps([_ | tail]), do: 0 + count_sheeps(tail)
end
```

## My First Elixir App

When I first learned ruby over 6 years ago, my first ruby app was a cli blackjack game. If it was good enough to start learning ruby, it should be good practice for elixir as well.

The app is still a work in progress, but here is what I've done so far. I started by creating a new `mix` project which was probably a bit over kill. /shrug The first thing black jack needs is cards. So, I started by creating a Card Struct.
`lib/card.ex`

```elixir
defmodule Card do
  defstruct suit: "", value: ""
end
```

And each card will be part of a Deck, so let's go ahead and create a Deck struct too. It was at this point, I felt like my Object Oriented Programming mindset was having an affect on the way I was writing elixir, but I kept going.

`lib/deck.ex`

```elixir
defmodule Deck do
  # I used Module constants to specify the different suits and the card values.
  @suits [:heart, :diamond, :club, :spade]
  @values ["2", "3", "4", "5", "6", "7", "8", "9", "10", "J", "Q", "K", "A"]

  # To generate the deck, we flat_map the @suits List using the shorthand
  # function syntax to pass the suit into the `Deck.build_suit/1` function.
  # Then, the List returned from the flat_map is shuffled using `Enum.shuffle`
  def generate do
    @suits
    |> Enum.flat_map(&(build_suit(&1)))
    |> Enum.shuffle()
  end

  # the `Deck.build_suit/1` function maps over the @values List and creates
  # %Card structs using the given suit and card value.
  defp build_suit(suit) do
    @values |> Enum.map(fn (value) -> %Card{ suit: suit, value: value } end)
  end
end
```

Not a bad start. However, I spun my wheels trying to figure out how I would store the `state` of the game, and handle the initial deal. I was finally able to scrap together something that worked, but it's definitely subject to change.

```elixir
defmodule Game do
  # I figured I could use a %Game struct to store the players and the current game's cards.
  # This will allow me to keep up with what a player's hand is as well as what
  # cards are still in play.
  defstruct players: [%Player{name: "player_one"}, %Player{name: "dealer"}], cards: Deck.generate()

  # With recursion on my mind from the earlier CodeWar Challenge, I created a
  # recursive deal function that would deal cards from the deck until each play
  # had their initial 2 cards.
  def deal(game) do
    cond do
      # The first conditional checks to see if all players have 2 cards
      Game.cards_dealt(game.players) -> game
      # The catch all conditional will deal a card to the first player in the
      # %Game struct then call deal again.
      true -> game |> deal_card(hd(game.players)) |> deal
    end
  end

  # deal_card/2 handles a couple of things.
  # 1. We pattern match the first `card` from the %Game.cards and the rest of
  # the cards. This essentially pulls a card from the deck.
  # 2. We update the %Game.players by giving the first `card` to the given player.
  # 3. Finally, we update the %Game struct cards to the `tail` from the original game argument.
  def deal_card(%{cards: [card | cards]} = game, player) do
    game
    |> update_players(Player.add_cards(player, card))
    |> case do
      game -> %{ game | cards: cards}
    end
  end

  def cards_dealt(players) do
    Enum.reduce(players, 0, fn player, acc -> acc + length(player.hand) end) == length(players) * 2
  end

  defp update_players(game, player) do
    players = game.players |> Enum.filter(fn (p) -> p.name != player.name end)

    # ++/2 is slower, but it makes it easier to deal the cards
    %{ game | players: players ++ [player]}
  end
end
```

I'd love to hear any thoughts about how to handle this better. You can drop a comment down below, or open up a pull request at https://github.com/basicBrogrammer/blackjack_cli. Thanks for tuning in and I'll see ya tomorrow.

<br>

## Follow Me @

[Twitter](https://twitter.com/basicbrogrammer) | [Instagram](https://instagram.com/basicbrogrammer)
