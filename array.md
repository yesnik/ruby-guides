# Array

```ruby
a = Array.new #=> []
a.class #=> Array
a.size #=> 0

a = [1, 2, 3, 4]
a.first #=> 1
a.last #=> 4
a[-1] #=> 4
a[-2] #=> 3

a[0, 2] #=> [1, 2]
a[4, 0] #=> []
a[5, 1] #=> nil

a[2..-1] #=> [3, 4]
a[0..2] #=> [1, 2, 3]
a[0...2] #=> [1, 2]
```

## .array_with_index

```ruby
[11, 22].each_with_index { |v, i| p "#{i} -- #{v}" }
#=> 
"0 -- 11"
"1 -- 22"
```

We can create custom method .map_with_index for Array:

```ruby
class Array
  def map_with_index(&block)
    self.each_with_index.map(&block)
  end
end

[10, 20].map_with_index { |v, i| v + i }
```

## .map (or .collect)

Method `collect` is alias of method `map`.

```ruby
['a', 'b', 'c'].map { |letter| letter.capitalize }
#=> ["A", "B", "C"]

['a', 'b', 'c'].map(&:capitalize)
#=> ["A", "B", "C"]

[1, 2, 3].map { |x| x*2 }
#=> [2, 4, 6]

(1..3).map {}
#=> [nil, nil, nil]

(1..3).map { |x| x + 5}
#=> [6, 7, 8]

a = [1, 2]
a.map { |x| x + 1 } #=> [2, 3]
a #=> [1, 2]
```

## .map!

This method modifies original array:

```ruby
a = [1, 2]
a.map! { |x| x + 1 } #=> [2, 3]
a #=> [2, 3]
```

## .product

```ruby
[1, 2].product([11, 22])
#=> [[1, 11], [1, 22], [2, 11], [2, 22]]

[1, 2].product([11])
#=> [[1, 11], [2, 11]]
```

## .select (or .find_all)

```ruby
[1, 2, 3, 4].select { |x| x % 2 == 0 }
#=> [2]
```

## .reject

```ruby
a = [[1, 2], [2, 2], [2, 3]]
a.reject { |x| x.first == x.last }
#=> [[1, 2], [2, 3]]

b = [1, 2, 3, 4, 5]
b.reject(&:even?)
#=> [1, 3, 5] 
```

## .transpose

```ruby
a = [
  [1, 2],
  [2, 2], 
  [2, 3]
]
a.transpose
# [
# [1, 2, 2], 
# [2, 2, 3]
# ]
```

## .find (or .detect)

These methods locate the first element matching a criteria

```ruby
[1, 4, 5].find { |x| x >= 4 }
#=> 4

['nik', 'jenny', 'kenny'].detect { |x| x.size > 3 }
#=> 'jenny'
```

## .inject

```ruby
[1, 2, 3].inject(0) { |sum, item| sum + item }
#=> 6

[1, 2, 3].inject(:+) #=> 6

menu = {juce: 50, carbonara: 200}
order = {juce: 2, carbonara: 1}
order.keys.inject(0) do |order_cost, key|
  order_cost + order[key] * menu[key]
end
#=> 300
```

### Calculate factorial

```ruby
(1..3).inject(:*) #=> 6
```

### Calculate number of word occurences

```ruby
str = 'aaa, bbb; Aaa'
str.scan(/\w+/).inject(Hash.new(0)) do |build, word|
  build[word.downcase] += 1
  build
end
#=> {"aaa"=>2, "bbb"=>1}
```

## .sum

```ruby
[1, 2, 3].sum #=> 6
['1', 2].sum #=> TypeError: String can't be coerced into Integer
```

## .sample

```ruby
[1, 4, 2].sample #=> 4
[1, 4, 2].sample #=> 1
```

## .select

```ruby
[1, 2, 3].select { |n| n.odd? } #=> [1, 3]
[1, 2, 3].select(&:odd?) #=> [1, 3]
```

## .sort

```ruby
['a', 'c', 'bb'].sort
#=> ['a', 'bb', 'c']

['ccc', 'a', 'bb'].sort { |a, b| a.length <=> b.length }
#=> ['a', 'bb', 'ccc']
```

## .sort_by

```ruby
'aaaa b cc'.split.sort_by(&:length) #=> ['b', 'cc', 'aaaa']
```

## .all?

```ruby
[4, 5].all? { |x| x > 3 } #=> true
```

## .any?

```ruby
[5, -3].any? { |x| x < 0 } #=> true
```

## .none?

```ruby
[2, 3].none? { |x| x < 0 } #=> true
```

## Modify arrays

```ruby
a = [1, 2]
a.push 3 #=> [1, 2, 3]
a << 'x' #=> [1, 2, 3, 'x']

a.pop #=> 3
a #=> [1, 2]

b = [1, 2]
b.unshift :a #=> [:a, 1, 2]
b.shift #=> :a
b #=> [1, 2]

arr = ['a', 'b', 'c']
arr.shift(2) #=> ['a', 'b']
arr #=> ['c']

arr = [1, 2]
arr.map { |x| x**2 } #=> [1, 4]
arr #=> [1, 2]

arr.map! { |x| x**2 } #=> [1, 4]
arr #=> [1, 4]

arr = ['a', 'b', 'a']
arr.delete('a') #=> 'a'
arr.delete('zzz') #=> nil
arr #=> ['b']

arr = [1, 2, 3, 4, 5]
arr.delete_if { |x| x < 4 } #=> [4, 5]
arr #=> [4, 5]

a = [1, 2, 3]
b = [3, 4]
a - b #=> [1, 2]
a + b #=> [1, 2, 3, 3, 4]
a | b #=> [1, 2, 3, 4]
a & b #=> [3]
```

### Union operator

Pipe character | is the Union operator. 
It joins 2 arrays and returns the result with *duplicates removed*.

```ruby
a = [1, 2]
b = [2, 3]
a | b #=> [1, 2, 3]
```

### Intersection operator

```ruby
a = [1, 2]
b = [2, 2, 3]
a & b #=> [2]
```

All the elements in the result of intersection will be unique.

### Difference between two arrays

```ruby
[1,2,3, 1,2,3] - [1]
#=> [2,3,2,3]
```

### Array methods

```ruby
['a', 'b', 'c', 'b'].index('b') #=> 1

[11, 11, 30, 50].index { |x| x > 15 } #=> 2

arr = ['a', 'b']
arr.[](1) #=> 'b'

[] == Array.new #=> true
['a', 'b', 'c'][-2] #=> 'b'

arr = ['a', 'b', 'c']
arr.first == arr[0] #=> true
arr.last == arr[-1] #=> true

arr.slice(1) #=> 'b'
arr.slice(1..2) #=> ["b", "c"]

a = [11, 22]
a.size #=> 2
a.count #=> 2
a.length #=> 2
a.to_s #=> '[11, 22]'
a.inspect #=> '[11, 22]'

[1, 2, 1, 1].count(1) #=> 3
[1, 2, 3].count { |x| x.odd? } #=> 2

[1, 2, 3].shuffle #=> [2, 1, 3]
[1, 2, 3].shuffle #=> [1, 3, 2]
[1, 2, 3].shuffle! #=> [3, 2, 1]

['a', 'c'].join(', ') #=> 'a, c'

[].empty? #=> true
[1, 2, 3, 3].count(3) #=> 2
[2, 1, 2].uniq #=> [2, 1]

[1, [2, [3, 4]], 5].flatten #=> [1, 2, 3, 4, 5]
[1, [2, [3, 4]], 5].flatten(1) #=> [1, 2, [3, 4], 5]

[1, nil, 2, nil].compact #=> [1, 2]


[1, 2, 3].zip([11, 22, 33]) #=> [[1, 11], [2, 22], [3, 33]]
[1, 2].zip([11, 22, 33]) #=> [[1, 11], [2, 22]]
[1, 2, 3].zip([11, 22]) #=> [[1, 11], [2, 22], [3, nil]]
```

## Comparison

```ruby
[1, 2] == [2, 1] #=> false
```

## Assignment

```ruby
a, b = ['Hello', 'world']
a #=> 'Hello'
b #=> 'world'

a, = [11, 22]
a #=> 11

a, b = [11]
a #=> 11
b #=> nil
```

### Parallel assignment in iterator

```ruby
arr = [[1, 2 ,3], [11, 22, 33]]
arr.each do |item|
  a, b = item
  p "#{a} -- #{b}"
end
#=> 
"1 -- 2"
"11 -- 22"
```

It's the same:

```ruby
arr = [[1, 2 ,3], [11, 22, 33]]
arr.each do |a, b|
  p "#{a} -- #{b}"
end
#=> 
"1 -- 2"
"11 -- 22"
```

### Destructuring using splat operator

```ruby
a, *b = [1, 2, 3]
a #=> 1
b => [2, 3]

*a, b = [1, 2, 3]
a #=> [1, 2]
b => 3

first, *middle, last = [1, 2, 3, 4]
first #=> 1
middle #=> [2, 3]
last #=> 4

[[1, 2, 3, 4], [11, 22]].each do |a, *b|
  p "#{a} -- #{b}"
end
#=>
"1 -- [2, 3, 4]"
"11 -- [22]"
```