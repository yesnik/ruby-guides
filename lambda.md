# Lambdas

- Lambda is a piece of code that you can store in a variable
- Lambda is just a function without a name
- Lambdas in Ruby are objects of the class `Proc`
- Thle last expression of a lambda is its return value
- The `lambda` keyword is what is most commonly used to create a block in Ruby

## Two ways of defining lambda

### Lambda syntax for Ruby version <= 1.8

```ruby
# No input params
a = lambda { 5 }
a = lambda do
  5
end

# With input params
a = lambda { |x| x**2 }
a = lambda do |x|
  x**2
end
```

### Lambda syntax for Ruby version >= 1.9: "stabby lambda" syntax

```ruby
-> { ... }
-> do
  ..
end
```

```ruby
a = lambda { 'hi' }
a.call #=> 'hi'

b = lambda { |n| "hi, #{n}" }
b.call 'Kenny' #=> 'hi, Kenny'

add = lambda { |a, b| a + b }
add.call(2, 3) #=> 5

empty_block = lambda {}
empty_block.object_id #=>1232123112312
empty_block.class #=> Proc
empty_block.class.ancestors #=> [Proc, Object, Kernel, BasicObject]
```

## Call lambda

Lambdas can be called using both `.call()` and `.()`:

```ruby
area = ->(a, b) { a * b }

area.call(2, 3) #=> 6
area.(2, 3) #=> 6
```

## Lambda that takes explicit block

```ruby
filter = lambda { |arr, block| arr.select(&block) }
filter.call([1, 2]) #=> ArgumentError: wrong number of arguments
filter.call([1, 2, 3], lambda { |x| x > 2 }) #=> [3]
```

## Short syntax for lambda

```ruby
add = -> (a, b) { a + b }
add.call(2, 3) #=> 5
```

Here '->' is a literal form.

```ruby
a = -> { 'hello' }
a.class #=> Proc
a.call #=> 'hello'
```

## Threequals defined as an alias to .call

```ruby
odd = ->(a) { a % 2 != 0 }

p odd === 1 #=> true
p odd === 2 #=> false
```
