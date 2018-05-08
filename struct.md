# Struct

```ruby
Student = Struct.new(:name, :age) do
  def greeting
    "Hello #{name}! Is your age #{age}?"
  end
end

ken = Student.new('Kenny', 20)
p ken.greeting #=> "Hello Kenny! Is your age 20?"
p ken.members #=> [:name, :age]

ken.each do |value|
  p "#{value}"
end
#=> "Kenny"
#=> "20"

ken.each_pair do |key, value|
  p "#{key} -- #{value}"
end
#=> "name -- Kenny"
#=> "age -- 20"
```
