# Integer

```ruby
5.class #=> Integer
Integer.ancestors
#=> [Integer, Numeric, Comparable, Object, Kernel, BasicObject]

1.next #=> 2
5.pred #=> 4
3.odd? #=> true
4.even? #=> true
2.integer? #=> true
(-5).abs #=> 5

5.between?(4, 6) #=> true
5.between? (4, 5) #=> true
5.between? (6, 7) #=> false

10 / 3 #=> 3
10.0 / 3 #=> 3.3333333333333335

1.upto(3).class #=> Enumerator
1.upto(3).map(&:to_i) #=> [1, 2, 3]
3.downto(1).map(&:to_i) #=> [3, 2, 1]
```
