# Control Flow


## If Statements

If statements can be defined without parentheses. You have to follow up the condition with a block of code. 


You can follow up the optional `else` part with another if statement, a match expression, or a block of code.


```karina
let number = 3

if number > 5 {
    println("Number is greater than 5")
} else if number == 5 {
    println("Number is equal to 5")
} else match number {
    case 4 => println("Number is 4")
    case 3 => println("Number is 3")
    case _ => println("Number is less than 3")
}
```

## Break

You can use the `break` keyword to exit a loop. 

```karina
let numbers = [1, 2, 3, 4, 5]

for number in numbers {
    if number == 3 {
        break
    }
    println(number)
}
```

## Continue

You can use the `continue` keyword to skip the rest of the loop and continue to the next iteration. 

```karina
let numbers = [1, 2, 3, 4, 5]
let i = 0
while i < numbers.size() {
    if numbers[i] == 3 {
        i += 1
        continue
    }
    println(numbers[i])
    i += 1
}
```

## Return

You don't need to use the `return` keyword to return a value from a function. The last expression in a function is automatically returned. 

```karina
fn add(a: int, b: int) -> int {
  a + b
}

println(add(1, 2)) // 3
```

You can still use the `return` keyword if you want to return early from a function. 

```karina
fn fib(n: int) -> int {
  if n <= 1 {
    return n
  }
  fibonacci(n - 1) + fibonacci(n - 2)
}
```

