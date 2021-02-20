---
title: '[WIP] Creating a Blackjack CLI game with Elixir'
published: true
description: On day 6 of my 100 days of code, I was able to make some head way with my CLI Blackjack game with elixir.
tags: 100daysofcode, webdev, elixir
cover_image:
datePosted: '2021-02-19'
---

## Day 6 of 100 Days of Code

Today I was able to work on my [Blackjack CLI](https://github.com/basicBrogrammer/blackjack_cli) written in elixir. It's starting to shape up, but it still feels a little off. Here's a quick demo:

![blackjack demo](https://cdn.hashnode.com/res/hashnode/image/upload/v1613759602886/OZjw9gwhJ.gif?auto=compress)

## Update to the user experience

First I took advantage of elixir's pattern matching in functions to create a set of functions to calculate the value for a given card.

```elixir
defmodule Card do
  ...
  def value(%{value: "J"}), do: 10
  def value(%{value: "Q"}), do: 10
  def value(%{value: "K"}), do: 10
  def value(%{value: "A"}), do: 11
  def value(%{value: value}), do: value
  ...
end
```

Next, I added a flash to displaying the cards by adding a white background and painting the card according to the suit.

```elixir
defmodule Card do
  ...
  def display(%{suit: suit, value: value}) do
    IO.ANSI.color_background(15) <> IO.ANSI.color(color_code(suit)) <> "#{suit}#{value}" <> IO.ANSI.reset()
  end

  defp color_code("♥"), do: 9
  defp color_code("♦"), do: 9
  defp color_code("♣"), do: 0
  defp color_code("♠"), do: 0
end
```

Another bit of flare I added was chaning out the `Deck @suits` constant with the unicode icons for each suit. It's the little things you know?

Now, let's take a look at the changes to the Game module.

```elixir
defmodule Game do
  # Added the "finished_players" attribute to the %Game struct. When a user
  # finishes their turn, they will be removed from the players list and placed
  # into the finished_players list.
  defstruct players: [%Player{name: "player_one"}, %Player{name: "dealer"}], cards: Deck.generate(), finished_players: []

  # After all users are removed from the players list, we can pattern match on
  # an empty players attr and finish the game.
  def play(%{players: []} = game) do
    game |> inspect_table()
  end

  # If there are still players, the play function logic will be a little
  # different, but with the pipe operator its pretty easy to follow.
  def play(game) do
    game
    |> deal()
    |> inspect_table()
    |> turn()
    |> play
  end

  # To handle a players turn, we just ask them if they want to Hit or Stay, and
  # build a new game struct accordingly.
  def turn(%{players: [current | players]} = game) do
    IO.puts "\n#{current.name} your move."
    case IO.gets("(H)it or (S)tay\n") |> String.downcase |> String.trim do
      "hit" -> deal_card(game, current)
      "h" -> deal_card(game, current)
      "stay" -> %{game | players: players, finished_players: [current | game.finished_players]}
      "s" -> %{game | players: players, finished_players: [current | game.finished_players]}
      _ -> game
    end
  end

  # Inspecting the table clears out the terminal, then displays info about for
  # the current state of the game
  defp inspect_table(%{players: players, finished_players: f_players} = game) do
    IO.write IO.ANSI.reset() <> IO.ANSI.clear() <> IO.ANSI.home()
    if length(players) == 0 do
      IO.puts "Game Over."
    end
    for player <- f_players ++ players do
      IO.puts "#{player.name}'s\n  hand: #{Player.display_hand(player)} |  #{player.total}"
    end
    game
  end
end
```

## Implement the Dealer logic

In regular Blackjack, the deal has to hit if they have less than 17. I decided to replace `%Player{name: "dealer"}` with a `%Dealer` struct. With that in place I can use pattern matching again for the `Game.turn/1` function which looks a little like this:

```elixir
  def turn(%{players: [%Dealer{} = dealer | players]} = game) do
    cond do
      dealer.total >= 17 -> %{game | players: players, finished_players: [dealer | game.finished_players]}
      true -> deal_card(game, dealer)
    end
    |> inspect_table()
    |> (fn (game) ->
      IO.gets("Press enter to continue.")
      game
    end).()
  end
```

## What's next?

If I want to finish Blackjack CLI, I would have to handle Aces. Aces can have a value of 1 or 11. This shouldn't be to hard, but might take some time. Other than that, It would be nice to choose how many decks could be played, and play to a certain percentage of the deck before regenerating the game.

I will prolly move on to the advance section of elixirschool.com, and come back to this repo later on.

## Follow Me @

[Twitter](https://twitter.com/basicbrogrammer) | [Instagram](https://instagram.com/basicbrogrammer)
