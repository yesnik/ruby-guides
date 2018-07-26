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
