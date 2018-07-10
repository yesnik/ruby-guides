
# Method

Methods exposed by any object are themselves objects.

### .method

All ruby objects expose .method that can be used to get hold of any of its
methods as an object.

Here we ask the object (that is the integer 1) to give us the instance of the method 'next'.

```ruby
m = 1.method('next')
#=> #<Method: Integer#next>
m.class #=> Method
```

In this example .method returned object that holds method with name 'next'.

The method object still maintains a relationship with the object to which it belongs 
so you can still call it using 'call' method
and it responds like a normal invocation of that method:

```ruby
m.call #=> 2
m.call #=> 2
```

A method must always return exactly one object.

```ruby
def hello
  'hello'
end

hello(1) #=> ArgumentError: wrong number of arguments
```

Wrong number of arguments is not a syntax error, but a runtime error.

### .arity

It represents the number of arguments that the method can accept.

```ruby
def hi(a, b, c)
end

method_hi = self.method :hi
p method_hi.arity #=> 3

def hi(a, b, c=1, d=2); end
method(:hi).arity #=> -3
```

If method has optional args, arity calculates with formula: `-(1 + N)`, where N - amount of required args.

### .parameters

```ruby
def hi(a, b=5, c)
end

method_hi = self.method :hi
p method_hi.parameters #=> [[:req, :a], [:opt, :b], [:req, :c]]

def hi(a, b=5, &some_block)
end

m = method :hi
p m.parameters #=> [[:req, :a], [:opt, :b], [:block, :some_block]]

def hey(a, *opts); end
method(:hey).parameters #=> [[:req, :a], [:rest, :opts]]
```

- `:req` - required param
- `:opt` - optional param
- `:block` - block param
- `:rest` - rest params

### .reciever and .owner

Method .receiver returns object on which the method is bound,
method .owner is the class that object belongs to.

```ruby
class Cat
  def login
    'tom'
  end
end

tom = Cat.new
m = tom.method :login

m.reciever #=> #<Cat:0x00559923123>
m.owner #=> Cat
```


### Method to proc

Method is simply a block bound to an object, with access to the object's state.

```ruby
class Cat
  def hi(name)
    "Hi #{name}"
  end
end

hi_method = Cat.new.method(:hi)
hi_method.class #=> Method
hi_method.class.ancestors #=> [Method, Object, Kernel, BasicObject]

hi_proc = hi_method.to_proc
hi_proc.call('ken') #=> 'Hi ken'
```

### Default arguments

```ruby
def hi(name, greet='hi')
  "#{greet}, #{name}"
end
```

### Many arguments

We use splat operator (*) to handle methods which have a variable parameter list.

```ruby
def hi(*x)
  x
end

hi.class #=> Array
hi 1, 2 #=> [1, 2]

def add(*numbers)
  numbers.inject(0) { |sum, n| sum + n }
end
add 1,2,3 #=> 6
add 1,2,3,4 #=> 10
```

We can use splat to split array to arguments list:

```ruby
def add(a, b)
  a + b
end
arr = [10, 5]
add(*arr) #=> 15
```

### Private methods

```ruby
def hi
  'hey'
end
private :hi

hi #=> 'hey'
self.hi #=> NoMethodError: private method 'hi' called for main:Object
```

### Method with keyword arguments

```ruby
def hi(name: 'Kenny', age: 20)
  [name, age]
end

hi #=> ['Kenny', 20]
hi(name: 'Jenny') #=> ['Jenny', 20]
hi age: 16 #=> ['Kenny', 16]

def hey(name, age: 20)
  [name, age]
end

hey('Kenny') #=> ['Kenny', 20]
hey('Kenny', 18) #=> ArgumentError (wrong number of arguments (given 2, expected 1))
hey #=> ArgumentError: wrong number of arguments (given 0, expected 1)
```

## Who holds the behaviour?

Who holds the methods? Is it stored only in the class definition, or does each instance carry
a copy of its entire behaviour?

```ruby
class Cat
  def speak
    'Meow'
  end
end

tom = Cat.new
p tom.speak #=> 'Meow'

class Cat
  def speak
    'Hi'
  end
end

kesha = Cat.new
p kesha.speak #=> 'Hi'

p tom.speak #=> 'Hi'
```

So behaviour is stored in the class, not in the instance.

## Singleton methods

We can define methods that are available only for a specific object. 
Such methods are called Singleton Methods.

```ruby
class Cat
  def speak
    'Meow'
  end
end

tom = Cat.new
def tom.hi
  'Hi'
end

p tom.hi #=> 'Hi'

Cat.new.hi #=> undefined method 'hi' for #<Cat:0x0055f07ea85880>
```

Method 'hi' is singleton method because it belongs to just one instance of a class 
and will not be shared with others.

But singleton methods contracict what we found early: instance objects cannot hold methods,
only class definitions (objects of class Class) can. 
Here is an answer to this. 
When you declare a singleton method on an object, Ruby automatically 
creates a class to hold just that method. This class is called the 'metaclass' of the object.
All subsequent singleton methods of this object goes to its metaclass.
It it is not there, it gets propagated to the actual class of the object and
if it is not found there, the message traverses the inheritance hierarchy.

## Access the metaclass of an object

```ruby
class Object
  def metaclass
    class << self
      self
    end
  end
end

class Cat; end

tom = Cat.new

def tom.hi
  'Hi'
end

p tom.class.instance_methods.include? :hi #=> false
p tom.metaclass.instance_methods.include? :hi #=> true
tom.metaclass.new #=> can't create instance of singleton class
```

Here the `class << self` syntax changes the current self to point to the metaclass of the current object.
Since we are already inside an instance method, this would be the instance's metaclass.
The method simply returns `self` which at this point is the metaclass of the instance.

A metaclass is almost a class in many respects. It however can't be instantiated.

### .singleton_class

This method is a shorthand for the class << self syntax we saw earlier. So we can just call singleron_class
instead of our custom metaclass method.

```ruby
class Object
  def singleton_method?(method)
  	singleton_methods = self.singleton_class.instance_methods - self.class.instance_methods
  	singleton_methods.include? method
  end
end

foo = 'tom'
def foo.hi
  'hi'
end

p foo.singleton_class
#=> #<Class:#<Cat:0x005611293f5eb8>>

p foo.singleton_method?(:hi) #=> true
```

In Ruby, both 'metaclass' and 'singleton class' means the same.
You'll find them being used interchangeably in Ruby literature.

### .singleton_methods

```ruby
class Cat; end
tom = Cat.new
def tom.hey
  'Hey'
end

tom.singleton_methods #=> [:hey]
```
