# Block

Block is a piece of code that can't be stored in a variable and isn't an object.
It's, as a consequence, significantly faster than a lambda, but not as versatile and
also one of the rare instances where Ruby's "everything is an object" rule is broken.
A block is like a method, but one that isn't associated with any object.

### Passing a block to a method that takes no parameter

```ruby
def greet
  puts 'Start'
  yield
  puts 'End'
end

greet { puts 'Hi' }
#=> Start Hi End
```

In this example, a block is passed to the `greet` method. To invoke this block inside the method, we use a keyword `yield`.
Calling `yield` will execute the code within the block that is provided to the method.


### Passing a block to a method that takes one or more parameters

Example that uses regular block syntax:

```ruby
def calculate(a, b, action)
  action.call(a, b)
end

calculate(2, 3, lambda { |a, b| a + b }) #=> 5
calculate(2, 3, lambda { |a, b| a - b }) #=> -1
```

Let's rewrite it using yield:

```ruby
def calculate(a, b)
  yield(a, b)
end

calculate(2, 3) { |a, b| a + b } #=> 5
calculate(2, 3) { |a, b| a - b } #=> -1
```

## yield is not a method, it's keyword

This `yield` statement allows method to invoke an associated block one or more times.

```ruby
def hi
  p yield
  p method(:hi)
  p method(:yield)
end

hi { 'hi' }
#=> 'hi'
#<Method: Object#hi>
NameError: undefined method 'yield' for class...

def hey
  yield + '!!!'
end

hey { 'Hello' } #=> 'Hello!!!'

def block_method
  yield
end

block_method { 1 + 2 }
#=> 3

block_method do
  1 + 2
end
#=> 3
```

We can pass values to the block by giving parameters to `yield`.

```ruby
def block_method_args
  yield('Kenny')
end

block_method_args { 'hi' }
#=> 'hi'

block_metod_args { |x| x }
#=> 'Kenny'

def many_yields
  yield(:a)
  yield(:b)
end

result = []
many_yields { |x| result << x }
#=> [:a, :b]
result
#=> [:a, :b]
```

### Use yield in each

```ruby
class OddNumbers
  NUMS = [1, 3, 5]
  
  def self.each
    NUMS.each { |x| yield x }
  end
end

a = OddNumbers.each { |x| p x }
#=> 1 3 5
```

### .block_given?

```
def yield_tester
  if block_given?
    yield
  else
    :no_given
  end
end

yield_tester { :hi }
#=> :hi

yield_tester
#=> :no_block
```

### Block can affect variables in the code where they are created

```ruby
def block_method
  yield
end

a = 1
block_method { a = 2 }
a #=> 2
```

### Blocks can be assigned to variables and called explicitly

```ruby
add_one = lambda { |x| x + 1 }
add_one.call(5) #=> 6
add_one[5] #=> 6
```

### Methods can take an explicit block argument

The `&` prefix operator allows a method to capture a passed block as a named parameter.

```ruby
def method_with_explicit_block(&block)
  block.call(10)
end

method_with_explicit_block { |x| x*2 }
#=> 20

add_one = lambda { |x| x + 1 }

method_with_explicit_block(&add_one)
#=> 11

def greet(&block)
  2.times(&block)
end

greet { 'hi' }
#=> 'hi' 'hi'

def greet(&block)
  2.times { block.call }
end

greet { p 'hi' }
```

### Convert implicit to explicit

Here '&block' is an explicit (named) parameter:

```ruby
def calculate(a, b, &block)
  block.call(a, b)
end
```

Note:

- the block should be the last parameter passed to a method.
- placking and ampersand (&) before the name of the last variable triggers the conversion.

This is an implicit block, it's nameless and is not passed as an explicit parameter:

```ruby
calculate(2, 3) { |a, b| a + b }

calculate(2, 3, lambda { |a, b| a + b }) #=> ArgumentError: wrong number of arguments (given 3, expected 2)
calculate(2, 3, &lambda { |a, b| a + b }) #=> 5

add = lambda { |a, b| a + b }
calculate(2, 3, &add) #=> 5
```

### Convert explicit to implicit

Here 'yield' calls an implicit (unnamed) block:

```ruby
def calculate(a, b)
  yield(a, b)
end
```

Here `&add` is an explicit (named) block - but `yield` can still call it:

```ruby
add = lambda { |a, b| a + b }
calculate(2, 3, &add)
```

### Pass to .select explicit block

```ruby
positive = lambda { |x| x > 0 }
[-2, 4, 5].select(positive) #=> ArgumentError: wrong number of arguments (given 1, expected 0)
[-2, 4, 5].select(&positive) #=> [4, 5]
```

## Proc, lambda, proc

```ruby
p lambda {} 
#=> #<Proc:0x000234234@(irb):135 (lambda)>

p Proc.new {}
#=> #<Proc:0x000234234@(irb):136>

p proc {}
#=> #<Proc:0x000234234@(irb):137>
```

All approaches produce an instance of a Proc, though the one created using lambda is clearly distinguished with the word '(lambda)' - an unusual deviation from the norm.

### proc is identical to Proc.new

`Kernel#proc` factory method is identical to Proc.new.

The following lines produce identical results:

```ruby
add = Proc.new { |a, b| a + b }
add.call(2, 3) #=> 5

add = proc { |a, b| a + b }
add.call(2, 3) #=> 5
```

### return in lambda vs return in Proc.new

1. A block created with lambda behaves like a method when you use return and simply exits the block, 
handing control back to the calling method.

```ruby
def hi
  lambda { return 'from lambda' }.call
  'from method'
end
hi #=> 'from method'
```

2. A block created with `Proc.new` or `proc` behaves like it's a part of 
the calling method when 'return' is used within it,
and returns from both the block itself as well as the calling method.

```ruby
def hey
  Proc.new { return 'from block' }.call
  'from method' 
end
hey #=> 'from block'

def wow
  proc { return 'from block' }.call
  'from method' 
end
wow #=> 'from block'
```

But if we omit 'return' code works as expected:

```ruby
def hoho
  proc { 'from block' }.call
  'from method'
end
hoho #=> 'from method'
```

As a consequence, Proc.new is something that's hardly ever used to explicitly create blocks because
of this surprising return semantics. It's recommended that you avoid using this form unless absolutelly necessary.
