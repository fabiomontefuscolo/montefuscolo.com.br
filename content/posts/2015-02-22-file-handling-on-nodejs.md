---
title: "File handling on NodeJS"
date: 2015-02-22T12:21:16-03:00
summary: "List files from a directory and read files' content using NodeJS."
tags: ["nodejs", "javascript"]
---

### Reading file synchronously
```js
// countLines.js
let fs = require('fs');
let filename = process.argv[2]; // parametro
let buffer = fs.readFileSync(filename);
let str = buffer.toString();
console.log(str.split('\n').length - 1);
```

### Reading file asynchronously
```js
// countLinesAsync.js
var fs = require('fs');
var filename = process.argv[2];
fs.readFile(filename, function(err, data) {
    var str = data.toString();
    console.log(str.split('\n').length - 1);
});
```

### Listing files from a directory
```js
// ls.js
var fs = require('js');
var path = require('path');
var dirPath = process.argv[2];
var ext = '.' + process.argv[3];

fs.readdir(dirPath, function (err, list) {
    if (err) {
        return;
    }
    var files = list.filter(function(f) {
        return path.extname(f) === ext;
    });
    console.log(files.join('\n'));
});
```

### References
* http://nodeschool.io/#workshopper-list
* https://github.com/rvagg/learnyounode