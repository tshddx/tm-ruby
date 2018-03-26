# tm-ruby

## A Turing Machine in Ruby for Fun

A TuringMachine is initialized with an initial State and an initial Tape.

A Tape has an array of Syms and an index of where the head is.

A Sym is either BLANK, ONE, or ZERO.

Each State has a name (for display purposes only) and three rules (one for each Sym). Each rule says what Sym the head should write, which direction (if any) the head should move, and finally which State to switch to.

## Example: Computer the input contains an odd or an even number of 1's

```ruby
require_relative './turing_machine'
require_relative './move'
require_relative './state'
require_relative './sym'

move_right_even = State.new('move-right-even')
move_right_odd = State.new('move-right-odd')
even_ones = State.halt_with_message("There is an even number of 1's.")
odd_ones = State.halt_with_message("There is an odd number of 1's.")

tm = TuringMachine.new(
  start_state: move_right_even,
  start_tape: Tape.from_string(
  "111"
))

move_right_even.on(
  Sym::ONE,
  write: Sym::ONE,
  move: Move::RIGHT,
  next_state: move_right_odd
)

move_right_even.on(
  Sym::BLANK,
  write: Sym::BLANK,
  move: Move::RIGHT,
  next_state: even_ones
)

move_right_odd.on(
  Sym::ONE,
  write: Sym::ONE,
  move: Move::RIGHT,
  next_state: move_right_even
)

move_right_odd.on(
  Sym::BLANK,
  write: Sym::BLANK,
  move: Move::RIGHT,
  next_state: odd_ones
)

until tm.halted?
  puts tm
  puts ""
  tm.next
end

puts "The Turing machine halted after #{tm.step + 1} steps!"
puts tm
puts tm.state.message
```