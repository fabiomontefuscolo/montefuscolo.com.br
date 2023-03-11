---
title: "Sed How to Delete Lines"
date: 2013-07-18T20:46:16-03:00
draft: true
summary: "Some sed commands demonstrating how to delete lines and range of lines in a text file"
tags: ["linux", "sed"]
---

The following creates a text file to be used by subsequent examples.

```shell
[~] cat > nomes.txt <<EOF
Guilherme Sofia
Gustavo Maria
Lucas Beatriz
Enzo Camila
Vinicius Amanda
Joao Bruna
Eduardo Isabela
Bruno Ana
EOF
```

### Deleting lines

#### Delete line number 2
```shell
[~] sed -e '2d' nomes.txt
Guilherme Sofia
Lucas Beatriz
Enzo Camila
Vinicius Amanda
Joao Bruna
Eduardo Isabela
Bruno Ana
```

#### Delete from line 1 to line 3 (inclusive)
```shell
[~] sed -e '1,3d' nomes.txt
Enzo Camila
Vinicius Amanda
Joao Bruna
Eduardo Isabela
Bruno Ana
```

#### Delete from line 3 until the end of the file
```shell
[~] sed -e '3,$ d' nomes.txt
Guilherme Sofia
Gustavo Maria
```

#### Delete lines having the first name ending with the char **o**
```shell
[~] sed -e '/^\w\w*o /d' nomes.txt
Guilherme Sofia
Lucas Beatriz
Vinicius Amanda
```

#### Delete from the line starting with **Lucas** to line ending with **Amanda**
```shell
[~] sed -e '/^Lucas/,/Amanda$/d' nomes.txt
Guilherme Sofia
Gustavo Maria
Joao Bruna
Eduardo Isabela
Bruno Ana
```

#### Delete lines ending with **Beatriz** and the next line to it
```shell
[~] sed -e '/Beatriz$/{ N; d }' nomes.txt
Guilherme Sofia
Gustavo Maria
Vinicius Amanda
Joao Bruna
Eduardo Isabela
Bruno Ana
```

#### References
* http://www.grymoire.com/Unix/Sed.html