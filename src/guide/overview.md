

## Features

The Karina programming language is designed to be a simple, expressive, and powerful language for building applications. It has a number of features that make it unique and easy to use. 

This document provides an overview of the key features of Karina, including its syntax, type system, and standard library. It also includes examples of how to use these features in practice.

You can also find some examples in the [test](https://github.com/Plixo2/KarinaC/tree/master/tests) directory of the KarinaC repository.

--- 

### Table of Contents

1. [Functions](#functions)
2. [Expressions](#expressions)
3. [Structs](#structs)
4. [Enums](#enums)
5. [Closures](#closures)
6. [Interfaces](#interfaces)


# Functions

Main page: [Functions](../concepts/functions.md)

```karina
fn create<T>(name: string, object: T) -> Option<T> {
    //...
}
```

This is a static generic function that takes a type parameter `T` and returns an `Option<T>`. 

The braces can be omitted if the function body is a single expression.

```karina
fn get_name(self) -> string = self.name
```
The `self` keyword is used to refer to the current instance of the struct or class. It is similar to `this` in other languages.

Functions can also be defined as methods on structs, enums and interfaces. 

# Expressions

Main page: [Expressions](../expressions/expressions.md)


Generally speaking, everything is a expression in Karina. This reduces less boilerplate code and makes the code more readable. 

```karina
let counter = {
    let value: int = 0
    if value > 0 { value } else { 0 }
}
```

<br>

There are way to many expressions to list them all here, but here are some of the most notable ones:

```karina
fn option() -> Integer? {
    //Closures
    let getValue = fn() Option::Some { _: 1}

    //Unwrapping
    let opt = getValue().map(fn(s) String::valueOf(s))?

    //if patterns
    let other: string = if getValue().okOr("Error") is Result::Ok(v) {
        //String interpolation
        'Value: $v' 
    } else is Result::Err(e) {
        e
    }

    // Object creation
    Option::OK { 
        //Boxing & Unboxing
        value: Integer::valueOf("123") + 1
    }        
}
```
<br>

Using Java features is exactly the same as using Karina features. 

```karina
let list = java::util::ArrayList{}
list.add(1)

list.forEach(fn(i) {
    println(i)
})
```
In this example, we can see the type inference in action. Also note that the closure will automatically adapt to the type expected by the `forEach` method.


# Structs

Main page: [Structs](../types/structs.md)

Structs are basicly java classes, but with a more lightweight syntax.

```karina
struct Person {
    const JANE_DOE: Person = Person::create("Jane Doe")

    name: string
    
    // mutable value
    age: mut int
    
    // method on the struct
    fn setAge(self, newAge: int) {
        self.newAge = newAge
    }

    // static method
    fn create(name: string) -> Person = {
        Person {
            name: name,
            age: 0
        }
    }


    //interfaces
    impl AutoCloseable {
        fn close(self) {
            // ...
        }
    }
}
```

By default, each structs has a default constructor, for initializing all fields.

Defining custom constructors and super classes is also possible, but not recommended:
```karina
@Super = {
    type: type { java::lang::RuntimeException }
}
struct RuntimeError {
    message: string
    cause: Throwable?

    fn (self, message: string, cause: Throwable?) {
        super<java::lang::RuntimeException>(message, cause.nullable())
    }
}
```

<div class="note">

> A default constructor will not be created if you define your own constructors.

</div>

</br>

# Enums

Main page: [Enums](../types/enums.md)

Karina supports **Algebraic Data Types**, aka **enums**, as a way to define a type with a fixed set of values.

The Karina standard library provides a number of useful enums, such as `Option` and `Result`.

# Closures

Main page: [Closures](../types/function.md)

# Interfaces

Main page: [Interfaces](../concepts/interfaces.md)