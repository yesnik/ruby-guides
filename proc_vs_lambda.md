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

## Closure behaviour

Blocks, Procs and Lambdas are *closures* in Ruby. They remember the values of all the variables that were in scope on the moment of their definition. They are then able to access those variables when it is called even if they are in a different scope.

### Proc

```ruby
pr = proc { a }
pr.call #=> NameError: undefined local variable or method 'a' for main:Object
a = 1

# On `pr` creation variable `a` wasn't defined, so there is no link from `pr` to current scope.
# That's why we get `NameError`:
pr.call #=> NameError: undefined local variable or method 'a' for main:Object
```

If we define `a` before `proc` creation:

```ruby
a = 1
pr = proc { a }
pr.call #=> 1

a = 10
# On `pr` creation variable `a` was defined in the scope, so there is the link from `pr` to current scope.
# That's why we don't see `NameError`:
our_proc.call #=> 10
```

### Lambda

```ruby
la = -> { a }
la.call #=> NameError: undefined local variable or method 'a' for main:Object
a = 1
la.call #=> NameError: undefined local variable or method 'a' for main:Object
```

If we define `a` before `lambda` creation:

```ruby
a = 1
la = -> { a }
la.call #=> 1

a = 10
la.call #=> 10
```
