# nil

```ruby
nil.class #=> NilClass

nil.class.ancestors
#=> [NilClass, Object, Kernel, BasicObject]

# nil is the default value for Hash
h = {name: 'Kenny'}
h[:login] #=> nil

# nil is the return value of empty method
def hello; end
hello #=> nil

# nil is the default value of unexecuted variable
city #=> NameError (undefined local variable or method 'city' for main object)
if false
  city = 'NY'
end
city #=> nil

# Unexecuted if condition returns nil
result = if 1 == 2
  'hi'
end
result #=> nil

# case statement with no matched when clause or else returns nil
result = case :name
         when Integer then 'int'
         when String then 'str'
         end
result #=> nil

# default value for instance variable is nil
@student #=> nil

# detect method returns nil if the value not found
[1, 3, 5].detect { |n| n.even? } #=> nil

```
