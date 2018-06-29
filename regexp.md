# Regexp

```ruby
/hi/.class #=> Regexp

a = 'hi kenny'
a[/ken/] #=> 'ken'
a[/ken2/] #=> nil
a[/n/] #=> 'n'
a[/kes?/] #=> 'ke'

'abcca'[/bc+/] #=> 'bcc'
'abbc'[/ab*/] #=> 'abb'
'accc'[/ab*/] #=> 'a'

'aaa'[/b*/] #=> ''
'zzza'[/z*/] #=> 'zzz'
'zzza'[/z*?/] #=> ''

animals = ['cat', 'bat', 'rat', 'zat']
animals.select { |x| x[/[cbr]at/] } #=> ['cat', 'bat', 'rat']

'num 15'[/\d+/] #=> '15'
'num 15'[/[0-9]+/] #=> '15'

"space: \t\n"[/\s+/] #=> " \t\n"

'var_1 = 15'[/\w+/] #=> 'var_1'
```

## Interpolating a string into a regex

```ruby
a = 'Kenny'
a =~ /nny/ #=> 2

str = 'nny'
a =~ /#{str}/ #=> 2
```

## .scan

This method allows us to get an array of matching results:

```ruby
'user 555 pass 123'.scan(/\d+/) #=> ['555', '123']
```

### Substitute first 

```ruby
'abca'.sub('a', '_') #=> '_bca'
'abca'.sub!('a', '_') #=> '_bca'
```

### Substitute global

```ruby
'abca'.gsub('a', '_') #=> '_bc_'
'abca'.gsub!('a', '_') #=> '_bc_'

'Arny Hello'.gsub(/[A-Z]/, '*') #=> '*rny *ello'

'one two-three'.sub(/(\w*)/) { "num=#{$1}" }
#=> "num=one two-three"

'one two-three'.gsub(/(t\w*)/) { $1[0, 1] }
```

### b anchors to a word boundary

```ruby
'xhay hello'[/\bhe.+/] #=> 'hello'
```

### Period is a shortcut for any non newline character

```ruby
"abc\n123"[/a.+/] #=> 'abc'

'the num 15'[/[^0-9]+/] #=> 'the num '
'the num is 15'[/\D+/] #=> 'the num is '

'space: \t\n'[/\S+/] #=> 'space:'
'var_1 = 42'[/\W+/] #=> ' = '
```

### Start of the string

```ruby
'start end'[/\Astart/] #=> 'start'
'start end'[/\Aend/] #=> nil

"num 20\n30 num"[/^\d+/] #=> '30'
```

### End of the string

```ruby
'start end'[/end\z/] #=> 'end'
'start end'[/start\z/] #=> nil

"num 20\n30 num"[/\d+$/] #=> '30'
```

### Parentheses group contents

```ruby
'ahahaha'[/(ha)+/] #=> 'hahaha'
'cat, dog'[/(\w+), (\w+)/, 1] #=> 'cat'
'cat, dog'[/(\w+), (\w+)/, 3] #=> nil
$1 #=> 'cat'
$2 #=> 'dog'
```

## .match

```ruby
'hello kenny'.match(/ken/) #=> #<MatchData 'ken'>
```

## .match?

```ruby
/wo/.match? 'world' #=> true
/wod/.match? 'world' #=> false
```
