# Forwardable

The Forwardable module provides delegation of specified methods to a designated object, 
using the methods `def_delegator` and `def_delegators`.

## .def_delegators

```ruby
require 'forwardable'

class Store
  extend Forwardable

  attr_accessor :items

  def_delegators :@items, :first, :last
end

store = Store.new
store.items = [5, 6, 7]
store.first #=> 5
store.last #=> 7
```
