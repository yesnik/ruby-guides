# Object

Every expression in Ruby evaluates to an object.

```ruby
nil.is_a?(Object)  #=> true
1.is_a? Object #=> true
'hello'.is_a? Object #=> true

nil.nil? #=> true

nil.to_s #=> ''

nil.inspect #=> 'nil'

obj = Object.new
obj.object_id.class #=> Integer

1.private_methods
1.protected_methods
```

To know which object you are at the moment

```ruby
self #=> main
```

If you don't specify which object you are, you automativally play the role 
of the 'main' object that Ruby provides us by default.

### .object_id

The Ruby VM keeps track of every object in memory through the object_id method which
present in all objects in Ruby. Different objects always have different ids.

```ruby
(1..5).each { |x| p x.object_id }
3
5
7
9
11
```

### .class

The class definition from which an object was instantiated is an intrinsic part of its identity.
This method exists in every object in Ruby.

```ruby
p 'hi'.class #=> String

class Foo; end
p Foo.new.class #=> Foo

p String.class #=> Class
p Foo.class #=> Class
```

### .superclass

This method lets us inspect the inheritance hierarchy of a class definition.

```ruby
class Foo; end
class Bar < Foo; end
Bar.superclass #=> Foo
Foo.superclass #=> Object
```

*Excercise:* Implement a method `superclasses` inside Object that 
returns this class hierarchy information?

```ruby
class Object
  def superclasses(kls = superclass)
    return [] unless kls

    [kls] + superclasses(kls.superclass)
  end
end

class Foo; end
class Bar < Foo; end

p Bar.superclasses #=> [Foo, Object, BasicObject]
```

### .ancestors

Method returns an array that starts with the class or module definition on which it was invoked.
It then contains the superclasses and mixed in modules of the object.

```ruby
module Smilable
  def smile
    ')))'
  end
end

class Cat
  include Smilable
end

Cat.ancestors
#=> [Cat, Smilable, Object, Kernel, BasicObject]
```

Kernel is a module that is mixed into the Object class. 
Kernel contains methods like: puts, p, rand, loop

### .instance_of?

```ruby
class Foo; end
foo = Foo.new
foo.instance_of? Foo #=> true
foo.instance_of? Object #=> false
```

It's the same as:

```ruby
foo.class == Foo
```

### .is_a?

This method tells you whether the object contains behaviour of the given class.

```ruby
class Foo; end
Foo.is_a? Object #=> true
foo = Foo.new
foo.is_a? Foo #=> true
foo.is_a? Object #=> true
```

### .clone

This method creates an independent copy of an object.

```ruby
obj = Object.new
clone = obj.clone

obj == clone #=> false
obj.object_id == clone.object_id #=> false
```

It will be without cloning:

```ruby
a = [1, 2]
b = a

a << 3

p a #=> [1, 2, 3]
p b #=> [1, 2, 3]

a.object_id == b.object_id #=> true
```

This is because variables store only references to objects.
When assigning one variable to another, Ruby does not copy the actual object - 
only the references are copied.

```ruby
name = 'kenny'
users = [name]
name.upcase!

p users #=> ['KENNY']
```

The clone method should be used when you need to mutate an object due to performance reasons.
This ensures that the original object is not affected. Mutation of objects results in brittle code 
and should be avoided.

### .hash

This method is used to check the equality of 2 objects. While '==' serves the purpose well,
it's not really fast. For operations that might involve large number of equality checks 
(like Array#uniq and Hash lookups), the speed disadvantage adds up and becomes an overhead.
To get abound this, Ruby provides a 'hash' method for every object.

It returns a numeric value which is usually unique to every object.

```ruby
a = 15
a.hash #=> -253960414146879521
b = 15
b.hash #=> -253960414146879521
```

### .caller

The caller method returns the stack trace as an array.

```ruby
def hi
  'Who called me?'
  caller
end

hi
#=> ["(irb):12:in `irb_binding'", 
# "/home/nik/.rvm/rubies/ruby-2.5.1/lib/ruby/2.5.0/irb/workspace.rb:85:in `eval'", 
# "/home/nik/.rvm/rubies/ruby-2.5.1/lib/ruby/2.5.0/irb/workspace.rb:85:in `evaluate'",
# ... ]
```

### .feeze

This method prevents any changes to an object.

```ruby
a = [1, 2]
a.freeze
a << 3
#=> RuntimeError: can't modify frozen Array
a = [5, 6] # Hmmm.. no error here.
```

This is because variables only store references to object. 
The `freeze` method was executed on the actual object and the object
remains frozen. However, we can change the variable to refer to 
a different object anytime - which is what the example above did.

### .frozen?

```ruby
a = [1, 2]
a.freeze
a.frozen? #=> true
[1, 2].frozen? #=> false
```

## Objects equality

To apply uniq method to array of the same objects 
we must define `hash`, `eql?` methods for this object.

```ruby
class Cat
  attr_reader :name, :year

  def initialize(name, year)
    @name = name
    @year = year
  end

  def to_s
    "#{name}, #{year}"
  end

  def eql?(other)
    name == other.name && year == other.year
  end

  def ==(other)
    eql?(other)
  end

  def hash
    name.hash ^ year.hash
  end
end

a = Cat.new('Simon', 2017)
b = Cat.new('Simon', 2017)
c = Cat.new('Simon', 2016)

p [a, b, c].uniq
=> [#<Cat:0x00565507552dc0 @name="Simon", @year=2017>, #<Cat:0x00565507552d20 @name="Simon", @year=2016>]
```

We use `hash` method that returns the result of XORing (using the ^ operator) 
the 'hash' of all that instance variables which together determine the state of the object.

We use `==` to check for equality of objects. 
It compares the state of your object with that of the other one and returns a boolean value.

```ruby
a == b #=> true
a == c #=> false
```

Routines like `Array#uniq` uses the `eql?` instead.
This means that we must implement the `eql?` method as well whenever we override `==`.
In most cases, these two methods will be identical, 
so you can implement the actual comparison
in one method and have the other method just call it.

To summarize, if you ever override any of the `==`, `eql?` or the `hash` method,
you must override the others as well.

## puts vs p

`puts` generally prints the result of applying `to_s` on an object.
While `p` prints the result of `inspect`-ing the object.

```ruby
class Cat
  def inspect
    'from inspect'
  end
end

cat = Cat.new
puts cat #=> #<Cat:0x0055879539c480>
p cat #=> from inspect
```

## to_s vs inspect

```ruby
class Cat
  def initialize(n)
    @name = n
  end
end

cat = Cat.new('Simon')

puts cat.to_s #=> #<Cat:0x00556d5a7cc140>
p cat #=> #<Cat:0x00556d5a7cc140 @name="Simon">
```

`puts` prints the class name of the object along with a number displayed as hex.
This number is relative to the position of the object in memory, 
but we seldom find any use for it.

`p` on the other hand prints the class name and all the instance variables of the object.

Also, it's generally a best practive to override the `to_s` method 
in your classes so that it can return
meaningful result that is tailored for each class.

### Splat operator

```ruby
a, *b = [11, 22, 33]
a #=> 11
b #=> [22, 33]
```

### Assignment statement

```ruby
name = nil
name ||= 'Kenny'
p name #=> 'Kenny'

age = 19
age ||= 16
p age #=> 19
```

So `age ||= 16` gives the value 16 if `age` is `nil` or `false`.

```ruby
count = 0
count += 1
p count #=> 1

price = 100
discount = 0.8
price *= discount # same as: price = price * discount
p price #=> 80
```

# BasicObject

The Object class is the ancestor of almost all objects in Ruby.
Object however inherits from BasicObject.

```ruby
Object.superclass #=> BasicObject
BasicObject.superclass #=> nil
```

BasicObject is useful, when you need to build a new minimal object 
that doesn't include the methods defined by Object and Kernel.

