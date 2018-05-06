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
