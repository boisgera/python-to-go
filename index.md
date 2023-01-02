---
marp: true
title: From Python to Go
author: Sébastien Boisgerault (@boisgera)
theme: uncover
---

# From Python to Go

---


# Typing

---

### The Good Old Days

`factorial.py`

```python
import sys

def factorial(n):
    fact_n = 1
    for i in range(n):
        fact_n = fact_n * (i + 1)
    return fact_n

n = sys.argv[1]
print(factorial(n))
```

---

###  🪲 Ooops

The program fails at runtime:

```bash
$ python factorial.py 10
Traceback (most recent call last):
  File "fact.py", line 10, in <module>
    print(factorial(n))
  File "fact.py", line 4, in factorial
    for i in range(n):
TypeError: 'str' object cannot be interpreted as an integer
```

---

### 🧠 Error Analysis

  - The elements of `sys.argv` are strings 
  
  - The argument of `factorial` should be an integer

---

### 🩹 Fix



```python
import sys

def factorial(n):
    fact_n = 1
    for i in range(n):
        fact_n = fact_n * (i + 1)
    return fact_n

n = str(sys.argv[1])
print(factorial(n))
```

```bash
$ python fact.py 10
3628800
```

---

### Nowadays

The same program, with a few **type hints**:

```python
import sys

def factorial(n: int) -> int:
    for i in range(n):
        fact_n = fact_n * (i + 1)
    return fact_n

n = sys.argv[1]
print(factorial(n))
```

in Visual Studio Code, with the optional static type checker [mypy](https://mypy-lang.org/) enabled.

---


![](images/mypy.png)

---


---

Typing Nazi

```python
import sys

def factorial(n: int) -> int:
    i: int
    fact_n: int = 1
    for i in range(n):
        fact_n = fact_n * (i + 1)
    return fact_n

n = sys.argv[1]
print(factorial(n))
```


---


  - Dynamic or Static 

  - Mandatory or Optional

---

  - Every variable has a type

  - Can the type change at runtime?

  - Should the type change at runtime?


---

# Assignment

# TODO: Later 

What is the semantics of assignment ?

    sum = 1 + 1

  - 🧮 Evaluate the right-hand side

  - 🤨 ???

  - 💸 Profit!

---

# Basic Types


| Python | Go         |
| :----- | :--------- | 
| bool     | bool       |
| int    | int        |
| float     | float64    |
| str     | string     |

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

## Methods

---

```python
from datetime import date

class Person:
    def __init__(self, name, year):
        self.name = name
        self.year = year
    def Age(self):
        return date.today().year - self.year

def main():
    guido = Person(name="Guido van Rossum", year=1956)
    print(guido.name)
    print(guido.year)
    print(guido.Age())
```

---

```go
package main

import "time"

type Person struct {
    Name string
    Year int
}

func (p Person) Age() int {
    return time.Now().Year() - p.Year
}

func main() {
    rob := Person{Name: "Robert Pike", Year: 1956}
    println(rob.Name)
    println(rob.Year)
    println(rob.Age())
}
```

---

```go
...

func (p Person) SetName(name string) {
    p.Name = name
}

func (p Person) SetYear(year int) {
    p.Year = year
}

func main() {
    rob := Person{} // ℹ️ equivalent to Person{Name: "", Year: 0}
    rob.SetName("Robert Pike")
    rob.SetYear(1956)
    println(rob.Name)
    println(rob.Year)
    println(rob.Age())
}
```

---

```bash
$ go run app.go 

0
2023
```

Uhu?

---

```go
func (p *Person) SetName(name string) {
    p.Name = name
}

func (p *Person) SetYear(year int) {
    p.Year = year
}
```


---

```bash
$ go run app.go 
Robert Pike
1956
67
```

---

```bash
$ go mod init app
$ cat go.mod
module app

go 1.19
```

---

`app/person/person.go`

```go
package person

import "time"

type Person struct {
    Name string
    Year int
}

func New(name string, year int) Person {
    return Person{Name: name, Year: year}
}

func (p *Person) SetName(name string) {
    p.Name = name
}

func (p *Person) SetYear(year int) {
    p.Year = year
}
```

---

`app.go`

```go
package main

import (
    "app/person"
    "fmt"
)

func main() {
    rob := person.Person{}
    rob.SetName("Robert Pike")
    rob.SetYear(1956)
    fmt.Println(rob.Name)
    fmt.Println(rob.Year)
    fmt.Println(rob.Age())
}
```
