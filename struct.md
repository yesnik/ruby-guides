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
## Using Struct in class

```ruby
class Student
  Status = Struct.new(:title) do
    def to_s
      title
    end
  end

  STATUSES = [
    FOREIGN = Status.new('foreign'),
    NATIVE = Status.new('native')
  ]
end

p Student::STATUSES
#=> [#<struct Student::Status title="foreign">, 
#    #<struct Student::Status title="native">]

p Student::NATIVE
#=> #<struct Student::Status title="native">

p "#{Student::FOREIGN}" #=> "foreign"
```
