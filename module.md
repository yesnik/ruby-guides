# Modules

Ruby modules allow you to create groups of methods that you can then include or mix into any number
of classes. Modules only hold behaviour, unlike classes, which hold both behaviour and state.

All modules in Ruby are instances of Module.

Module is the superclass of Class, so this means that all classes are also modules, and can be used as such.

```ruby
module Cat; end
Cat.class #=> Module
Module.ancestors #=> [Module, Object, Kernel, BasicObject]
Module.class #=> Class
Module.superclass #=> Object
```

When you're creating libraries with Ruby, it's a good practice to namespace your code 
under the name of your library or project.
Using modules and namespacing is the standard way of organizing libraries with Ruby.


### We cannot instantiate modules

Since a module cannot be instantiated, there is no way for its methods to be called directly.
Instead, it should be included in another class, which makes its methods available for use in 
instances of that class.

```ruby
module Some
end
Some.new #=> NoMethodError: undfined method new ...
```

### Module's methods are accessible for class instance

```ruby
module Polite
  def hi
    'Hi, nice to meet you'
  end
end

class Robot
  include Polite
end
Robot.new.hi #=> 'Hi, nice to meet you'
```

### Module methods can affect instance variables in the object

```ruby
module Nameable
  def name=(n)
    @name = n
  end

  def here
    :in_module
  end
end

class Dog
  include Nameable
  
  attr_reader :name

  def initialize
    @name = 'Fido'
  end

  def here
    :in_object
  end
end

fido = Dog.new
fido.name= 'Kenny'
fido.name #=> 'Kenny'
```

Classes can override module methods

```ruby
fido = Dog.new
fido.here #=> :in_object

Ken.new #=> NameError: uninitialized constant Ken

module Kenny
  class Dog

  end
end

Kenny::Dog.new
```

### .included callback

Method 'included' is a callback that Ruby invokes whenever the module is included into another module/class.
`Module.included` method receives just one parameter, which is the Module or Class into which your `Module` was included.

```ruby
module Robot
  def self.included(base)
    p "Module Robot was included in class #{base}"
  end
end

class Robocop
  include Robot
end
```

### .extended callback

The `Module#extended` callback is triggered when an object is extended using `Module#extend`.
The only difference is that `Module#extended` receives the object as a parameter as opposed to `Module#included` where parameter is always either Class or a Module.

```ruby
module Man
  def self.extended(base)
    p "Class #{base} was extended by module #{self}"
  end
end

class Kenny
  extend Man
end
#=> "Class Kenny was extended by module Man"
```

### Include module with class methods

```ruby
module Man
  def self.included(base)
    base.extend ClassMethods
  end

  module ClassMethods
    def hi
      'hi'
    end
  end
end

class Kenny
  include Man
end

Kenny.hi #=> 'hi'
```

It would come in handy when you need to write modules 
that also can add class level methods to the target class.

### Module with ClassMethods and InstanceMethods

```ruby
module Polite
  def self.included(class_or_module)
    class_or_module.send :include, InstanceMethods
    class_or_module.extend ClassMethods
  end

  module ClassMethods
    def polite?
      true
    end
  end

  module InstanceMethods
    def greet
      'hello'
    end
  end
end

class Cat
  include Polite
end

p Cat.polite? #=> true
tom = Cat.new
p tom.greet #=> 'hello'
```

### .inherited callback

`Class#inherited` lifecycle callback method is invoked whenever a given Class is sub-classed.
It receives one parameter, which is the sub-class.

```ruby
class Cat
  def self.inherited(klass)
    p "Class #{klass} was inherited from #{self}"
  end
end

class Tom < Cat; end
#=> "Class Tom was inherited from Cat"
```

### .extend

Use `extend` for adding class level methods.
The `extend` method works similar to `include`, but unlike `include`, 
you can use it to extend any object by including methods and constants from a module.

```ruby
module Robot
  def fire
    'Fire!'
  end
end

class Robocop; end

robocop = Robocop.new
robocop.extend Robot
robocop.fire #=> 'Fire!'
```

### 'extend' works everywhere

The important thing to not here is that 'extend' works everywhere: 
inside a class/module definitiion and on specific instances.
'include' however cannot be used on specific objects.

```ruby
module Robot
  def fire
    'Fire!'
  end
end

class Robocop
  extend Robot  
end

rob = Robocop.new
rob.fire # undefined method `fire' for
Robocop.fire #=> "Fire!"

class Terminator; end
arny = Terminator.new
arny.extend Robot
arny.fire #=> "Fire!"
Terminator.fire #=> undefined method `fire' for Terminator:Class
```

### 'include' cannot be used on specific objects

```ruby
module Robot
  def fire
    'Fire!'
  end
end

class Terminator; end
arny = Terminator.new
arny.include Robot #=> undefined method `include'
```

### We can use 'extend' instead of 'include'

```ruby
module Robot
  def fire
    'Fire!'
  end
end

class Robocop
  def initialize
    self.extend Robot
  end
end

Robocop.new.fire #=> 'Fire!'
Robocop.fire #=> undefined method `fire' for Robocop:Class
```

Note that the same result we can get using 'include':

```ruby
class Robocop
  include Robot
end

Robocop.new.fire #=> 'Fire!'
Robocop.fire #=> undefined method `fire' for Robocop:Class
```

### .module_eval

It allows to add method to module dynamically.

```ruby
module Polite
  def hi
    'Hello'
  end
end

Polite.module_eval do
  def bye
    'Good bye'
  end
end

class Cat
  extend Polite
end

p Cat.hi #=> 'Hello'
p Cat.bye #=> 'Good bye'
```

### .module_function

`module_function` allows us to call module's methods directly.

```ruby
module Hero
  def secret
    'Secret!'
  end
  
  module_function
  
  def hi
    'Hi!'
  end
  
  def hey
    'Hey!'
  end
end

Hero.secret #=> undefined method `secret' for Hero:Module
Hero.hi #=> 'Hi'
Hero.hey #=> 'Hey!'
```

`module_function` acts like `private` keyword for module's methods:

```ruby
class Batman
  extend Hero
end

p Batman.secret #=> "Secret!"
p Batman.hey #=> private method `hey' called for Batman:Class


class Cat
  include Hero
end

tom = Cat.new

p tom.secret #=> "Secret!"
p tom.hey #=> private method `hey' called for #<Cat:0x000055e6dcd5c6e0>
```


