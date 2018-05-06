# Operators

## and

```ruby
true and false #=> false
false and true #=> false

a = true and false
a #=> true
```

It's because `and` operator has lower precidence than `&&`, `=` operators.
That's why the following records are equivalent:

```ruby
# 1 way
a = true and false
a #=> true

# 2 way
(a = true) && false
a #=> true
```

## or

These two records are the same:

```ruby
# Example 1
a = false or true
#=> true
a #=> false

# Example 2
(a = false) || true
#=> true
a #=> false
```
