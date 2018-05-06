# True and False

### true and false are objects

```ruby
false.object_id
#=> 0

true.object_id
 => 20 
```

```ruby
true.class
#=> TrueClass 

true.class.ancestors
#=> [TrueClass, Object, Kernel, BasicObject]

false.class
#=> FalseClass 

false.class.ancestors
#=> [FalseClass, Object, Kernel, BasicObject]
```

### Only nil and false is false

```ruby
def true?(val)
  return :yes if val
  :no
end

true? false #=> :no
true? nil #=> :no

true? 0 #=> :yes
true? 1 #=> :yes
true? '' #=> :yes
true?({}) #=> :yes
true? [] #=> :yes
```
