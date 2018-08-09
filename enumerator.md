# Enumerator

```ruby
Enumerator.ancestors
#=> [Enumerator, Enumerable, Object, Kernel, BasicObject]
```

'Enumerable' is Ruby's way of saying that we can get elements out of a collection, one at a time.
'Enumerable' is a module used as a mixin in the Array class. It provides a number of enumerators like
map, select, inject.

Ruby module Enumerable is included in classes Array, Hash.

```ruby
[1, 2].each.class #=> Enumerator

enumerator = [1, 2].each
enumerator.each { |a| p a + 1 }
#=> 2 3
```

Enumerator is an objectification of enumeration.
The point of these methods returning these enumerators is to allow us to chain operations indefinetely
and make more heavy-duty collections.

```ruby
block = proc { |value, index| value + index }
[10, 20].each_with_index.map(&block) #=> [10, 21]
```

## Make object a collection

We can make an object a collection by defining the each method and mixing-in the Enumerable module into the class.
All of the Enumerable methods rely on each wich should be implemented by the host class.

```ruby
class OddNumbers
  NUMS = [1, 3, 5]
  
  include Enumerable
  
  def each
    NUMS.each { |x| yield x }
  end
end

nums = OddNumbers.new

nums.each { |x| p x }
#=> 1 3 5

nums.select { |x| x > 2 }
#=> [2, 5]
```

## Lazy evaluation

Ruby 2.0 introduced a lazy enumeration feature. Lazy evaluation is an evaluation strategy that delays the assessment of an expression untill its value is needed. It increases performance by avoiding needless calculations, and it has the ability to create potentially infinite data structures.

### Print an array of the first N palindromic prime numbers

For example, the first 7 palindromic prime numbers are: `[2, 3, 5, 7, 11, 101, 131]`.

```ruby
require 'prime'
n = gets.to_i
p Prime.each.lazy.select { |x| x.to_s == x.to_s.reverse }.first(n)
```

## .each_cons

```ruby
(1..5).each_cons(3) { |x| p x }
#=> [1, 2, 3]
#=> [2, 3, 4]
#=> [3, 4, 5]

(1..3).each_cons(2) { |current, following| p "#{current} -> #{following}" }
#=> "1 -> 2"
#=> "2 -> 3"
```
