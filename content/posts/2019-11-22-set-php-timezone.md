---
title: "Set PHP timezone"
date: 2019-11-22T12:58:16-03:00
summary: "Set a time in PHP configuration file to correctly display date and time information"
tags: ["php"]
---

### Check current timezone

```shell
$ php -r "echo ini_get('date.timezone');"
>>>
```


### Check what date PHP would print (on cli)

```shell
$ php -r "echo date('r');"
>>> Fri, 22 Nov 2019 14:28:03 +0000
```


### Set the timezone in the PHP config file

Set the value `America/Sao_Paulo` in the directive `date.timezone`. The config file location varies according to the PHP installation method.

```shell
$ echo date.timezone=America/Sao_Paulo | sudo tee -a /etc/php/conf.d/my-customs.ini
```


### Check the timezone and current date again

```shell
$ php -r "echo ini_get('date.timezone');"
>>> America/Sao_Paulo

$ php -r "echo date('r');"
>>> Fri, 22 Nov 2019 11:51:25 -0300
```


### References
* https://www.php.net/manual/en/datetime.configuration.php#ini.date.timezone
* https://www.php.net/manual/en/function.date.php
* https://www.php.net/manual/en/timezones.php