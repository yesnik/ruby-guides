# Binding

```ruby
Binding.ancestors
#=> [Binding, Object, Kernel, BasicObject] 
```

Bindings are regular objects that contain scope and the state within it, but no code.

### .binding

Returns current binding object

```ruby
binding #=> #<Binding:0x005651123>
```
