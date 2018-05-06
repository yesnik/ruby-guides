# Loop

```ruby
i = 0
loop do
  i += 1

  next if i == 2
  break if i >=4
  
  puts i
end
#=> 1 3

3.times { p 'x' }
#=> x x x

for i in [1, 2, 3]
  p i
end
#=> 1 2 3

[1, 2, 3].each do |i|
  p i
end
#=> 1 2 3
```
