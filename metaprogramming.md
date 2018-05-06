# Metaprogramming

It is the act of writing code that operates on code rather than on data.
Here are some of the 'meta' changes we're making to the program:

- Reopening classes: Add a method .words to Ruby's native String class

```ruby
class String
  def words
    self.split(' ')
  end
end
```

- Programmatic method invocation: Use `send` to call a method by name programmatically.

```ruby
method_name = 'find_friend'
object.send method_name

args = 'Kenny'
object.send method_name, args
```

Here is the list of some languages besides Ruby that support metaprogramming in some form:

- List via homoiconicity
- Java, C# via reflection

Reflection is the ability of a computer program to examine, introspect, and modify its own structure and
behaviour at runtime.

Learning when metaprogramming is the right tool for the job will be of considerable value to
you in creating powerful yet maintainable codebases.

Almost every major language construct in Ruby - most notably classes and methods - can be changed at runtime.
You can add methods to classes, remove them or redefine them.

## .define_method

```ruby
class Factory
  ['robocop', 'terminator'].each do |hero|
    define_method "init_#{hero}" do |argument|
      "My power is #{argument}"
    end
  end
end

f = Factory.new
p f.init_robocop(50) #=> 'My power is 50'
p f.init_terminator(10) #=> 'My power is 10'
```

We have code that's generating the code for us. Pretty meta, isn't it?

Many Ruby libraries use exactly this technique to define methods dynamically based on, say, database schemas.
Rails, for example, will create methods such as `User#find_by_first_name_and_last_name`

## .remove_method

```ruby
class Cat
  def hi; end
end

p Cat.instance_methods(false) #=> [:hi]

class Cat
  remove_method :hi
end

p Cat.instance_methods(false) #=> []
```
