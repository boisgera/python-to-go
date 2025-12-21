---
marp: true
title: From Python to Go
author: Sébastien Boisgerault (@boisgera)
theme: uncover
---

# From 🐍 Python to 🦫 Go

Introduction to Go for Python devs


---

### Selected References

  - 📖 [A Tour of Go](https://go.dev/tour/welcome/1)

  - 📖 [Go Playground](https://go.dev/play/) 

  - 📖 [Go by Example](https://gobyexample.com/)

  - 📖 [Effective Go](https://go.dev/doc/effective_go)

  - 📖 [The Go Programming Language Specification](https://go.dev/ref/spec)
  
---


# 🧭 Typing

##  Dynamic vs Static

---

### 🐍 The Good Old Days

```bash
$ python factorial.py 10
3628800
```

```python
# filename: factorial.py
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
  File "/home/boisgera/tmp/tmp/factorial.py", line 11, in <module>
    print(factorial(n))
          ^^^^^^^^^^^^
  File "/home/boisgera/tmp/tmp/factorial.py", line 6, in factorial
    for i in range(n):
             ^^^^^^^^
TypeError: 'str' object cannot be interpreted as an integer
```

---

### Traceback & Program Analysis

🤔

 1. `n` should be an integer for `range(n)` to work,

 2. `factorial(n)`: same analysis,

 3. but instead `n` a string,

 4. because the elements of `sys.argv` are strings !
  
😌

---

```python
# filename: factorial.py
import sys

def factorial(n):
    fact_n = 1
    for i in range(n):
        fact_n = fact_n * (i + 1)
    return fact_n

n = sys.argv[1]  # 🪲 n should be an int
print(factorial(n))
```

---

### 🩹 Fix



```python
# filename: factorial.py
import sys

def factorial(n):
    fact_n = 1
    for i in range(n):
        fact_n = fact_n * (i + 1)
    return fact_n

n = int(sys.argv[1])  # 🩹 str -> int
print(factorial(n))
```

```bash
$ python fact.py 10
3628800
```

---

### Nowadays: Gradual Typing

The same program, with a few **type hints**:

```python
# filename: factorial.py
import sys

def factorial(n: int) -> int:
    for i in range(n):
        fact_n = fact_n * (i + 1)
    return fact_n

n = sys.argv[1]
print(factorial(n))
```

---

### Static Type Checking

With [mypy](https://mypy-lang.org/):

```
$ mypy factorial.py 
factorial.py:11: error: 
Argument 1 to "factorial" has incompatible type "str"; 
expected "int"  [arg-type]
Found 1 error in 1 file (checked 1 source file)
```

---

in Visual Studio Code, with [mypy](https://mypy-lang.org/) enabled:



![](images/mypy.png)

---

All types declared

```python
# filename: factorial.py
import sys

def factorial(n: int) -> int:
    i: int
    fact_n: int = 1
    for i in range(n):
        fact_n = fact_n * (i + 1)
    return fact_n

n: int = sys.argv[1]
print(factorial(n))
```

---
### 🐍

  - Python is **dynamically typed**: the same variable may refer to different types of object at different moments.

  - Developers may use (static) type hints + static type checkers to *willingly* restrict this freedom.

---
### 🦫

  - Go is **statically typed**: the type of every variable is known at compile-time.

  - Because it has been either:

    - **declared** by the developper or,

    - **inferred** by the compiler.

---

```go
var message string       // declaration
message = "Hello world!" // assignment
```

```go
var message string = "Hello world!" // combined
```

In this context, the type of message is obvious; 
we can drop the explicit type information.

```go
var message = "Hello world!"  // combined
```

---

In the body of functions (⚠️ but not at the top-level!)

```go
var message = "Hello world!"  // combined
```

can be replaced with the **short variable declaration**:

```
message := "Hello world!"
```

which *feels* like dynamical typing (but isn't 😀).
 
---

### Buggy Program (invalid types)

```go
package main

func main() {
    a := "Hello world!"
    a = "Hello gophers! 🦫"
    a = 42
    println(a)
}
```

---

### Compilation Step

```
$ go build sketch.go 
# command-line-arguments
./sketch.go:6:6: 
cannot use 42 (untyped int constant) as string value in assignment
```

---

# Basic Types


| Python | Go         |
| :----- | :--------- | 
| bool     | bool       |
| int    | int        |
| float     | float64    |
| str     | string     |

---

# 🧭 Functions

---

## 🐍 Python

```python
from datetime import date

def get_duration(year: int) -> int:
    return date.today().year - year
```

---

## 🦫 Go

```go
import "time"

func GetDuration(year int) int {
    return time.Now().Year() - year
}
```

---

# 🐍 Lists $\to$ 🦫 Slices

```
>>> l = [1, 2, 4]
>>> l[0]
1
>>> l.append(8)
>>> l
[1, 2, 4, 8]
>>> for i, n in enumerate(l):
...     print(i, n)
... 
0 1
1 2
2 4
3 8   
```

---

(with the go interpreter/REPL [yaegi](https://github.com/traefik/yaegi))

```
> s := []int{1, 2, 4}
: [1 2 4]
> s[0]
: 1
> s = append(s, 8)
: [1 2 4 8]
> for i, n := range s { println(i, n) }
0 1
1 2
2 4
3 8
: [1 2 4 8]
```

---

# 🐍 Dicts $\to$ 🦫 Maps

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

# 🐍 Objects $\to$ 🦫 Structs

### 🐍 Python

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

### 🐍 Modern Python

```python
from dataclasses import dataclass

@dataclass
class Person:
    name: str
    year: int

def main():
    guido = Person(name="Guido van Rossum", year=1956)
    print(guido.name)
    print(guido.year)
```

---

### 🦫 Go 

```go
package main

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

# 🧭 Object Methods

---

### 🐍 Python methods

```python
from dataclasses import dataclass
from datetime import date

@dataclass
class Person:
    name: str
    age: int
    def get_age(self):
        return date.today().year - self.year

def main():
    guido = Person(name="Guido van Rossum", year=1956)
    print(guido.name)
    print(guido.year)
    print(guido.get_age())
```

---
### 🦫 Go Methods

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

### 🦫 Go Methods (continued)

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
2025
```

😕 Uhu?

---

# Assignment


🤔 What is the semantics of assignment ?

    sum = 1 + 1

  - 🧮 Evaluate the right-hand side

  - 🤨 ???

  - 💸 Profit!

---

### 🐍 Assignment in Python

Variables **refer to** a location in memory (via a **pointer**).

Assignment **rebinds** the pointer to a new location.

```python
>>> a = 1 + 1
>>> print(hex(id(a)))
0x559b96062d60
>>> a = 42
>>> print(hex(id(a)))
0x559b96063260
```

---

### 🦫 Assignment in Go

Variables store data in a fixed location:

```go
package main

func main() {
    a := 1 + 1
    println(&a)
    a = 42
    println(&a)
}
```

Output:

```
0xc00003e728
0xc00003e728
```

---

We can also store content at a location

```go
package main

func main() {
    a := 1 + 1
    p := &a
    *p = 42
    println(a)
}
```
Output:
```
42
```

---

### Back to the Go methods


```go
func (p Person) SetName(name string) {
    p.Name = name
}
```
This method **copies** the original person to the local variable `p` of the method, then modifies this copy. 

**The original person has not been modified!**

To avoid that, provide a pointer argument instead.


---

```go
func (p *Person) SetName(name string) {
    p.Name = name
}

func (p *Person) SetYear(year int) {
    p.Year = year
}

func main() {
    rob := Person{}
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
Robert Pike
1956
69
```

# 👍

---

### 🧹 Clean-up

```bash
$ go mod init app
```

Crée le fichier `go.mod`:

```bash
module app

go 1.25.1
```

---

`person.go`

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

func main() {
    rob := Person{}
    rob.SetName("Robert Pike")
    rob.SetYear(1956)
    println(rob.Name)
    println(rob.Year)
    println(rob.Age())
}
```

------
### Compilation

On my (Linux) computer,

```bash
$ go build
```

 creates a `app` executable for Linux.

```bash
$ ./app
Robert Pike
1956
69
```

---
### 🖥️ Cross-compilation


Set the environment variables `GOOS` and `GOARCH`, then `go build` as usual:

  - 🪟 `GOOS=windows` and `GOARCH=amd64`

    $\to$ windows/amd-64 executable `app.exe`.

  - 🍏 `GOOS=darwin` `GOARCH=arm64`

    $\to$ macOS/arm64 executable `app`.

  - 🐧 `GOOS=linux` and `GOARCH=arm64`

    $\to$ raspberry-pi/arm-64 executable `app`.

---

### Embedded Systems

![The Go Programming Language](https://tinygo.org/images/tinygo-logo.png)

Try [TinyGo](https://tinygo.org/)!

(Arduino, Raspberry Pi Pico, etc.)


