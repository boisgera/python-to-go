Control Flow
================================================================================


The [FizzBuzz](https://en.wikipedia.org/wiki/Fizz_buzz) challenge: 

> Write a program that prints the numbers from 1 to 20. 
> But for multiples of three print "Fizz" instead of the number and 
> for the multiples of five print "Buzz". 
> For numbers which are multiples of both three and five print "FizzBuzz".

In Python:

```python
for number in range(1, 21):
    if number % 3 == 0 and number % 5 == 0:
        print("FizzBuzz")
    elif number % 3 == 0:
        print("Fizz")
    elif number % 5 == 0:
        print("Buzz")
    else:
        print(number)
```

In Go

```go
for n := 1; n <= 20; n = n + 1  {
    if n%3 == 0 && n%5 == 0 {
        println("FizzBuzz")    
    } else if n%3 == 0 {
        println("Fizz")
    } else if n%5 == 0 {
        println("Buzz")
    } else {
        println(n)
    }
}
```
