---
title: "Beginning with Python and Pandas"
date: 2023-05-17T14:28:35+02:00
summary: "Getting started with Python Pandas while playing with it on Leetcode"
tags: ["python", "pandas", "data"]
---

### Creating a dataframe

A dataframe is a 2d table-like structure, similar to a database table.

```python
import pandas as pd

columns = ["product_id", "price"]
data = [
    [1, 50],
    [2, 55],
    [3, 30],
    [4, 32],
    [5, 44],
]
frame = pd.DataFrame(data, columns=columns)

print(frame)
# Output
#
#    product_id  price
# 0           1     50
# 1           2     55
# 2           3     30
# 3           4     32
# 4           5     44
```

### Dataframe size

The `shape` property of the `DataFrame` object holds information about number of rows and columns in the dataframe.

```python
import pandas as pd

columns = ["product_id", "price"]
data = [
    [1, 50],
    [2, 55],
    [3, 30],
    [4, 32],
    [5, 44],
]
frame = pd.DataFrame(data, columns=columns)

rows = frame.shape[0]
cols = frame.shape[1]

print("row: {0}, columns: {1}".format(rows, cols))
# row: 5, columns: 2
```

### Slicing a dataframe

```python
import pandas as pd

columns = ["product_id", "price"]
data = [
    [1, 50],
    [2, 55],
    [3, 30],
    [4, 32],
    [5, 44],
]
frame = pd.DataFrame(data, columns=columns)

## first two rows
frame[0:2]
#    product_id  price
# 0           1     50
# 1           2     55

## also first two rows
frame.head(2)
#    product_id  price
# 0           1     50
# 1           2     55

## last two rows
frame[-2:]
#    product_id  price
# 3           4     32
# 4           5     44

## also last two rows
frame.tail(2)
#    product_id  price
# 3           4     32
# 4           5     44

## from the second to before last
frame[1:-1]
#    product_id  price
# 1           2     55
# 2           3     30
# 3           4     32
```

### Selecting/searching data

The property `loc` allows to search and select data inside the frame.

```python
import pandas as pd

frame = pd.DataFrame(
    [
        [1, 50],
        [2, 55],
        [3, 30],
        [4, 32],
        [5, 44],
    ],
    columns=["product_id", "price"],
)

## Search for product_id == 2
frame.loc[frame["product_id"] == 2]
#    product_id  price
# 1           2     55

## Search for product_id == 2 and select the price
frame.loc[frame["product_id"] == 2, ["price"]]
#    price
# 1     55
```

### Creating a new column

This example creates a new column called "**cost**".

```python
import pandas as pd

frame = pd.DataFrame(
    [
        [1, 50],
        [2, 55],
        [3, 30],
        [4, 32],
        [5, 44],
    ],
    columns=["product_id", "price"],
)

margin = 0.6
frame["cost"] = frame["price"] * (1 - margin)

print(frame)
#    product_id  price  cost
# 0           1     50  20.0
# 1           2     55  22.0
# 2           3     30  12.0
# 3           4     32  12.8
# 4           5     44  17.6
```

### Dropping duplicated rows

```python
import pandas as pd

frame = pd.DataFrame(
    [
        [1, 50],
        [2, 55],
        [3, 30],
        [4, 32],
        #🡓 Duplicated IDs
        [5, 44], # <- this will be deleted
        [5, 54], # <- let's keep only this last one
    ],
    columns=["product_id", "price"],
)
print(frame)
#    product_id  price
# 0           1     50
# 1           2     55
# 2           3     30
# 3           4     32
# 4           5     44
# 5           5     54

frame.drop_duplicates(subset="product_id", keep="last", inplace=True)
print(frame)
#    product_id  price
# 0           1     50
# 1           2     55
# 2           3     30
# 3           4     32
# 5           5     54 # <-- kept
```

### Dropping rows having null values

```python
import pandas as pd

frame = pd.DataFrame(
    [
        [1, 50],
        [2, 55],
        [3, 30],
        [4, None], # <-- let's drop this one
        [5, 44],
    ],
    columns=["product_id", "price"],
)
print(frame)
#    product_id  price
# 0           1   50.0
# 1           2   55.0
# 2           3   30.0
# 3           4    NaN  # <--
# 4           5   44.0

frame.dropna(subset="price", inplace=True)
print(frame)
#    product_id  price
# 0           1   50.0
# 1           2   55.0
# 2           3   30.0
# 4           5   44.0
```

### Modifying a column

```python
import pandas as pd

frame = pd.DataFrame(
    [
        [1, 50],
        [2, 55],
        [3, 30],
        [4, 32],
        [5, 44],
    ],
    columns=["product_id", "price"],
)
print(frame)
#    product_id  price
# 0           1     50
# 1           2     55
# 2           3     30
# 3           4     32
# 4           5     44

tax = 1.25
frame["price"] = frame["price"] * tax
print(frame)
#    product_id  price
# 0           1  62.50
# 1           2  68.75
# 2           3  37.50
# 3           4  40.00
# 4           5  55.00
```

### Renaming columns

```python
import pandas as pd

frame = pd.DataFrame(
    [
        [1, 50],
        [2, 55],
        [3, 30],
        [4, 32],
        [5, 44],
    ],
    columns=["product_id", "price"],
)
print(frame)
#    product_id  price
# 0           1     50
# 1           2     55
# 2           3     30
# 3           4     32
# 4           5     44

frame.rename(columns={"price": "cost"}, inplace=True)
print(frame)
#    product_id  cost
# 0           1    50
# 1           2    55
# 2           3    30
# 3           4    32
# 4           5    44
```

### Changing column data-type

```python
import pandas as pd

frame = pd.DataFrame(
    [
        [1, 50.1],
        [2, 55.3],
        [3, 30.2],
        [4, 32.7],
        [5, 44.6],
    ],
    columns=["product_id", "price"],
)
print(frame)
#    product_id  price
# 0           1   50.1
# 1           2   55.3
# 2           3   30.2
# 3           4   32.7
# 4           5   44.6


# Convert price to integer
frame = frame.astype({"price": int})
print(frame)
#    product_id  price
# 0           1     50
# 1           2     55
# 2           3     30
# 3           4     32
# 4           5     44
```

### Filling missing data

```python
import pandas as pd

frame = pd.DataFrame(
    [
        [1, 50],
        [2, None],
        [3, None],
        [4, None],
        [5, 44],
    ],
    columns=["product_id", "price"],
)
print(frame)
#    product_id  price
# 0           1   50.0
# 1           2    NaN
# 2           3    NaN
# 3           4    NaN
# 4           5   44.0

frame["price"] = frame["price"].fillna(0)
print(frame)
#    product_id  price
# 0           1   50.0
# 1           2    0.0
# 2           3    0.0
# 3           4    0.0
# 4           5   44.0
```

### Concatenating dataframes

```python
import pandas as pd

columns = ["product_id", "price"]
frame1 = pd.DataFrame(
    [
        [1, 50],
        [2, 55],
        [3, 30],
    ],
    columns=columns,
)
print(frame1)
#    product_id  price
# 0           1     50
# 1           2     55
# 2           3     30

frame2 = pd.DataFrame(
    [
        [4, 32],
        [5, 44],
        [6, 31],
    ],
    columns=columns,
)
print(frame2)
#    product_id  price
# 0           4     32
# 1           5     44
# 2           6     31

frame3 = pd.concat([frame1, frame2])
print(frame3)
#    product_id  price
# 0           1     50
# 1           2     55
# 2           3     30
# 0           4     32
# 1           5     44
# 2           6     31
```

### Pivot tables

```python
import pandas as pd

frame = pd.DataFrame(
    [
        ["Fabio", "Literature", 3],
        ["Fabio", "History", 8],
        ["Fabio", "Geography", 8],
        ["João", "Literature", 9],
        ["João", "History", 7],
        ["João", "Geography", 7],
        ["Pedro", "Literature", 7],
        ["Pedro", "History", 9],
        ["Pedro", "Geography", 7],
    ],
    columns=["student", "exam", "score"]
)
print(frame)
#   student        exam  score
# 0   Fabio  Literature      3
# 1   Fabio     History      8
# 2   Fabio   Geography      8
# 3    João  Literature      9
# 4    João     History      7
# 5    João   Geography      7
# 6   Pedro  Literature      7
# 7   Pedro     History      9
# 8   Pedro   Geography      7


pivoted_frame = frame.pivot(index="exam", columns="student", values="score")
print(pivoted_frame)
# student     Fabio  João  Pedro
# exam                          
# Geography       8     7      7
# History         8     7      9
# Literature      3     9      7

```

### Melt dataframe (undo pivot)

```python
import pandas as pd

frame = pd.DataFrame(
    [
        ["Geography", 8, 7, 7],
        ["History", 8, 7, 9],
        ["Literature", 3, 9, 7],
    ],
    columns=["exam", "Fabio", "João", "Pedro"]
)
print(frame)
#          exam  Fabio  João  Pedro
# 0   Geography      8     7      7
# 1     History      8     7      9
# 2  Literature      3     9      7

melted_frame = frame.melt(
    id_vars=["exam"],
    value_vars=["Fabio", "João", "Pedro"],
    var_name="student",
    value_name="score",
)
print(melted_frame)
#          exam student  score
# 0   Geography   Fabio      8
# 1     History   Fabio      8
# 2  Literature   Fabio      3
# 3   Geography    João      7
# 4     History    João      7
# 5  Literature    João      9
# 6   Geography   Pedro      7
# 7     History   Pedro      9
# 8  Literature   Pedro      7
```

### Chaining methods

An exampe of search and sorting.

```python
import pandas as pd

frame = pd.DataFrame(
    [
        ["Fabio", 6],
        ["Joao", 8],
        ["Pedro", 9],
        ["Aline", 10],
        ["Ricardo", 7],
        ["Tina", 8],
        ["Julio", 7],
        ["Rogério", 6],
        ["Maria", 9],
    ],
    columns=["student", "score"]
)
print(frame)
#    student  score
# 0    Fabio      6
# 1     Joao      8
# 2    Pedro      9
# 3    Aline     10
# 4  Ricardo      7
# 5     Tina      8
# 6    Julio      7
# 7  Rogério      6
# 8    Maria      9

MIN_SCORE = 7

approved = (
    frame[frame["score"] >= MIN_SCORE]
    .sort_values(by="score", ascending=False)
)
print(approved)
#    student  score
# 3    Aline     10
# 2    Pedro      9
# 8    Maria      9
# 1     Joao      8
# 5     Tina      8
# 4  Ricardo      7
# 6    Julio      7
```