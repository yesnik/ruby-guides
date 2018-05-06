# Float

```ruby
1.5.class #=> Float
Float.ancestors
#=> [Float, Numeric, Comparable, Object, Kernel, BasicObject]

5e2 #=> 500.0
5e-2 #=> 0.05
5e14 #=> 500000000000000.0
5e15 #=> 5.0e+15
```

## Omit decimal part if needed

```ruby
n = 5.2
(n % 1 == 0) ? n.to_i : n #=> 5.2

n = 5.0
(n % 1 == 0) ? n.to_i : n #=> 5
```
