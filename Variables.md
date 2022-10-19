## Variables


## Variables & Types

Python is **dynamically typed**; 
the same variable can refer to several types of data.

```python
a = 42        # a is an int
a = "Hello!"  # a is a string
```

On the other hand, Go is **statically typed**: 
every variable is assigned a type when it is **declared**.

```go
var a int
```

Variable declaration is mandatory before use; 
afterwards it can be assigned values of this type.

``` go
a = 42
```

Any assignment with another data type is invalid.

```go
a = "Hello!"  // 🪲 The Go compiler will reject this.
```

Static typing brings some extra safety to the language
(the compiler will check typing consistency), 
but also some extra work for the programmer. 
Hopefully, several mecanisms can simplify 
the management of variables in Go.

### Initial Assignment

When a variable is declared without such a value, it is assigned the "zero value" 
that corresponds to its type :

```go
var a int  // Here a == 0
```

If you need another value, you can assign it in the declaration

```go
var a int = 42
```

### Type Inference

Go can often deduce from the context what the type of the variable is.
In this case, you can omit the type from the declaration.


```go
var a = 42  // That's an int for you!
```

### Short Declaration

A fused declaration-assignment such as `var a = 42` is equivalent to the 
short declaration syntax.

```go
a := 42
```

⚠️ This shortcut doesn't work for global variables, declared outside a function.

