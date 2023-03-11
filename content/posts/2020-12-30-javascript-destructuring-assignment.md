---
title: "JavaScript destructuring assignment"
date: 2020-12-30T07:22:16-03:00
draft: true
summary: "This feature of JavaScript allows the programmer to unpack array and object values to assign them to variables and function parameters."
tags: ["javascript"]
---

### Assignments

#### Array to scalars

```js
const [ weight, age ] = [ 70, 36 ];
console.log(weight)
console.log(age)
```

output
```
70
36
```

#### Array to scalars and another array

Assing the first two array elements to the **weight** and **age** constants. The remaining array elements are assigned to the **remaining** constant.

```js
const [ weight, age, ...remaining ] = [ 70, 36, 170, 1984 ]
console.log(weight)
console.log(age)
console.log(remaining)
```

output
```
70
36
[ 170, 1984 ]
```

#### Object properties to scalars

It also works with object properties. But instead of using `[]` in the left hand, the `{}` should be used.

```js
const { weight, age } = { weight: 70, age: 36 }
console.log(weight)
console.log(age)
```

output
```
70
36
```

#### Object properties to scalars and another object

Assign the properties' **weight** and **age** to the constants with the same name. The remaining properties will be part of the **remaining** object.

```js
const { weight, age, ...remaining } = {
    "weight": 70,
    "age": 36,
    "height": 170,
    "birth_year": 1984
}
console.log(weight)
console.log(age)
console.log(remaining)
```

output
```
70
36
{ height: 170, birth_year: 1984 }
```

#### Create array from arrays

Create the **array_3** by unpacking elements from **array_1** and **array_2** into a new array.

```js
const array_1 = [ 70, 36 ]
const array_2 = [ "setenta", "trintaeseis" ]
const array_3 = [...array_1, ...array_2 ]
console.log(array_3)
```

output
```
[ 70, 36, 'setenta', 'trintaeseis' ]
```

#### Create an object from other objects

Create the **obj3** by unpacking properties from **obj1** and **obj2** into a new object.

```js
const obj1 = { "age": 36, "weight": 70 }
const obj2 = { "name": "Fabio" }
const obj3 = { "role": "Janitor", ...obj1, ...obj2 }
console.log(obj3)
```

output
```
{ role: 'Janitor', age: 36, weight: 70, name: 'Fabio' }
```


### Function parameters

#### Array to position parameters

Array elements will be used as positional parameters for the function.

```js
f = function([age, weight]) {
    console.log("age", age);
    console.log("weight", weight);
}

f([ 36, 70 ])
```

output
```
age 36
weight 70
```

#### Object properties to keyword arguments

Object properties will be used as keyword arguments for the function

```js
f = function ({ age, weight }) {
    console.log("weight", weight);
    console.log("age", age);
}

f({ "weight": 70, "age": 36 })
```

output
```
weight 70
age 36
```

### References

* https://cursos.alura.com.br/destructuring-em-js-c308
  * https://github.com/juunegreiros
* https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment