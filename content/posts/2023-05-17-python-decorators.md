---
title: "Python Decorators"
date: 2023-05-17T14:28:35+02:00
summary: "Examples to quickly learn and use python decorators"
tags: ["python", "decorators"]
---

## Simple decorator

Decorating the function `flower`, to print `red roses`

```python
def decorate(func):
    def _wrapper():
        return "red %s" % func()
    return _wrapper

@decorate
def flower():
    return "roses"


print(flower)
# <function decorate.<locals>._wrapper at 0x7fdcd850c700>

print(flower())
# red roses
```


## Decorate a function with parameters

Decorating the function flowers, which receive the flower name as a parameter.

```python
def decorate(func):
    def _wrapper(name):
        return "red %s" % func(name)
    return _wrapper

@decorate
def flower(name):
    return name


print(flower)
# <function decorate.<locals>._wrapper at 0x7fdcd850c700>

print(flower("dahlias"))
# red dahlias
```


## Passing data to the decorator function

The `color` will be available in the inner scopes when creating the decorator .

```python
def create_decorator(color):
    def _decorate(func):
        def _wrapper(name):
            return f"{color} {func(name)}"
        return _wrapper
    return _decorate

@create_decorator("blue")
def flower(name):
    return name


print(flower)
# <function create_decorator.<locals>._decorate.<locals>._wrapper at 0x7f360530c790>

print(flower("bluebell"))
# blue bluebell
```


## References

* https://www.geeksforgeeks.org/decorators-in-python/
