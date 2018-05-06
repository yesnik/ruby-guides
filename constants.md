# Constants

```ruby
Name = 'Kenny'

class Hi
  Name = 'Insider'
  
  def name
    Name
  end

  def outer_name
    ::Name
  end
end

Hi.new.name #=> 'Insider'
Hi.new.outer_name #=> 'Kenny'
```

### Who wins with both nested and inherited constants

```ruby
class Robot
  POWER = 10
end

class Cyborg
  POWER = 100

  class Terminator < Robot
    def power
      POWER
    end
  end
end

p Cyborg::Terminator.new.power #=> 100
```
