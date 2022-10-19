Hello world!
================================================================================

The classic "Hello world!" program in Python (file `hello.py`):

```python
print("Hello world! 👋")
```

Execute this program with:

```bash
$ python hello.py
Hello world! 👋
```

The corresponding program in Go (file `hello.go`):

```go
package main

func main() {
    println("Hello world! 👋")
}
```

Execute this program with:

```bash
$ go run hello.go
Hello world! 👋
```


## Python Modules

Our Python file `hello.py` is a designed as a program. 
But at the same time it's also a Python library, a bundle of features 
that are meant to be used by other Python programs (or in the
Python interpreter).

Start the python interpreter, import `hello` and you will be greeted with 
the hello message.

```pycon
>>> import hello
Hello world! 👋
```

If you intend to use `hello.py` as a module, 
you probably don't want the message to appear when the message is imported 
but when you call a specific function.
To achieve this goal, replace the content of `hello.py` with:

```python
def print_hello():
    print("Hello world! 👋")
```

Now in the Python interpreter, you need to call the `print_hello` function
for the message to appear

```pycon
>>> import hello
>>> hello.print_hello()
Hello world! 👋
```

But now, we have broken `"hello.py"` as a program :

```bash
$ python hello.py  # not output!
```

At this stage, the execution of `hello.py` will
define the function `print_hello` but not execute it.

In order for `hello.py` to work as intended as a program **and** as a library,
we can use a magic variable `__name__` provided by Python. 
It will be the module name `"hello"` when the file is imported and
 `"__main__"` when it's executed.

```python
def print_hello():
    print("Hello world! 👋")

if __name__ == "__main__":
    print_hello()
```

Now everything works as expected:

```bash
$ python hello.py
Hello world! 👋
```

and

```pycon
>>> import hello
>>> hello.print_hello()
Hello world! 👋
```

## Go packages

In Go, on the other hand, there is a clear difference between **commands**
(programs) and **packages** (libraries).

this practice is mandatory. Note that in Python you need to call
the function that you have just defined for your program to work. In Go,
this is not necessary since the function called `main` is always the
program entry point.

--------------------------------------------------------------------------------

In Python, the `print` function adds a newline to its argument,
which is probably what you want by default.

```
>>> print("Hello world!")
Hello world!
>>>
```

Use the `end` parameter to override this behavior, for example to get rid of
this extra newline.

```
>>> print("Hello world!", end="")
Hello world!>>>
```

In Go, the `print` does not add a newline

```
> print("Hello world!")
Hello world!>
```

but the `println` function does  

```
> println("Hello world!")
Hello world!
>
```
