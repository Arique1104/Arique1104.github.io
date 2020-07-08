# Refactoring Ruby
Lessons to land
1. Stop passing `counter` around
1. Refactor `print_turn_summary` (So it takes zero arguments instead of 3)
1. Getting rid of the seven line `if statement`
1. Passing an object into helper methods instead of results of method call.


Here's the beginning code:

```ruby
def start_game
    counter = 1
    until winning_condition(counter) do
      turn = Turn.new(@player1, @player2)
      p "TURN TYPE: #{turn.type}"
      winner = turn.winner
        if turn.type == :basic
          basic_turn(turn, winner, counter)
        elsif turn.type == :war
          war_turn(turn, winner, counter)
        else
          mad_turn(turn, counter)
        end
      print_turn_summary(turn, winner, counter)
      counter += 1
    end
    game_completion
  end

  def print_turn_summary(turn, winner, counter)
    case turn.type
    when :basic
      p "Turn #{counter}: #{winner.name} won 2 cards"
    when :war
      p "Turn #{counter}: #{winner.name} won 6 cards"
    when :mutually_assured_destruction
      p "Turn #{counter}: *mutally assured destruction* 6 cards removed from play"
    end
  end

  def winning_condition(counter)
    @player1.has_lost? || @player2.has_lost? || counter == 1_000_000
  end

  def basic_turn(turn, winner, counter)
    turn.pile_cards
    turn.award_spoils(winner)

  end

  def war_turn(turn, winner, counter)
    turn.pile_cards
    turn.award_spoils(winner)

  end

  def mad_turn(turn, counter)
    turn.pile_cards

  end
```

Here's the end code:
```ruby
def start_game
  until winning_condition do
    @turn = Turn.new(@player1, @player2)
    turn.pile_cards
    turn.award_spoils(turn.winner)
    print_turn_summary(turn)
    @counter += 1
  end
  game_completion
end

ef print_turn_summary(turn)
    case turn.type
    when :basic
      p "Turn #{counter}: #{turn.winner.name} won 2 cards"
    when :war
      p "Turn #{counter}: #{turn.winner.name} won 6 cards"
    when :mutually_assured_destruction
      p "Turn #{counter}: *mutally assured destruction* 6 cards removed from play"
    end
  end

  def winning_condition
    @player1.has_lost? || @player2.has_lost? || counter == 1_000_000
  end

  def game_completion
    if @player1.has_lost?
      p "*~*~*~* Congrats #{@player2.name}! YOU WON! *~*~*~*"
    elsif @player2.has_lost?
      p "*~*~*~* Congrats #{@player1.name}! YOU WON! *~*~*~*"
    else
      p "It's a draw, Captain!"
    end
  end
```
