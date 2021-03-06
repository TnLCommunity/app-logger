# app-logger

This is a drop in logger that uses [python-json-logger](https://github.com/madzak/python-json-logger) to log json to stdout and stderr appropriately. DEBUG and INFO go to stdout, WARNING, ERROR, CRITICAL go to stderr.

# Installation
`pip install app-logger`

# Configuration
All configuration is through environment variables, all have defaults so none are required.

## LOGGER_NAME
What should the logger be called? Default will use the root logger. Using the default or `root` will log not only log statements from `app_logger` but also any statements from libraries you're using.

**Format**: Just a regular string

**Default**: `root`

**Example**: `mysweetapp`

## LOG_LEVELS
Used to set log levels. Can be used to set any log level you wish so you can selectively control logging. May be upper or lower case.

**Format**: A comma separated string of key=value pairs

**Default**: `root=INFO`

**Example**: `root=DEBUG,asyncio=INFO`

## LOG_FORMAT
Used to control the log format.

**Format**: A log format string of [log record attributes](https://docs.python.org/3.7/library/logging.html#logrecord-attributes)

**Default**: `%(levelname)%(name)%(asctime)%(module)%(funcName)%(lineno)%(message)`

## Usage
The logger initializes itself and makes itself available as a variable called `app_logger`. It's a regular python logger and can be used as such.

**Example** `app_logger.info("I always hated python logging but now it's easy and just works")`

**Resulting log** `{"levelname": "INFO", "name": "root", "asctime": "2020-04-11 11:24:17,299", "module": "main", "funcName": "main", "lineno": 14, "message": "I always hated python logging but now it's easy and just works"}`

### Adding context
You can use the `extra=` feature of python-json-logger to add context to your messages. This makes it really easy to have logging that's easy to parse, search, learn, and alert on if you're using log aggregation.

**Example**
Given you have something that's a dictionary, you can include it in log statements without verbose string formatting.
`app_logger.error('Error handling message', extra=message)`

**Resulting log**
`{"levelname": "ERROR", "name": "root", "asctime": "2020-04-11 11:27:36,584", "module": "main", "funcName": "main", "lineno": 16, "message": "Error handling message", "guid": "7cc81eba-3bbc-4555-8fab-2a7556072f5d", "subject": "test"}
`

You can see how easy it would be to find logs like this in kibana, and it's already in json format so it's easily indexable/searchable, including the context given via `extra=`