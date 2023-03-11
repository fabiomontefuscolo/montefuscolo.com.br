---
title: "Quick JavaScript OOP"
date: 2020-12-30T07:22:16-03:00
summary: "Basic JavaScript syntax to write classes, methods, properties and inheritance"
tags: ["javascript"]
---


### Defining a class

```js
class Employee {

}
```


### Defining the constructor

```js
class Employee {
    constructor () {
    }
}
```


### Adding properties

```js
class Employee {
    constructor (name, salary) {
        this._name = name;
        this._salary = salary;
        this.bonus = 1.05;
    }
}
```


### Creating a method

```js
class Employee {
    constructor (name, salary) {
        this._name = name;
        this._salary = salary;
        this.bonus = 1.05;
    }
    calculatePayment () {
        return this._salary * this.bonus;
    }
}
```


### Static properties

```js
class Employee {
    static counter = 0;

    constructor (name, salary) {
        this._name = name;
        this._salary = salary;
        this.bonus = 1.05;

        Employee.counter += 1;
    }
    calculatePayment () {
        return this._salary * this.bonus;
    }
}
```


### Getters and setters

```js
class Employee {
    static counter = 0;

    constructor (name, salary) {
        this._name = name;
        this._salary = salary;
        this.bonus = 1.05;

        Employee.counter += 1;
    }
    calculatePayment () {
        return this._salary * this.bonus;
    }
    get name () {
        return this._name;
    }
    set name (novoNome) {
        this._name = novoNome;
    }
}
```


### Inheritance

```js
class Manager extends Employee {
    constructor (name, salary) {
        super(name, salary);
        this.bonus = 1.07;
    }
}
class Diretor extends Employee {
    constructor (name, salary) {
        super(name, salary);
        this.bonus = 1.09;
    }
}

```


### Overwrite method

```js
class Diretor extends Employee {
    constructor (name, salary) {
        super(name, salary);
        this.bonus = 1.09;
    }
    calculatePayment (additional=0) {
        return super.calculatePayment() + additional;
    }
}
```


### Simulate abstract class

```js
class MyClass {
    constructor () {
        if (this.constructor === MyClass) {
            throw new Error('"MyClass" is abstract');
        }
    }
}
```


### Simulate abstract method

```js
class MyClass {
    constructor () {
        if (this.constructor === MyClass) {
            throw new Error('"MyClass" is abstract');
        }
    }
    meuMetodo () {
        throw new Error('Not implemented');
    }
}
```


### References

* https://cursos.alura.com.br/course/javascript-polimorfismo
* https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes