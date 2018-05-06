# File

```ruby
File.ancestors
#=> [File, IO, File::Constants, Enumerable, Object, Kernel, BasicObject]
```

## Write to file

```ruby
open('hello.txt', 'w') do |f|
  f.puts 'hello'
end

File.open('hi.txt', 'w') do |f|
  f.write 'hi'
end
```

## Read from file

```ruby
open('hello.txt', 'r') { |f| f.read }
#=> "hello\n"

open('some.txt') { |f| f.readlines }
#=> ["hello\n", "world\n"]

File.open('test.txt') do |file|
  file.map { |line| line.strip.upcase }
end
#=> ['THIS', 'IS', 'TEST']

file = File.open('test.txt')
file.each_line { |line| p line }
file.close
file.closed? #=> true

file = File.open('some.txt', 'r+')

p file.read #=> 'Hello'
p file.read #=> ''
file.rewind
p file.read #=> 'Hello'
```

### Read N bytes

```ruby
file = File.open('some.txt', 'r+')
file.read(2) #=> 'he'
file.read(3) #=> 'llo'
```

### Read N bytes to buffer

```ruby
file = File.open('some.txt', 'r+')
buffer = ''
p file.read(3, buffer) #=> 'hel'
p buffer #=> 'hel'
f.close
```

### Set cursor for file

```ruby
p File.read('hello.txt') #=> "hello world\n"

File.open('hello.txt') do |f|
  f.seek(6, IO::SEEK_SET)
  p f.read(3)
end
#=> 'wor'
```

## File modes

- `r` - read-only, starts at beginning of file (default mode)
- `r+` - read-write, starts at beginning of file
- `w` - write only, truncates existing file to zero length or creates a new file for writing
- `w+` - read-write, truncates existing file to zero length or creates a new file for reading and writing
- `a` - write-only, starts at end of file, or creates a new file
- `a+` - read-write, starts at end of file, or creates a new file
- `b` - binary file mode (may appear with any of the key letters listed above)
- `t` - text file mode (may appear with any of the key letters listed above except 'b')

### Sandwich code

Sandwich code is code that comes in 3 parts:

1. the top slice of bread
2. the meat. This part changes all the time.
3. the bottom slice of bread

Because the changing part of the sandwich code is in the middle,
abstracting the top and bottom bread slices to library can be difficult
in many languages. But it's possible in Ruby.

```ruby
def file_sandwich(file_name)
  file = open(file_name)
  yield(file)
ensure
  file.close if file
end

file_sandwich('example_file.txt') do |f|
  f.readlines
end
#=> ["This\n", "test\n"]
```
