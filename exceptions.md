# Exceptions

Exceptions are simply, just packaged objects that contain error information.
These are typically meant for more exceptional cases that require halting the current program flow
to handle abnormal conditions.
This is different from `else` clauses, error codes or state variables which are more about guiding the control flow.
They also allow you to clearly segregate the expected processing of your workflow from the anticipated exceptional cases in your code.

- Raising an exception halts program execution.
- A raised exception will propagate through each method in the call stack until it is stopped, handled 
or reaches the point where the program started.
- Exceptions are just Ruby objects.

```ruby
begin
  nil.some_method
rescue Exception => ex
  p ex.class # NoMethodError
end
```

```ruby
class MyError < RuntimeError; end

MyError.ancestors
#=> [MyError, RuntimeError, StandardError, Exception, Object, Kernel, BasicObject]

begin
  # 'raise' and 'fail' are synonyms
  raise MyError, 'My message'
rescue MyError => ex
  result = :handled
end

result #=> :handled
ex.message #=> 'My message'
```

## rescue

```ruby
begin
  content = load_passport_data(file_name)
rescue PassportDataNotFound
  STDERR.puts "File #{file_name} not found"
rescue PassportDataFormatError
  STDERR.puts "Invalid passport data in #{file_name}"
rescue Exception => e
  STDERR.puts "General error loading #{file_name}: #{e.message}"
end
```

## raise

To raise exceptions we use `raise` or `Kernel.raise` or `Kernel.fail`.
`raise` without any arguments simply raises a `RuntimeError` exception with the string
message that we've specified.

```ruby
raise 'Some Error!'
#=> RuntimeError: Some Error!

raise StandardError, 'Some error'
#=> StandardError: Some error

raise IndexError, 'Strings need to be at lease 3 characters long'
raise ArgumentError, 'Only 5 strings are allowed'
```

## Using of ensure

'ensure' forces execution of some cleanup code we need to run wherher there was an error or not.
This sort of thing often involves closing a connection to a database or 
freeing up some other non-Ruby resource.

```ruby
result = nil
begin
  fail 'Oops'
rescue StandardError
  # no code here
ensure
  result = :always_run
end
```

## Exception class ancestors

```ruby
StandardError.ancestors
#=> [StandardError, Exception, Object, Object, Kernel, BasicObject]

ZeroDivisonError.ancestors
#=> [ZeroDivisionError, StandardError, Exception, Object, Kernel, BasicObject]

SyntaxError.ancestors
#=> [SyntaxError, ScriptError, Exception, Object, Kernel, BasicObject]
```

The larger part of the commonly recoverable exceptions are sub-classed by the StandardError class.
The others are more low, virtual-machine level errors that are less important in a regular course
of the program and are generally not captured.

### StandardError class hierarchy

```ruby
ArgumentError
IOError
  EOFError
IndexError
LocalJumpError
NameError
  NoMethodError
RangeError
  FloatDomainError
RegexpError
RuntimeError
SecurityError
SystemCallError
SystemStackError
ThreadError
TypeError
ZeroDivisionError
```

For more granularity in your exceptions, you can rescue each of these individually.
Rescuring `StandardError` takes care of all the sub-classed exceptions as well.

```ruby
begin
  eval '10 / 0'
rescue StandardError => e
  p e
end
#=> #<ZeroDivisionError: divided by 0>
```

## Custom error

To create custom exception, you can subclass the Exception class.
It's generally a good practice to end your custom exception name with Error.

```ruby
class SomeCustomError < StandardError; end

begin
  raise SomeCustomError, 'Oops, some error'
rescue SomeCustomError => e
  p "message: #{e.message}"
  p "backtrace: #{e.backtrace}"
end
#=> 
"message: Oops, some error"
"backtrace: [\"(repl):3:in `<main>'\", \"/run_dir/repl.rb:45:in `eval'\", \"/run_dir/repl.rb:45:in `run'\", \"/run_dir/repl.rb:61:in `handle_eval'\", \"/run_dir/repl.rb:177:in `start'\", \"/run_dir/repl.rb:184:in `start'\", \"/run_dir/repl.rb:191:in `<main>'\"]"
```

You should raise an exception under cxceptional circumstances, and only then. 
The temptation to use exceptions for flow control is one we all experience. 
Your tools for flow control include if, else, unless, each, for, method calls, and the like.
The reason not to use exceptions for flow control isn't simply one of semantics, either.
Exceptions are designed to provide you, the programmer, with as much information as possible about what went wrong (since they imply a failure).
To do this, Ruby's exception system figures out all the methods you called to get where you are -- and this is slow.
Control flow code should be fast. Exceptions are slow. 

## Throw and catch

`throw` accepts both an identifying symbol and a return value.
The matching `catch` will return this value.

```ruby
toys = ['a', 'b', 'c']

toy = catch(:found) do
  toys.each do |toy|
    throw(:found, toy) if toy == 'b'
  end
end

p toy #=> 'b'
```
