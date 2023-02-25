---
title: "Creating a Django development environment"
date: 2014-08-21T20:46:16-03:00
draft: true
summary: "Setup a Django 1.6 development environment using python virtualenv."
---

## Install basic tools

```shell
$ sudo pacman -S      \
    python-pip        \
    python-virtualenv \
    python-virtualenvwrapper
```

## Load virtualenvwrapper

```shell
$ source /usr/bin/virtualenvwrapper.sh
$ echo /usr/bin/virtualenvwrapper.sh >> ~/.bashrc
```


## Create an env for a project called **pao_na_chapa**

```shell
$ mkvirtualenv pao_na_chapa -p /usr/bin/python2
```


## Install Django

```shell
$ pip install Django==1.6
```

## Create a Django project

```shell
(pao_na_chapa)$ django-admin startproject pao_na_chapa
(pao_na_chapa)$ cd pao_na_chapa
(pao_na_chapa)$ ./manage.py syncdb --noinput
(pao_na_chapa)$ ./manage.py runserver
```

At this point, https://127.0.0.1:8000 should be accessible in the browser


## Activating the **virtualenv**

It should be executed for every new shell session

```shell
$ workon pao_na_chapa
```


## References

* https://docs.djangoproject.com/en/1.6/
* https://pip.pypa.io/en/latest/installing.html#using-package-managers
* http://virtualenvwrapper.readthedocs.org/en/latest/