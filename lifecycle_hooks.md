# Lifecycle hooks

Worried that someone might override a method? Register a callback and you'll be notified when something happens.
That's why it's called the Hollywood Principle: you don't poll the runtime for changes; instead, the runtime calls you
when something changes.

### .method_added

```ruby
class Cat
  def self.method_added(method_name)
    p "#{method_name} was added to #{self}"
  end
end

class Cat
  def hi; end
end
#=> "hi was added to Cat"
```

### .singleton_method_added

```ruby
class Cat
  def self.singleton_method_added(method_name)
    p "Singleton method #{method_name} was added to #{self}"
  end
end

tom = Cat.new
def tom.hi; end
#=> "Singleton method singleton_method_added was added to Cat"
```

### .method_removed

```ruby
class Cat
  def hi; end
  def self.method_removed(m)
    p "Method #{m} was removed from #{self}"
  end
end

class Cat
  remove_method :hi
end
#=> "Method hi was removed from Cat"
```
