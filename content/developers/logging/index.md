---
title: "Logging"
description: "The YIO logging features are based on the Qt logging framework and fully supports custom message pattern and logging rules."
menu:
  developers:
    parent: "home"
weight: 70
---

The YIO logging features are based on the Qt logging framework and fully supports custom message pattern and logging rules.

Basic logging features can be configured in the YIO `config.json` configuration file. Advanced logging rules are set with Qt's own mechanism through environment variables or configuration files.  
On the remote, the Qt environment variables can be added to `/etc/profile.d/yio.sh`.

- See [Qt configuring categories](https://doc.qt.io/qt-5/qloggingcategory.html#configuring-categories) for configuration options.
- See [KDAB's Qt Logging Framework presentation](https://www.kdab.com/wp-content/uploads/stories/slides/Day2/KaiKoehne_Qt%20Logging%20Framework%2016_9_0.pdf) for more detailed information about the Qt logging framework.
- YIO app implementation class: [Logger.cpp](https://github.com/YIO-Remote/remote-software/blob/master/sources/logging.cpp)

## Features

### Message Patterns

The log message format can be customized with the Qt environment variable `QT_MESSAGE_PATTERN`.  
The message format is documented in [qSetMessagePattern()](https://doc.qt.io/qt-5/qtglobal.html#qSetMessagePattern).  
Default log message format if not set:

    %{time yyyy-MM-dd hh:mm:ss:zzz t} %{if-debug}DEBUG%{endif}%{if-info}INFO %{endif}%{if-warning}WARN
    %{endif}%{if-critical}CRIT %{endif}%{if-fatal}FATAL%{endif} [%{category}]:\t%{message}\t[%{file}:%{line}]

Examples:

- Log message only: `%{message}`
- Log level and message: `%{category} %{message}`

[Terminal escape sequences](http://misc.flogisoft.com/bash/tip_colors_and_formatting) can be used for the console target. This allows to colourize parts of the message or highlight warnings and errors.

Using escape sequences or special characters like tabs can be tricky to correctly escape in an environment variable. The `echo -e` in single back quotes trick can be used:

    export QT_MESSAGE_PATTERN="`echo -e "%{category}\t%{message}"`"

### Logging Rules

Logging rules let you enable or disable logging for categories in a flexible way.
Qt logging rules can be configured in env variable `QT_LOGGING_RULES`.  
The logging rules are documented in [QLoggingCategory](https://doc.qt.io/qt-5/qloggingcategory.html#logging-rules)

ℹ️ The log level configuration in `config.json` in key `settings.logging.level` are mapped to logging rules. The `DEBUG` level doesn't set any logging rules.

If custom rules are defined in `QT_LOGGING_RULES` the log level in `config.json` should be set to `DEBUG`. Otherwise the mapped logging rules might interfere with the custom rules!

#### YIO Logging Categories

The following logging categories are defined in the YIO app. For an up-to-date definition please refer to [logging.cpp](https://github.com/YIO-Remote/remote-software/blob/master/sources/logging.cpp).

```
yio.app
yio.app.i18n
yio.api
yio.plugin
yio.logger
yio.mediaplayer
yio.notification
yio.entity
yio.entity.remote
yio.ota
yio.ota.download
yio.cfg
yio.cfg.json
yio.hw.factory
yio.hw.btnhandler
yio.hw.touchevent
yio.hw.standby
yio.bluetooth
yio.wifi
yio.wifi.mock
yio.wifi.wpactrl
yio.wifi.script
yio.dev.device
yio.dev.apds9960
yio.dev.apds9960.gesture
yio.dev.apds9960.light
yio.dev.apds9960.prox
yio.dev.portexp
yio.dev.haptic
yio.dev.bat.charger
yio.dev.bat.fuelgauge
yio.dev.display
yio.env
yio.env.sysinfo
yio.env.srv
yio.env.srv.systemd
yio.env.srv.mock
yio.env.srv.web
```

❗️ YIO integration plugins are not yet fully using log categories.

Examples:

- Disable debug logging: `yio*.debug=false;qml.debug=false`
- Only disable WiFi debug logging: `yio.wifi*.debug=false`
- Focus on WiFi debugging, only print warning or higher of other categories: `yio*.debug=false;yio*.info=false;qml.debug=false;qml.info=false;yio.wifi*=true`
- Focus on Bluetooth debugging only: `yio*=false;qml*=false;yio.bluetooth*=true;qt.bluetooth*=true`

### Log Targets

Log targets are individual log sinks. The default Qt logging backend can be used, dedicated log files or the YIO WebSocket API.
Multiple log targets can be active at the same time. Targets can be individually enabled and disabled at runtime.

#### Console

This log target enables the default Qt logging backend.

Configuration entry in `config.json`: `settings.logging.console: true | false`

The actual backend depends on the OS and how Qt is configured.  
The backend can be selected with the environment variable `QT_LOGGING_TO_CONSOLE`:

- 0: journald
- 1: console

❗️ On the YIO remote-os v1 the journald backend is not yet working.

#### Log File

Dedicated log files with daily rollover and auto purge function.

Configuration entry in `config.json`: `settings.logging.path: "<LOG_DIRECTORY>"`

- If LOG_DIRECTORY is empty, the log file target is disabled.
- `settings.logging.purgeHours` defines the expiration time when log files shall be purged.
- Log file naming: `yio-YYYY-MM-DD.log.n`
  - Rollover at midnight
  - Rollover at app (re-)start
  - `n`: file counter if multiple log files are created for the same day, e.g. after a reboot or app update.
  - For simplicity reasons the first log file of the day starts with `.0`.

#### Queue

A queue with definable maximum size. It is accessible through the YIO WebSocket API.

Configuration entry in `config.json`: `settings.logging.queueSize: n`

- If queue size `n` is set to 0, the queue target is disabled.
- If the queue is filled up, the oldest entries are automatically purged when new log messages arrive.

## YIO API integration

❗️ The log queue and YIO API logging features are a "left over" from the old logger implementation and are not yet fully functional.

> Planned enhancement: log subscription for asynchronous log events.

Logger functions can be controlled by the YIO API (type is `log`).

Actions :

- `start`, target : (`file`|`console`|`queue`), default is `queue`: starts the log target
- `stop`, target : (`file`|`console`|`queue`), default is `queue`
- `show_source`: enable source information in log messages for queue target
- `hide_source`: disable source information
- `purge`: delete expired log files with expiration time in `days` or `hours`
- `set_log_level`: Either Qt logging rule in `category` or general log level in `level`: (`debug`|`info`|`warning`|`critical`|`fatal`)
- `clear_log_level`: clears all custom log levels set by `set_log_level`
- `get_messages`: _TODO_
- `get_info`  
  Sample result :

      {
          "type": "log",
          "info": {
            "consoleEnabled": true, "fileEnabled": true, "fileCount": 3, "queueEnabled": true, "showSourcePos": true
          },
      }
