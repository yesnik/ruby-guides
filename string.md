# String

```ruby
'hey'.class #=> String
"\n".size #=> 1
'\n'.size #=> 1

a = 'hi'
a.each_char { |x| p x }
#=> 'h'
#=> 'i'

'HI'.downcase #=> 'hi'
'hi'.upcase #=> 'HI'
'aBc'.swapcase #=> 'AbC'
'aBc'.swapcase! #=> 'AbC'
'nek'.reverse #=> 'ken'

'aa bb'.split #=> ['aa', 'bb']

'hi'.to_s #=> 'hi'
'hi'.inspect #=> '"hi"'

'Hello my Kenny'.include?('my') #=> true

'abbc'.start_with? 'a' #=> true
'abbc'.end_with? 'c' #=> true
```

## Flexible quotes

```ruby
a = %(flexible quotes can handle both ' and " characters)
b = %!flexible quotes can handle both ' and " characters!
c = %{flexible quotes can handle both ' and " characters}
a == b #=> true
a == c #=> true

long_string = %{
a
b
}
long_string[0, 1] #=> "\n"
long_string.size #=> 5
long_string.lines.size #=> 3
```

## Here documents

```ruby
long = <<EOS
a
b
EOS

long.length #=> 4
long.lines.count #=> 2
long[0,1] #=> 'a'
```
## .chars

```ruby
'abc'.chars #=> ['a', 'b', 'c']
''.chars #=> []
```

## .count

```ruby
str = 'aabxxxx'
str.count('ab') #=> 3
```

## .tr, .tr!

It replaces letters in given string:

```ruby
'abca'.tr('ab', '_*') #=> '_*c_'
'abca'.tr('a', '_*') #=> '_bc_'
'abca'.tr('abc', '_*') #=> '_**_'
```

## .gsub

This method replaces substring

```ruby
'Hello World'.gsub(/[a-z\s]/, '') #=> 'HW'
'abbc'.gsub('bb', '') #=> "ac" 
```

We can apply method for matching parts of string:
```ruby
# upcase will be applied for letter 'o'
'hello world'.gsub(/o/, &:upcase) #=> "hellO wOrld"

# upcase function will be applied for words with length 5 or more
'good morning my friends'.gsub(/\w{5,}/, &:upcase) #=> "good MORNING my FRIENDS"
```

## .empty?

```ruby
'hello'.empty? #=> false
''.empty? #=> true
```

## .chomp

```ruby
" hi  \n".chomp #=> " hi  "
" hi".chomp #=> " hi"
```

## .chop

```ruby
" hi  \n".chop #=>" hi  "
"hi".chop #=> "h"
" hi \t \r\n".chop #=> " hi \t "
```

## .strip

```ruby
" hi  \n".strip #=> "hi"
" hi \t \r\n".strip #=> "hi"
```

## String to array

```ruby
'hi kenny   man'.split #=> ['hi', 'kenny', 'man']

a = *'hi'
p a #=> ['hi']
```

## String interpolation

```ruby
name = 'Kenny'
"Hi #{name}" #=> 'Hi Kenny'
```

## String concatenation

```ruby
a = 'hi'
b = ' world'
```

### This will create new string

```ruby
a + b #=> 'hi world' 
```

### Shovel operator modifies string

```ruby
a = 'hi'
b = ' world'
a << b
a #=> 'hi world'
b #=> ' world'
```

### concat method also modifies string

```ruby
a = 'hi'
b = ' ken'
a.concat(b) #=> 'hi ken'
a #=> 'hi ken'
b #=> ' ken'
```

Ruby programmers tend to favor the shovel operator (<<) over the
plus equals operator (+=) when buliding up strings.

### Escape characters

```ruby
'\''.size #=> 1
'\\\''.size #=> 2
```

### Substrings

```ruby
string = 'hi world'
string[3, 2] #=> 'wo'
string[3..4] #=> 'wo'
string[1] #=> 'i'

string.split #=> ['hi', 'world']

str = 'hello:world'
str.split(/:/) #=> ['hello', 'world']
```

### Strings are unique objects

```ruby
a = 'hi'
b = 'hi'
a == b #=> true
a.object_id == b.object_id #=> false
```

### object_id changes when new value is assigned

```ruby
a = 'tim'
p "#{a}, #{a.object_id}"
#=> "tim, 47196449552440"

a = 'tom'
p "#{a}, #{a.object_id}"
#=> "tom, 47196449551660"
```

We made the variable 'a' refer to a different object (the string 'tom').
Thus the object_id, which is the id of the object that the variable refers to, changed.

### Bang methods mutate object

Bang methods (ends with !) are dangerous - they mutate the objects in-place.

```ruby
a = 'tim'
p "#{a}, #{a.object_id}"
#=> "tim, 46923963657300"

a.gsub! 'tim', 'tom'
p "#{a}, #{a.object_id}"
#=> "tom, 46923963657300"
```

### In modern ruby (>= 1.9) single characters are represented by strings

```ruby
?a == 'a' #=> true
```

### big-q literal - %Q[]

```ruby
name = 'Kenny'
a = %Q[Hello,
#{name}! This is "Lora" from 'USA']
p a #=> "Hello,\nKenny! This is \"Lora\" from 'USA'"
```

### String to array

```ruby
name = 'Kenny'
%W(Lenny #{name}) #=> ["Lenny", "Kenny"]
%w(Lenny #{name}) #=> ["Lenny", "\#{name}"]
```

### Multiline values to array

```ruby
students = %w{
  Mary
  Jessica
}
#=> ['Mary', 'Jessica']
```
