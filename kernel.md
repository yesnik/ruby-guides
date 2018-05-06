# Kernel

### .puts

Whenever you call `puts`, you're actually calling `Kernel.puts`
Methods in Kernel are accessible everywhere in Ruby.

### .eval

This method belongs to Kernel module, which is included in the Object class.
Therefore it available on any Ruby objects.

Method `.eval` can take a second parameter - Binging object:

```ruby
def get_binding
  binding
end

class Cat
  def get_binding
    binding
  end
end

p eval 'self', get_binding #=> main
p eval 'self', Cat.new.get_binding #=> #<Cat:0x0055ed1f2269f8>
```

Here we see how `self` has changed in different binding contexts. 
This is how you can change `self`
while evaluation code through `eval` using bindings.

It's a good practice to include the keywords __FILE__, __LINE__ 
as the 3rd and 4th parameters to `eval()`.
Adding these params makes debugging code that uses `eval` much easier.

```ruby
eval('18', binding, __FILE__, __LINE__)
```
