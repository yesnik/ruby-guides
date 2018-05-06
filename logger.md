# Logger

Ruby ships with the Logger class that you can use for logging:

This class exposes multiple methods - all of which logs the message,
but in the following order of severity:

```
debug < info < warn < error < fatal < unknown.
```

So, if the logging level is set to WARD,
then only levels that have save or higher severity than WARN are printed:
WARN, ERROR, FATAL, UNKNOWN.

The highest state of logging is DEBUG, which will print everything.

We can set the severity level of logging by assigning the level like so:

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

We should use info as severity level for normal logs. For exceptions, use error.
