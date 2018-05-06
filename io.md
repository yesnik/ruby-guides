# IO

'Pure' code is code without side-effects: code which simply performs calculations.
But 'pure' program isn't very useful if it can't even pring its results to the screen.
This is where I/O streams come in. Ruby's IO class allows you to initialize these streams.

Open the file 'hi.txt' and create a file descriptor:

```ruby
fd = IO.sysopen('hi.txt', 'w+') #=> 10
fd.class #=> Integer
```

Create a new I/O stream using the file descriptor for 'hi.txt':

```ruby
io = IO.new(fd)
io.write 'hello'
io.close
io.closed? #=> true
```

Then we can see that file 'hi.txt' was created with text 'hello' in it.

Also we can create IO objects using the File class that is subclass of IO:

```ruby
io = File.new(fd)
io.write 'wow'
io.close
```

We can see text 'wow' in file 'hi.txt'.

## Get initialized streams

There are a bunch of I/O streams that Ruby initializes when the interpreter gets loaded.

```ruby
io_streams = []
ObjectSpace.each_object(IO) { |x| io_streams << x }
p io_streams
#=> [#<File:/home/nik/.rvm/rubies/ruby-2.4.1/.irbrc (closed)>, #<IO:<STDERR>>, #<IO:<STDOUT>>, 
    #<IO:<STDIN>>, #<IO:fd 1>, #<IO:fd 0>]
```

### Constants STDOUT, STDIN, STDERR

Ruby defines constants STDOUT, STDIN, STDERR that are IO objects pointing
to your program's input, output, error streams that you can use through your terminal,
without opening any new files. 

```ruby
STDIN.class #=> IO
STDIN.fileno #=> 0

STDOUT.class #=> IO
STDOUT.fileno #=> 1

STDERR.class #=> IO
STDERR.fileno #=> 2
```

Whenever you call 'puts', the output is sent to the IO object that STDOUT points to.
It's the same for 'gets', where the input is captured by the IO object for STDIN.
Method 'warn' directs message to STDERR.

The Kernel module provides us with global variables $stdout, $stdin, $stderr,
which point to the same IO objects that the constants STDOUT, STDIN, STDERR point to:

```ruby
$stdin.object_id == STDIN.object_id #=> true
$stdout.object_id == STDOUT.object_id #=> true
$stderr.object_id == STDERR.object_id #=> true
```

The purpose of these global variables is temporary redirection: you can assign
these global variables to another IO object and pick up an IO stream other than the one
that it is linked to by default. This is sometimes mecessary for logging errors or caputring keyboard input.

```ruby
f = File.open('err.txt', 'w')
$stdout = f
p 'Hello'
quit
```

Now check file 'err.txt', there you'll see text 'Hello'.
