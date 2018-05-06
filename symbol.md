# Symbols

```ruby
a = :hi
a.class #=> Symbol

a = 'hi'
a.to_sym == :hi #=> true

a = 'hi world'
a.to_sym == :'hi world' #=> true

name = 'Kenny'
:"hi #{name}" == "hi #{name}".to_sym #=> true

a = :cat
"hi #{a}" #=> 'hi cat'

a.respond_to?(:reverse) #=> false
a.respond_to? :each_char #=> false

:a + :b #=> NoMethodError
```

### Identical symbols are a single internal object

```ruby
a = :hi
b = :hi
a == b #=> true
a.object_id == b.object_id #=> true
```

### Method's name becomes symbol

```ruby
def hello
  'hello'
end

symbols_as_strings = Symbol.all_symbols.map { |x| x.to_s }
symbols_as_strings.include?('hello') #=> true

Symbol.all_symbols.include? :some_x #=> true
```

### Constants become symbols

```ruby
Name = 'Kenny'
all_symbols_as_strings = Symbol.all_symbols.map { |x| x.to_s }
all_symbols_as_strings.include?('Name') #=> true
```
