# Control statements

Every statement in Ruby returns a value.

```ruby
value = if true
          :true_value
        else
          :false_value
        end

p value #=> :true_value

result = 1
unless false  # same as saying 'if !false', which evaluates as 'if true'
  result = 2
end

result #=> 2
```

### break statement returns value

```ruby
i = 1
x = while true
  p i
  i += 1
  break 'stop' if i >= 2
end
#=> 1
x #=> 'stop'
```

### next statement

```ruby
i = 1
while i <= 3
  next if i == 2
  p i
  i += 1
end
#=> 1
```
