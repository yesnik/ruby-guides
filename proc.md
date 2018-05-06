# Proc

```ruby
Proc.ancestors
#=> [Proc, Object, Kernel, BasicObject]

pr = Proc.new { 'hi' }
pr.call #=> 'hi'
```

### .binding

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

But if we'll to get .binding of another object we'll get an error:

```ruby
5.binding #=> NoMethodError: private method 'binding' called for 1:Integer
'hi'.binding #=> NoMethodError: private method 'binding'
```
