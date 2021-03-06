# How to log messages to check the execution of my scripts?

There are actually two ways to do this:

## Write to the logs

- Open your [workspace]("https://www.scriptr.io/workspace") and click on "New Script".
- In the script, require the **log** module, then specify the **log level** you desire using **log.setLevel()**. There are four log levels: 
info, error, warn and debug (or "INFO", "ERROR", "WARN", "DEBUG")
- To write to the logs, use on of the available methods: **log.debug()**, **log.info()**, **log.warn()**, **log.error()**

```
var log = require("log");
log.setLevel("info");

log.debug("some debug data");
log.info("some info");
log.warn("some warning");
log.error("some error");
```
**Note**: you can display objects or data structures in the logs. You first have to stringify them (JSON.stringify(data))

### Log levels

As in any othr log system, the log level defines the verbosity, i.e. what appears in the log and what doesn't. Hence:
- If the level is set to "debug", any call to log.debug(), log.info(), log.warn(), log.error() results in a log entry
- If the level is set to "info", any call to log.info(), log.warn(), log.error() results in a log entry, log.debug() is ignored
- If the level is set to "warn", any call to log.warn(), log.error() results in a log entry, log.debug(), log.info() are ignored
- If the level is set to "error", only calls to log.error() result in a log entry, log.debug(), log.info() and log.warn() are ignored

### How to see the logs

From the [workspace]("https://www.scriptr.io/workspace"), click on "Logs". This opens the logs file in a new tab. 

![Open Logs](./images/logs.png)

*Image 1*

Logs are listed by script execution and sorted by date in descending order. Click on a row to see the log messages for the corresponding script execution.

![View logs](./images/log_file.png)

*Image 2*

## Write to the console

Another option is to write to the console, which actually adds logs to the **scriptLog** field of the metadata section of the response returned by your script:

```
{"response": {
  "metadata": {
    "requestId": "8b53e8cb-67e6-40f3-ab33-76cbb0f15b7f",
    "status": "success",
    "scriptLog": [
      {
        "timestamp": "2018-10-24 06:41:29.253",
        "level": "log",
        "component": "log",
        "message": "this is a message"
      }
    ],
    "statusCode": "200"
  },"result": null
}}
```

When running the script from the [workspace]("https://www.scriptr.io/workspace"), the logs are displayed in the console panel at the bottom of the screen:

![Log to the Console](./images/console_log.png)

*Image 3*
