# Proc

`Proc` objects are blocks of code that can be bound to a set of local variables. 
We can think of a `proc` object as a "saved" block.

```ruby
Proc.ancestors
#=> [Proc, Object, Kernel, BasicObject]

pr = Proc.new { 'hi' }
pr.call #=> 'hi'
```
Another way of defining `proc`:

```ruby
def calc(x, y, operation)
  operation.call(x, y)
end

pow = proc { |x, y| x ** y }
puts calc(2, 3, pow) #=> 8
```

## .binding

It returns a Binding object representing the context in which the `Proc` was created.
This is because procs have access to the local variables outside their scope.

```ruby
catch = proc { mouse }
catch.call #=> NameError: undefined local variable or method 'mouse' for main:Object
mouse = 'Jerry'
catch.call #=> NameError: undefined local variable or method 'mouse' for main:Object
```

But if we define variable before we create proc, there will not be error:

```ruby
girl = 'Jessica'
greet = proc { girl }
greet.call #=> 'Jessica'

girl = 'Jenny'
greet.call #=> 'Jenny'

greet.binding #=> #<Binding:0x00595656524>
```

But if we'll try to get `.binding` of another object we'll get an error:

```ruby
5.binding #=> NoMethodError: private method 'binding' called for 1:Integer
'hi'.binding #=> NoMethodError: private method 'binding'
```

## `return` word in proc

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
