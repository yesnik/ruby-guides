# Hash

```ruby
h = Hash.new
h.class #=> Hash
h.size #=> 0
```

We need to use the arrow syntax whenever the key is not a symbol:

```ruby
# BAD
h = {'5': [1, 2]}
h[5] #=> nil
h['5'] #=> nil
h[:'5'] #=> [1, 2]

# GOOD
h = {5 => [1, 2]}
h[5] #=> [1, 2]
```

```ruby
{'kenny' => 20} #=> {"kenny"=>20}
{'kenny': 20} #=> {:kenny=>20}
{kenny: 20} #=> {:kenny=>20}

{'joe doe' => 20} #=> {"joe doe"=>20}
{:'joe doe' => 20} #=> {:"joe doe"=>20}
{'joe doe': 20} #=> {:"joe doe"=>20}
```

### Array to hash

```ruby
arr = [['a', 1], ['b', 2]]
p Hash[ arr ]
#=> {"a"=>1, "b"=>2}
```

We can use asterisk `*` to convert plain array to arguments:

```ruby
arr = ['a', 11, 'b', 22]
Hash[*arr] #=> {'a'=>11, 'b'=>22}

arr2 = ['a', 11, 'b']
Hash[*arr2] #=> ArgumentError: odd number of arguments for Hash

[ ['a', 11], ['b', 22] ].to_h
#=> {'a'=>11, 'b'=>22}
```

### .delete

```ruby
h = {a: 11, b: 22}
h.delete(:a) #=> 11
h #=> {:b=>22}
```

## Non-destructive selection

### .select

```ruby
h = {a: 11, b: 22, c: 0}
h.select { |k, v| v > 0 } #=> {a: 11, b: 22}
h #=> {a: 11, b: 22, c: 0}
```

### .reject

```ruby
h = {a: 11, b: 22, c: 0}
h.reject { |k, v| v.zero? } #=> {a: 11, b: 22}
h #=> {a: 11, b: 22, c: 0}
```

### .drop_while

```ruby
h = {a: 11, b: 22, x: 0, y: 5}
h.drop_while { |k, v| v > 0 } #=> [[:x, 0], [:y, 5]]
h #=> {a: 11, b: 22, x: 0, y: 5}
```


## Destructive selection

### .delete_if

```ruby
h = {a: 11, b: 22, x: 0}
h.delete_if { |k, v| v.zero? } #=> {a: 11, b: 22}
h #=> {a: 11, b: 22}
```

### .keep_if

```ruby
h = {a: 11, b: 22, x: 0}
h.keep_if { |k, v| v.zero? } #=> {x: 0}
h #=> {x: 0}
```

### Get hash elements

```ruby
h = {a: 1, b:2}
h.size #=> 2
h[:b] #=> 2
h[:some_key] #=> nil
h.fetch(:a) #=> 1
h.fetch(:zzz, 'Kenny') #=> 'Kenny'

h.keys #=> [:a, :b]
h.values #=> [1, 2]
```

### Set hash elements

```ruby
h = {}
h[:name] => 'Kenny'
h #=> {:name=>'Kenny'}
```

### Default hash value

```ruby
h = Hash.new('Kenny')
h[:some] #=> 'Kenny'

h = Hash.new
h.default = 99
h[:a] #=> 99

h = Hash.new { |hash, key| hash[key] = [] }
h[:a] = 11
h[:b] << 22
h[:c]
h #=> {a: 11, b: [22], c: []}
```

### Modify default value

```ruby
h = Hash.new([])
h[:one] << 'one' #=> ['one']
h[:two] << 'two' #=> ['one', 'two']
h #=> {}
h[:zzz] #=> ['one', 'two']
h[:xxx] #=> ['one', 'two']
```

### Merge

```ruby
h = {a: 1, b: 2}
params = h.merge(b: 22, c: 33) #=> {a: 1, b: 22, c: 22}
h #=> {a: 1, b: 2}

h = {a: 1, b: 2}
params = h.merge!(b: 22, c: 33)
h #=> {a: 1, b: 22, c: 33}
```

### Comparison

```ruby
h = {a: 1, b: 2}
h2 = {b: 2, a: 1}
h == h2 #=> true
```

### Iterating over a Hash

```ruby
h = {a: 11, b: 22}
h.each { |k, v| p "#{k} -- #{v}" }
#=> 'a -- 11'
#=> 'b -- 22'
```

```ruby
h = {a: 11, b: 22}
h.each { |x| p "#{x[0]} -- #{x[1]}" }
#=> 'a -- 11'
#=> 'b -- 22'
```
