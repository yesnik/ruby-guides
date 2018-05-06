# Class

```ruby
Class.ancestors
#=> [Class, Module, Object, Kernel, BasicObject] 

class Cat; end
cat = Cat.new
cat.class #=> Cat

class Cat
  def set_name(n)
    @name = n
  end
end

cat = Cat.new
cat.instance_variables
#=> []

cat.set_name('Barsik')
cat.instance_variables
#=> [:@name]
```

An instance variable is bound to the specific instance of the class. By binding itself to the entire object,
an instance variable makes itself available to every method of the object.

A local variable is available only inside the method it is defined. It is not shared across the entire object.

### Another syntax for defining class

```ruby
robot = Class.new do
  def self.hi
  	'Hi'
  end
end

robot.hi #=> 'Hi'
```

We use capitalized constant when put class in module:

```ruby
module Machine
  Robot = Class.new do
    def self.hi
    	'Hi'
    end
  end
end

Machine::Robot.hi #=> 'Hi'
```

### .const_get

Method returns constant with the name of passed value.

```ruby
class Cat
  NAME = 'Tom'
end

Kernel.const_get('Cat') == Cat #=> true
Kernel.const_get('Robocop') #=> NameError: wrong constant name Robocop
Kernel.const_get('Cat::NAME') #=> 'Tom'
```

### .instance_variable_get

It allows us to retrieve the value of an instance variable defined on our class or module.

```ruby
class Cat
  def initialize
    @name = 'Tom'
  end
end

tom = Cat.new
tom.name #=> NoMethodError: undefined method 'name' for #<Cat:0x00589922 @name="Tom">
tom.instance_variable_get '@name' #=> 'Tom'
tom.instance_variable_get 'name' #=> NameError: 'name' is not allowed as an instance variable name
tom.instance_variable_get '@some_name' #=> nil
```

### .instance_variable_set

Sets the instance variable named by symbol to the given object, 
thereby frustrating the efforts of the class's author 
to attempt to provide proper encapsulation. The variable does not have to exist prior to this call. 
If the instance variable name is passed as a string, that string is converted to a symbol.

```ruby
module BlindInitialize
  def initialize(h)
    h.each { |key, value| instance_variable_set "@#{key}", value }
  end
end

class ErrorMessage
  attr_accessor :message, :status
  include BlindInitialize
end

p ErrorMessage.new(message: 'No free memory', status: 123)
#=> #<ErrorMessage:0x00558ec3619430 @message="No free memory", @status=123>
```

Note that we can set missing instance variable:

```ruby
p ErrorMessage.new(message: 'No free memory', status: 123, foo: 1)
#=> #<ErrorMessage:0x00562220c447b0 @message="No free memory", @status=123, @foo=1>
```

It's better to prevent this:

```ruby
module BlindInitialize
  def initialize(h)
    h.each { |key, value| send "#{key}=", value }
  end
end

p ErrorMessage.new(message: 'No free memory', status: 123, foo: 1)
#=> undefined method `foo=' for #<ErrorMessage:0x0056488f271720>
```

*Example*

```ruby
module HashInitialized
  def hash_initialized(*fields)
    define_method(:initialize) do |h|
      missing = fields - h.keys
      
      raise "Undefined keys: #{missing}" if missing.any?
      
      h.each do |key, value|
        instance_variable_set("@#{key}", value) if fields.include? key
      end
    end
  end
end

class Cat
  extend HashInitialized
  
  hash_initialized :nickname, :age
end

p Cat.new(nickname: 'Tom', age: 2)
#=> #<Cat:0x00561f13ed4a58 @nickname="Tom", @age=2>

p Cat.new(nickname: 'Tom', age: 2, city: 'NY')
#=> #<Cat:0x0055de0d9cbf30 @nickname="Tom", @age=2>

p Cat.new(nickname: 'Tom')
#=> Error: Undefined keys: [:age]
```

### .instance_eval

This method is like eval, but it is scoped by the object you specify.
We can add class method like this:

```ruby
class Cat; end
Cat.instance_eval('def hi; "hi"; end', __FILE__, __LINE__)
Cat.hi #=> 'hi'
Cat.new.hi #=> NoMethodError: undefined method `hi' for #<Cat:0x00564>
```

We can use this method to get access to class' object data.
Instance variables are created when you create a new instance from your class.

```ruby
class Cat
  def initialize
    @login = 'Tom'
  end
end

p Cat.instance_eval '@login' #=> nil
tom = Cat.new
p tom.instance_eval '@login' #=> 'Tom'
```

When we use block we can omit quotes wrapping variable name:

```ruby
p tom.instance_eval { @login } #=> 'Tom'
```

We can use this method to create singleton methods. Singleton methods are only available
to one single instance of the class. 

```ruby
class Cat; end
tom = Cat.new
tom.instance_eval do
  def hi
    'hi'
  end
end

p tom.hi #=> 'hi'

bob = Cat.new
p bob.hi #=> undefined method `hi' for #<Cat:0x00563c9717d570>
```

### .class_eval

It works on the class as if you were opening it in a text editor and adding new state or behaviour to it.

```ruby
class Cat
  def age=(years)
    @age = years
  end
end

tom1 = Cat.new
p tom1.respond_to? :age #=> false

Cat.class_eval('def initialize; @age = 5; end')
Cat.class_eval('def age; @age; end')

p tom1.respond_to? :age #=> true
p tom1.age #=> nil

tom2 = Cat.new

p tom2.age #=> 5

tom1.age = 10
p tom1.age #=> 10 
```

As we can see, `Cat#age` is accessible even for all instances.

Also we can use class_eval with block:

```ruby
Cat.class_eval do
  def initialize
    @age = 5
  end
end
```

### .superclass

This method tells you which class any given class was inherited from.

```ruby
Float.superclass #=> Numeric
Numeric.superclass #=> Object
Object.superclass #=> BasicObject
BasicObject.superclass #=> nil
```

### .instance_methods

Float has methods:

```ruby
Float.instance_methods.count #=> 121
```

Let's remove those methods that are inherited from Object:

```ruby
Float.instance_methods.count - Object.instance_methods.count #=> 61
```

Let's remove methods from Numeric

```ruby
(Float.instance_methods - Object.instance_methods - Numeric.instance_methods).count #=> 12
```

### .class_eval

```ruby
Cat = Class.new
Cat.class_eval do
  def hi
    'hi'
  end
end

Cat.new.hi #=> 'hi'
```

### Class variables

We can use class variables to store application configuration - 
things like application name, version, database, etc.

```ruby
class Cat
  @@counter = 0

  def initialize
    @@counter += 1
  end

  def self.counter
    @@counter
  end
end

Cat.new
Cat.new
Cat.counter #=> 2

class Simon < Cat; end

Simon.new
Simon.counter #=> 3
```

Be careful: subclasses also affect class variable of parent class.

- Instance variables ( @name ) are a better alternative than class variables simply 
because the data is not shared across the inheritance chain.
- Instance variables are available only in class methods.
- Class variables ( @@name ) are available in both class methods and instance methods.

### Instance variables cannot be accessed outside the class

```ruby
cat = Cat.new
cat.set_name('Kesha')
cat.name #=> NoMethodError: undefined method 'name'
```

But we can get this value:

```ruby
cat.instance_variable_get(:@name) #=> 'Barsik'
cat.instance_eval('@name') #=> 'Barsik'
cat.instance_eval { @name } #=> 'Barsik'
```

### attr_reader

```ruby
class Cat
  def name
    @name
  end

  def set_name(n)
    @name = n
  end
end
```

We can ommit 'name' method:

```ruby
class Cat
  attr_reader :name

  def name=(n)
    @name = n
  end
end

cat = Cat.new
cat.name = 'Barsik'
cat.name #=> 'Barsik'
```

### attr_writer

```ruby
class Dog
  attr_writer :nickname

  def nickname
    @nickname
  end

  def initialize(name)
    @nickname = name
  end
end

dog = Dog.new('Jerry')
dog.nickname #=> 'Jerry'
dog.nickname = 'Leo'
doc.nickname #=> 'Leo'
```

### attr_accessor

This method will automatically define both read and write accessors

```ruby
class Cat
  attr_accessor :name
end

cat = Cat.new
cat.name = 'Leo'
cat.name #=> 'Leo'
```

### inspect method

```ruby
class Cat
  attr_accessor :name
  def initialize(n)
    @name = name
  end

  def inspect
    "Cat named: '#{name}'"
  end

  def to_s
    name
  end
end

cat = Cat.new('Barsik')
cat
#=> Cat named 'Barsik' 
"#{cat}" #=> "Barsik"
```

### Call methods

```ruby
class Dog
  def say(name = '')
    'hi' + name
  end
end
dog = Dog.new
dog.say #=> 'hi'
dog.send :say #=> 'hi'
dog.__send__ :say #=> 'hi'
dog.send 'say' #=> 'hi'
dog.public_send :say #=> 'hi'
dog.send :say, 'Leo' #=> 'hiLeo'

dog.respond_to? :say #=> true
dog.respond_to? :hi #=> false
```

### Add new methods to existing class

```ruby
class Dog
  def bark
    'Woof'
  end
end

class Dog
  def wag
    'happy'
  end
end

jerry = Dog.new
jerry.bark #=> 'Woof'
jerry.wag #=> 'happy'
```

### Even existing built in classes can be reopened

```ruby
2.even? #=> true

class ::Integer
  def even?
    (self % 2 == 0) ? 'y' : 'n'
  end
end

2.even? #=> 'y'
```

## Class inheritance

### Avoid method redefine

Never redefine methods, ever, especially with classes supplied by the language.

Example:

```ruby
class Integer
  def +(n)
    25
  end
end

100 + 50 #=> 25 
1 + 1 #=> 25
```

### .super

We use 'super' method to call parent's class method.

```ruby
class Animal
  def say
    'woo'
  end
end

class Cat
  def say
    super + ' purr'
  end
end

cat = Cat.new
cat.say #=> 'woo purr'
```

### .method_missing

```ruby
class Cat; end
cat = Cat.new
cat.hey #=> NoMethodError: undefined method 'hey' for ...
cat.method_missing #=> NoMethodError: private method 'method_missing' called for ...

class Cat
  def method_missing(method_name, *args, &block)
    p "called: #{method_name}"
  end
end

cat = Cat.new
cat.hey #=> "called: hey"
cat.hey2 #=> "called: hey2"
cat.respond_to? :hey2 #=> false


class Hello
  def method_missing(method_name, *args, &block)
    if method_name.to_s[0, 3] == 'hey'
      'hey to you too'
    else
      super(method_name, *args, &block)
    end
  end
end
```

*Example with method missing*

```ruby
class Cat
  attr_accessor :cute
  
  def cute?
    cute == true
  end
end

class Object
  def not
    Not.new(self)
  end
  
  class Not
    def initialize(original)
      @original = original
    end
    
    def method_missing(sym, *args, &block)
      !@original.send(sym, *args, &block)
    end
  end
end

tom = Cat.new
p tom.cute? #=> false
tom.cute = true
p tom.cute? #=> true

p tom.not.cute? #=> false
```

### Method to_str allows us to use object as string

```ruby
class MyString
  def to_s
    'hello'
  end
end

File.exists?(MyString.new) #=> TypeError: no implicit conversion of MyString into String

class MyString
  def to_str
    'hello'
  end
end

File.exists?(MyString.new) #=> false
```

### Compare classes' instances

```ruby
class Cat
  def initialize(name)
    @name = name
  end

  def to_s
    "#{@name}"
  end

  def ==(other)
    to_s == other.to_s
  end
end

Cat.new('Simon') == Cat.new('Simon') #=> true
Cat.new('Simon') == Cat.new('Simon 1') #=> false
```

### Class names are just constants

```ruby
MyString = ::String
MyString == ::String #=> true
MyString == 'hello'.class #=> true
```

### Class constants

```ruby
class Cat
  NAME = 'Kesha'
end

Cat::NAME #=> 'Kesha'
Cat.const_get 'NAME' #=> 'Kesha'
Cat.constants #=> [:NAME]
```

**We cannot reassign constansts dynamically**

```ruby
class Man; end

Man::Age = 20

p Man::Age #=> 20

def change
  Man::Age = 21 # SyntaxError: dynamic constant assignment
end
```

### .methods | .public_methods

These methods return a list of public methods available on an Object and its ancestors.

```ruby
class Cat
  def self.hi
    'hi'
  end

  def hello
    'hello'
  end
end

Cat.methods.size #=> 102
Cat.public_methods.size #=> 102
Cat.methods == Cat.public_methods #=> true
```

If you want to ignore the ancestors and restrict the listing to just the 
receiver, you can pass in false:

```ruby
p Cat.methods(false) #=> [:hi]
p Cat.public_methods(false) #=> [:hi, :new, :allocate, :superclass]
```

Let's create an instance of Cat:

```ruby
tom = Cat.new
tom.methods.size #=> 57
tom.methods == tom.public_methods #=> true
```

We can try to pass `false` to methods:

```ruby
tom = Cat.new
tom.methods(false) #=> []
tom.public_methods(false) #=> [:hello]
```

### We can define methods on individual objects

```ruby
fido = Dog.new
def fido.wag
  :hi
end
fido.wag #=> :hi

class Cat
  def self.hi
    :hi_from_class
  end

  class << self
    def hello
      :hi2
    end
  end
end
Cat.hi #=> :hi_from_class
Cat.hello #=> :hi2
```

### We can define methods on classes

```ruby
def Dog.wag
  :hi_from_class
end

Dog.wag #=> :hi_from_class
```

### Classes and instances don't share instance variables

```ruby
class Cat
  attr_accessor :age
end
cat = Cat.new
cat.age = 3
cat.age #=> 3
Cat.age #=> NoMethodError: undefined method age for Cat::class
```

### Class statements return the value of their last expression

```ruby
class Dog
  123
end
#=> 123

class Cat
  self
end
#=> Cat
```

### We can call class methods from instance methods

```ruby
class Cat
  def self.hi
    :hi
  end
end

kesha = Cat.new
kesha.class.hi #=> :hi
```

Class methods don't have access to instance methods or instance variables.
However instance methods can access both class methods and class variables.

