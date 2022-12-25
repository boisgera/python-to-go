# Variables

# Fixed/Variable Memory Location

A Python assignment make a variable refer to the object on the right-hand side.
Reassign a variable and its **address** (the location in memory it points to) 
changes.

```python
>>> a = 1
>>> id(a)
94024396889408
>>> a = 2
>>> id(a)
94024396889440
```

In Go, you need to declare a variable before you make an assignment

```go
> a = 1
1:28: undefined: a
```

```go
> a = 1
1:28: undefined: a
> var a int
: 0xc0002082e0
> a = 1
: 1
```

Alternatively, the shortcut `:=` declares and assigns the variable:

```go
> a := 1
1
```

Once a variable is declared, the location in memory it points to is fixed.

```go
> &a
: 0xc00003b2f0
> a = 2
: 2
> &a
: 0xc00003b2f0
```

