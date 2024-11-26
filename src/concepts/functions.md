# Functions

Karina has four types of functions:

1. [Methods](#methods)
2. [Static Functions](#functions)
3. [Extension Functions](#extension-functions)
4. [Functions in Interfaces](#functions-in-interfaces)

## General Syntax and Semantics


```karina
fn create<T>(name: string, object: T) -> Option<T> {
    //...
}
```

Every function has a name and a list of parameters. The parameters are defined by with `<name>:<type>` and are comma separated.

The return type is defined after the `->` symbol, but can be omitted if the function does not return anything:

```karina
fn println(obj: Object) {
    //...
}
```

Functions can define generic types by using the `<T>` syntax after the function name. The generic type can be used in the function signature and body.

```karina
fn toString(obj: T) -> string {
    //...
}
```

The base of every generic type is `java.lang.Object`, so you can still invoke methods like `toString()` on generic types.

Every Function has a body, except for [methods defined in interfaces](#methods-in-interfaces). The body is enclosed in curly braces `{}`. You can also use the shortshand syntax `= <expr>` after the function signature to define the body in a single line.

```karina
fn toString(obj: T) -> string = obj.toString()
```


## Static Functions

Static functions are defined on the top level and can be called without an instance of a struct. The follow the general syntax of functions. 

The main function is for example a static function.

```karina
fn main(args: Array<string>) {
    //...
}
```

You cannot call a static function on an instance of the first parameter, use [extension functions](#extension-functions) for this.


## Methods

Methods are functions that are defined within a struct. They are called with/on an instance of the struct. 

```karina
struct Album {
    //...

    fn shuffle() -> [Song] {
        self.songs.shuffle()
    }
}
```

To refer to the current instance of the struct use the `self` keyword.

You can call the function directly on the instance:

```karina
let album = //...

album.shuffle()
```

.. or by using the path to the function and passing the instance as the first argument:

```karina
let album = //...

Album.shuffle(album)
```

You can also pass the method as a function reference:

```karina
let album = //...

let shuffle = Album.shuffle
shuffle(album)
```


## Extension Functions

Karina also allows to extend existing objects with functions. 
These functions are static functions that allow special syntax.
They can only be defined on the top level. 

```karina
fn toDouble() -> double extends int {
  return self as double 
}
```

The `self` keyword is used to refer to the object the function is called on.

These functions allow you to call them like methods on an instance:
```karina
let number = 5.toDouble()
```

This can also limit the domain of the function:

```karina
fn join() -> string extends [string] {
 //...
}

fn sum() -> int extends [Integer] { 
  //...
}
```

Make sure to import the function to use it.


## Methods in Interfaces

See [Interfaces](interfaces.md)


