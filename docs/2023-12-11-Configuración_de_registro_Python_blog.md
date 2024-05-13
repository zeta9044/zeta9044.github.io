---
layout: post
title: "Configuración de registro Python"
date: 2023-12-11
tags: [registro, Python, de]
categories: blog
---

## 사용자 입력:
> version: 1
# disable_existing_loggers: True
formatters:
  simple:
    format: '%(asctime)s - %(name)s - %(levelname)s - %(funcName)s - %(message)s'
handlers:
  console:
    class: logging.StreamHandler
    level: DEBUG
    formatter: simple
    stream: ext://sys.stdout
  db2xml_file:
    class: logging.handlers.TimedRotatingFileHandler
    level: DEBUG
    formatter: simple
    when: D
    backupCount: 7
    encoding: utf-8
    filename: ./log/db2xml.log
    #filters: [db2xml_filter]
  xml2db_file:
    class: logging.handlers.TimedRotatingFileHandler
    level: DEBUG
    formatter: simple
    when: D
    backupCount: 7
    encoding: utf-8
    filename: ./log/xml2db.log
    #filters: [xml2db_filter]    
loggers:
  transform_db2xml:
    level: DEBUG
    handlers: [console,db2xml_file]
    propagate: no
  db2xml:
    level: DEBUG
    handlers: [console,db2xml_file]
    propagate: no
  xml2db:
    level: DEBUG
    handlers: [console,xml2db_file]
    propagate: no       
filters:
  db2xml_filter: 
    name: db2xml
  xml2db_filter: 
    name: xml2db

root:
  level: INFO
  handlers: [console]   이건 파이썬 로깅 설정 yml이야. 그런데,   INFO는 INFO와 ERROR만 포함하고, ERROR은 ERROR만 파일로 생성하고 싶어.I

## ChatGPT 응답:
> To modify your Python logging configuration to meet your requirements, you'll want to adjust the logging levels and handlers. Based on your description, you want to:

1. Log only `INFO` and `ERROR` messages to the file for the `INFO` level.
2. Log only `ERROR` messages to the file for the `ERROR` level.

Here is a revised version of your logging configuration to achieve this:

```yaml
version: 1
formatters:
  simple:
    format: '%(asctime)s - %(name)s - %(levelname)s - %(funcName)s - %(message)s'
handlers:
  console:
    class: logging.StreamHandler
    level: DEBUG
    formatter: simple
    stream: ext://sys.stdout
  info_file:
    class: logging.handlers.TimedRotatingFileHandler
    level: INFO
    formatter: simple
    when: D
    backupCount: 7
    encoding: utf-8
    filename: ./log/info.log
  error_file:
    class: logging.handlers.TimedRotatingFileHandler
    level: ERROR
    formatter: simple
    when: D
    backupCount: 7
    encoding: utf-8
    filename: ./log/error.log
loggers:
  transform_db2xml:
    level: DEBUG
    handlers: [console, info_file, error_file]
    propagate: no
  db2xml:
    level: DEBUG
    handlers: [console, info_file, error_file]
    propagate: no
  xml2db:
    level: DEBUG
    handlers: [console, info_file, error_file]
    propagate: no

root:
  level: INFO
  handlers: [console]
```

Key Changes:

- Added two separate file handlers: `info_file` and `error_file`.
- `info_file` is set to log `INFO` level and above (which includes `ERROR`).
- `error_file` is set to log only `ERROR` level messages.
- Each logger (`transform_db2xml`, `db2xml`, `xml2db`) now has both `info_file` and `error_file` handlers along with the console handler.

This configuration ensures that `INFO` and `ERROR` messages are logged in the `info.log` file, while only `ERROR` messages are logged in the `error.log` file.

## 사용자 입력:
> 그리고, 로그파일 생길때, 이어 붙이는게 아니라 실행할때 마다 새로 받음.

## ChatGPT 응답:
> To create a new log file each time your application runs instead of appending to the existing log files, you should change the file handler class from `TimedRotatingFileHandler` to a different handler that supports creating a new file on each run. A suitable choice could be `logging.FileHandler`, which will create a new file each time it's instantiated if you set the `mode` to `'w'` (write mode).

Here's how to adjust your configuration:

```yaml
version: 1
formatters:
  simple:
    format: '%(asctime)s - %(name)s - %(levelname)s - %(funcName)s - %(message)s'
handlers:
  console:
    class: logging.StreamHandler
    level: DEBUG
    formatter: simple
    stream: ext://sys.stdout
  info_file:
    class: logging.FileHandler
    level: INFO
    formatter: simple
    mode: 'w'
    encoding: utf-8
    filename: ./log/info.log
  error_file:
    class: logging.FileHandler
    level: ERROR
    formatter: simple
    mode: 'w'
    encoding: utf-8
    filename: ./log/error.log
loggers:
  transform_db2xml:
    level: DEBUG
    handlers: [console, info_file, error_file]
    propagate: no
  db2xml:
    level: DEBUG
    handlers: [console, info_file, error_file]
    propagate: no
  xml2db:
    level: DEBUG
    handlers: [console, info_file, error_file]
    propagate: no

root:
  level: INFO
  handlers: [console]
```

Key Changes:

- Replaced `TimedRotatingFileHandler` with `logging.FileHandler`.
- Set the mode to `'w'` for both `info_file` and `error_file` to ensure that logs are written to a new file on each run.

This configuration ensures that each time your application starts, it creates new `info.log` and `error.log` files, instead of appending to existing ones. Keep in mind that this will overwrite the previous log files, so make sure this is the intended behavior.

