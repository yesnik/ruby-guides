# Logger

Ruby has `Logger` class that we can use for logging.

This class exposes multiple methods - all of which logs the message,
but in the following order of severity:

```
debug < info < warn < error < fatal < unknown.
```

So, if the logging level is set to `warn`, then only levels that have same or higher severity than `warn` are printed:
`warn, error, fatal, unknown`.

The highest state of logging is `DEBUG`, which will print everything.

We can set the severity level of logging by assigning the level:

```ruby
require 'logger'

logger.level = Logger::UNKNOWN
logger = Logger.new($stdout)

logger.unknown 'Strange bug'
logger.error 'This is error'
logger.warn 'This is warning'
logger.info 'This is an info'
logger.debug 'Some debug'
```

Only 'Strange bug' will be logged.

We should use `info` as severity level for normal logs. For exceptions, we use `error`.
