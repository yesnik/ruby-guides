# Time

## strftime

See [doc](https://apidock.com/ruby/Time/strftime)

```ruby
t = Time.new(2020,12,1,8,37,48,"-06:00") #=> 2020-12-01 08:37:48 -0600 
t.strftime("%b %-e, %Y") #=> "Dec 1, 2020" 
```
