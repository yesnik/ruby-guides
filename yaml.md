# YAML

```ruby
require 'yaml'

class Cat
  attr_reader :name
  def initialize(name)
    @name = name
  end  
end

simon = Cat.new('Simon')
simon_str = YAML::dump(simon)
#=> "--- !ruby/object:Cat\nname: Simon\n"

simon_restored = YAML::load(simon_str)
simon_restored.name #=> 'Simon'
simon_restored.class #=> Cat
```
