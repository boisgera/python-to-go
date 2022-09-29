Hello world!
================================================================================

The classic "Hello world!" program in Python (`hello.py`):

```python
print("Hello world!")
```

Execute this program:

```bash
$ python hello.py
Hello world!
```

The corresponding program in Go (`hello.go`):

```go
package main

func main() {
    println("Hello world!")
}
```

Execute this program with:

```bash
$ go run hello.go
Hello world!
```

--------------------------------------------------------------------------------

In Go, the `package main` declaration is mandatory for programs.

In our Python example, it would probably be a good idea to enclose our
code in a function

```python
def main():
    print("Hello world!")

main()
```

In Go, this practice is mandatory. Note that in Python you need to call
the function that you have just defined for your program to work. In Go,
this is not necessary since the function called `main` is always the
program entry point.

--------------------------------------------------------------------------------

In Python, the `print` function adds a newline to its argument,
which is often what you want.

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
>
```
