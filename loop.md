# Loop

## loop

```ruby
i = 0
loop do
  i += 1

  next if i == 2
  break if i >=4
  
  puts i
end
#=> 1 3
```

## .times

```ruby
3.times { p 'x' }
#=> x x x

3.times { |x| x }
#=> 3
```
If you want to use index of each iteration:

```ruby
3.times.map { |x| x**2 }
#=> [0, 2, 4]
```

## for

```ruby
for i in [1, 2, 3]
  p i
end
#=> 1 2 3
```

## .each

```ruby
[1, 2, 3].each do |i|
  p i
end
#=> 1 2 3
```
