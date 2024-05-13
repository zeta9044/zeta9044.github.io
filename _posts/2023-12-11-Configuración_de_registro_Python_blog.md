---
layout: post
title: "Configuración de registro Python"
tags: [de, Python, registro]
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

1. Log only {% raw %}`INFO`{% endraw %} and {% raw %}`ERROR`{% endraw %} messages to the file for the {% raw %}`INFO`{% endraw %} level.
2. Log only {% raw %}`ERROR`{% endraw %} messages to the file for the {% raw %}`ERROR`{% endraw %} level.

Here is a revised version of your logging configuration to achieve this:

{% raw %}```yaml
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
```{% endraw %}

Key Changes:

- Added two separate file handlers: {% raw %}`info_file`{% endraw %} and {% raw %}`error_file`{% endraw %}.
- {% raw %}`info_file`{% endraw %} is set to log {% raw %}`INFO`{% endraw %} level and above (which includes {% raw %}`ERROR`{% endraw %}).
- {% raw %}`error_file`{% endraw %} is set to log only {% raw %}`ERROR`{% endraw %} level messages.
- Each logger ({% raw %}`transform_db2xml`{% endraw %}, {% raw %}`db2xml`{% endraw %}, {% raw %}`xml2db`{% endraw %}) now has both {% raw %}`info_file`{% endraw %} and {% raw %}`error_file`{% endraw %} handlers along with the console handler.

This configuration ensures that {% raw %}`INFO`{% endraw %} and {% raw %}`ERROR`{% endraw %} messages are logged in the {% raw %}`info.log`{% endraw %} file, while only {% raw %}`ERROR`{% endraw %} messages are logged in the {% raw %}`error.log`{% endraw %} file.

## 사용자 입력:
> 그리고, 로그파일 생길때, 이어 붙이는게 아니라 실행할때 마다 새로 받음.

## ChatGPT 응답:
> To create a new log file each time your application runs instead of appending to the existing log files, you should change the file handler class from {% raw %}`TimedRotatingFileHandler`{% endraw %} to a different handler that supports creating a new file on each run. A suitable choice could be {% raw %}`logging.FileHandler`{% endraw %}, which will create a new file each time it's instantiated if you set the {% raw %}`mode`{% endraw %} to {% raw %}`'w'`{% endraw %} (write mode).

Here's how to adjust your configuration:

{% raw %}```yaml
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
```{% endraw %}

Key Changes:

- Replaced {% raw %}`TimedRotatingFileHandler`{% endraw %} with {% raw %}`logging.FileHandler`{% endraw %}.
- Set the mode to {% raw %}`'w'`{% endraw %} for both {% raw %}`info_file`{% endraw %} and {% raw %}`error_file`{% endraw %} to ensure that logs are written to a new file on each run.

This configuration ensures that each time your application starts, it creates new {% raw %}`info.log`{% endraw %} and {% raw %}`error.log`{% endraw %} files, instead of appending to existing ones. Keep in mind that this will overwrite the previous log files, so make sure this is the intended behavior.

