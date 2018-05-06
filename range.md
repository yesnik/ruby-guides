# Range

```ruby
(1..3).each { |n| p n }
#=> 1 2 3

('a'..'c').map(&:to_s) #=> ['a', 'b', 'c']
```

## One less syntax

```ruby
(1...3).map(&:to_i) #=> [1, 2]
(1...3).each { |n| p n } #=> 1 2
```

## Range to array

We use splat operator:

```ruby
a = *(1..3)
a #=> [1, 2, 3]
```

Range to array of strings:

```ruby
(1..3).map(:to_s) #=> ArgumentError: wrong number of arguments (given 1, expected 0)
(1..3).map(&:to_s) #=> ['1', '2', '3']
```
