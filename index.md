---
marp: true
title: From Python to Go
author: Sébastien Boisgerault (@boisgera)
theme: uncover
---

# From Python to Go

---


# Typing

  - Every variable has a type

  - Can the type change at runtime?

  - Should the type change at runtime?


---

# Assignment

What is the semantics of assignment ?

    sum = 1 + 1

  - 🧮 Evaluate the right-hand side

  - 🤨 ???

  - 💸 Profit!

---

# Basic Types


| Python | Go         |
| :----- | :--------- | 
| bool	 | bool       |
| int    | int        |
| float	 | float64    |
| str	 | string     |

---

# Do It Yourself

```
$ python -c 'print(type(42).__name__)'
int
```

```
$ yaegi run -e 'reflect.TypeOf(42)'
int
```

---

# Functions

```python
from datetime import date

def getDuration(year):
    return date.today().year - self.year
```

---

```go
import time

def getDuration(year int) int {
    return time.Now().Year() - year
}
```

---

# Lists $\to$ Slices

```
>>> l = [1, 2, 4]
>>> l[0]
1
>>> l.append(8)
>>> l
[1, 2, 4, 8]
>>> for n in l:
...     print(n)
... 
1
2
4
8   
```

---

```
> s := []int{1, 2, 4}
: [1 2 4]
> s[0]
: 1
> s = append(s, 8)
: [1 2 4 8]
> for n := range s { println(n) }
0
1
2
3
: [1 2 4 8]
```

---

# Dicts $\to$  Maps

```
>>> d = {"a": 1, "b": 2}
>>> d["a"]
1
>>> d["c"] = 3
>>> d
{'a': 1, 'b': 2, 'c': 3}
>>> for k, v in d.items():
...     print(k, v)
... 
a 1
b 2
c 3
```

---

```
> m := map[string]int{"a": 1, "b": 2}
: map[a:1 b:2]
> m["a"]
: 1
> m["c"] = 3
: 0
> m
: map[a:1 b:2 c:3]
> for k, v := range m { println(k, v) }
a 1
b 2
c 3
: map[a:1 b:2 c:3]
```
---

# Objects $\to$ Structs

```python
class Person:
    def __init__(self, name, year):
        self.name = name
        self.year = year

def main():
    guido = Person(name="Guido van Rossum", year=1956)
    print(guido.name)
    print(guido.year)
```

---


```go
 type Person struct {
    Name string
    Year int
}

func main() {
    rob := Person{Name: "Robert Pike", Year: 1956}
    println(rob.Name)
    println(rob.Year)
}
```

---

```python
from datetime import date

class Person:
    def __init__(self, name, year):
        self.name = name
        self.year = year
    def getAge(self):
        return date.today().year - self.year

def main():
    guido = Person(name="Guido van Rossum", year=1956)
    print(guido.name)
    print(guido.year)
    print(guido.getAge())
```

---

```go
package main

import "time"

type Person struct {
    Name string
    Year int
}

func (p Person) getAge() int {
    return time.Now().Year() - p.Year
}

func main() {
    rob := Person{Name: "Robert Pike", Year: 1956}
    println(rob.Name)
    println(rob.Year)
    println(rob.getAge())
}
```