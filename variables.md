# Variables

## Scope

```ruby
a = 1
defined?(a) #=> "local-variable"
defined?(b) #=> nil
```

## Globals

### Global variables starts with $

```ruby
module Robot
  class Terminator
    $greet = 'Hi'
  end
  
  class Robocop
    def hi
      p $greet
    end
  end
end

Robot::Robocop.new.hi #=> 'Hi'
```

### Internal Ruby globals

`$0` - the name of the current ruby script

`$~` - the last regular expression match

`$@` - the location of the last error

`$*` - the command line arguments used to execute this Ruby program
