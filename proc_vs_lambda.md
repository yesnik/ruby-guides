# Proc vs Lambda

## return word

### Proc

When you call your proc, it will not only jump out of it, but will also return from the enclosing method:

```ruby
def hello
  p 'before'
  proc_test = proc do
    p 'inside'
    return
  end
  proc_test.call
  p 'after'
end

hello
#=> before
#=> inside
```

### Lambda

The `return` within the lambda only dumps us out of the lambda itself and the enclosing method continues executing.

```ruby
def hello
  p 'before'
  lambda_test = lambda do
    p 'inside'
    return
  end
  lambda_test.call
  p 'after'
end

hello
#=> before
#=> inside
#=> after
```

## Number of arguments check

### Proc

`proc` doesn't check the number of arguments:

```ruby
add_proc = proc { |a, b| a + b }
add_proc.call(2, 3) #=> 5
add_proc.call(2, 3, 4) #=> 5
```


### Lambda

`lambda` checks the number of arguments:

```ruby
add_lambda = ->(a, b) { a + b }
add_lambda.call(2, 3) #=> 5
add_lambda.call(2, 3, 4) #=> ArgumentError: wrong number of arguments (given 3, expected 2)
```
