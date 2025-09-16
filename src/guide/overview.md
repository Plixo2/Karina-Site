

## Features


**Karina** is a statically typed, general-purpose, high-level programming language focused on:

- Simplicity
- Interoperability
- Concise notation
- Null safety

**Key features:**
- Fully compatible with Java
- Seamless use of existing libraries and frameworks
- Modern programming experience
- Algebraic Data Types
- First-class functions

You can also find some examples in the [test](https://github.com/Plixo2/KarinaC/tree/master/tests) directory of the KarinaC repository.

--- 

### Table of Contents

1. [Structs](#structs)
2. [Functions](#functions)
3. [Enums](#enums)
4. [Interfaces](#interfaces)
5. [Statics](#static-fields)
6. [Expressions](#expressions)
7. [Imports](#imports)

# Structs

Structs are basically java classes, but with a more lightweight syntax.

```karina
pub struct Vec2 {
    pub x: double
    pub y: double 
    
    // static function
    pub fn fromAngle(angle: double) -> Vec2 =
        Vec2 { x: Math::cos(angle), y: Math::sin(angle) }
    
    // member function
    pub fn length(self) -> double {
        Math::sqrt(self.x * self.x + self.y * self.y)
    }
}
```
A default constructor and a `toString` function are automatically created. 

```karina
let v = Vec2 { x: 1.0, y: 1.5 }
println(v) // Vec2{x=1.0, y=1.5}
```

Structs, Fields and Functions are private by default. Use the `pub` keyword to make them public.
Fields are also immutable by default. Use the `mut` keyword to make them mutable.


<br>

# Functions

Karina has five types of functions: 
1. Member Functions
2. Static Functions
3. Abstract functions
4. Closures
5. Extension Functions

### Member functions 
Member functions are defined inside a struct, enum or interface and can be called on an instance of that type. \
They have access to the instance via the `self` keyword as the first parameter.

```karina
struct User {
    name: string

    fn getNameUpperCase(self) -> string = 
        self.name.toUpperCase() 
}
```
```karina
user.getNameUpperCase()
```

### Static functions
Static functions are defined on the top level, inside a struct, enum or interface and can be called without an instance of that type.

```karina
fn display<T: ToStr + Display>(obj: T) {
    //...
}
```

This also shows the use of generic types and type constraints. 

### Abstract functions
Abstract functions are defined in interfaces and do not have a body. They have to be implemented by the struct or enum that implements the interface.

```karina
interface ToStr {
    fn toStr(self) -> string
}
```

### Closures

Closures are both a type and a expression.

```karina
let add = fn(a: int, b: int) -> int { a + b }
add(1, 2) // 3
```

Parameter types and return types can be omitted if they can be inferred. 
\
\
Closures compile to interfaces. You can explicitly define the interfaces a closure implements.

```karina
fn () -> string impl Callable<string>, Supplier<string>
```


### Extension functions
Extension functions behave like member functions, but are defined outside of the type.

```karina
@Extension
pub fn sqrt(value: double) -> double = Math::sqrt(value)
```
```karina
2.sqrt() // 1.41421...
```

They can be defined on any type, including primitives. \
The underlying function has to be in scope, so you might need to import it first.

<br>

# Enums

Karina supports **Algebraic Data Types**, as a way to define a type with a fixed set of values. \
\
This `enum` does not behave like a Java enum, but more like enums in languages like Rust or Swift. \
Coming from a Java background, think of them more like a sealed interface + record classes. 

```karina
enum AuthState {
    Guest(name: string)
    User(name: string, id: int, token: string)
    
    fn name(self) -> string
}
```
\
The Karina standard library provides the `Option` and `Result` enum. These Types can be written as `T?` and `R?E` respectively.

<br>

# Static Fields

You can define statics inside structs, enums and interfaces and on the top level.
```karina
static PI: double = 3.1415927
```
They are immutable and private by default. Use the `mut` and `pub` keywords to make them mutable and public.

<br>

# Interfaces

```karina
interface ToStr {
    fn toStr(self) -> string
    fn toString(self) -> string = self.toStr() // default implementation
    
    impl Display // extends another interface
}
```

To implement an interface, use the `impl` keyword.

```karina
struct Thing {
    name: string
    
    impl ToStr {
        fn toStr(self) -> string = self.toString()
    }
}
```

<br>

# Expressions

<details>
<summary>Table of Contents</summary>

- [Variables](#variables)
- [Branching](#branching)
- [Creating Enums and objects](#creating-enums-and-objects)
- [String interpolation](#string-interpolation)
- [Closures](#closures)
- [Unwrapping](#unwrapping)
- [Arrays](#arrays)
- [try-with-resources](#try-with-resources)
- [Cast](#cast)
- [Instance check](#instance-check)
- [Literals](#literals)
- [Paths](#paths)
- [while, for, continue, break](#while-for-continue-break)
- [Super calls and custom constructors](#super-calls-and-custom-constructors)
- [Throw](#throw)

</details>


## Variables

```karina
let name: string = "Karina" 
```
Local variables are always mutable. The type can be omitted if it can be inferred. 

```karina
let number = 0.5 // type is inferred as double
```

## Branching

```karina
if condition { 
    //...
} else if otherCondition {
    //...
}
```

`if` expressions also supports pattern matching:

```karina
let orElse = if value is Result::Ok r {
    r.toString()
} else is Result::Err e {
    e.toString()
}
```
As everything is an expression in Karina, `if` expressions can return a value

## Creating Enums and objects

```karina
let opt = Option::Some { value: 1 }
let lang = Language { _: "Karina" }
```
This constructs finds and calls the underlying constructors. 
The name and order of the fields have to match the constructor, but you can use `_` to ignore the name of the field, mainly for working with Java classes.


## String interpolation

```karina
let name = "Karina"
let greeting = 'Hello, $name!'
```

String interpolation works with single quotes. \
Use `$name` to insert a variable. Complex expressions are not supported... yet.

## Closures

Closures are both a type and a expression.

```karina
let add = fn(a: int, b: int) -> int { a + b }
add(1, 2) // 3
```

Parameter types and return types can be omitted if they can be inferred.
\
\
Closures compile to interfaces. You can explicitly define the interfaces a closure implements.

```karina
fn () -> string impl Callable<string>, Supplier<string>
```

## Unwrapping

The `?` operator can be used to unwrap an `Option` or `Result`. If the value is `None` or `Err`, the function will return early.

```karina
fn trim(opt: string?) -> string? {
    let trimmed = opt?.trim()
    Option::some(trimmed)
}
```

## Arrays

Arrays are created using square brackets.

```karina
let arr = [1, 2, 3]
```

You can also give it a type explicitly.

```karina
let arr = <int>[1, 2, 3]
```

Creating empty arrays it not supported. Use functions in the `karina::lang::Values` and `karina::lang::Option` namespaces instead.

## try-with-resources

```karina
using stream = Files::createStream(path)? {
    // ...
}
```

The `using` expression automatically closes the resource when it goes out of scope. The resource has to implement the `AutoCloseable` interface.

## Cast
```karina
10.0 as int // cast double to int
```

Casting is only allowed between primitives and types that can be safely casted.

Use `Option::instanceOf(class, value)` for safe casting.

## Instance check

```karina
let isObject = value is MyClass 
```

## Literals

All JVM primitive are supported: `int`, `long`, `float`, `double`, `byte`, `short`, `char` and `bool`. 

Numbers can be written using decimal, hexadecimal and binary notation.
```karina
0x1A // hexadecimal
0b1101 // binary
1_000_000 // underscores are ignored
```



String literals can be written using double quotes.
```karina
let str = "Hello, World!"
```

Characters can be written using single quotes.
```karina
let char = 'A'
```

Be aware that single quotes are used for characters and string interpolation. 

Strings, String interpolation and Characters support escape sequences like `\n`, `\t`, `\\`, `\'` and `\"` and even unicode characters like `\u03A9` (Ω)


## Paths

Paths are written using `::` as a separator, for types, static fields and functions.
```karina
let path = org::example::MyClass::MY_STATIC
```

## while, for, continue, break

`while`, `for`, `continue` and `break` work as expected.

```karina
while true {
    if false {
        break
    } else {
        continue
    }
}
```

Yielding values from loops is not supported... yet.

## Super calls and custom constructors

```karina
struct Thing {
    name: string
    fn (self) {
        super<Object>()
        self.name = "Thing"
    }
}
```
You can define a custom constructor by defining a member function without a name. \
Be aware that if you define a custom constructor, the default constructor will not be generated and you have to call a super constructor manually.

<div class="warning">
Super calls are not yet checked for correctness, so this might lead to runtime errors.
</div>

## Throw

```karina
throw java::io::IOException { message: "File not found" }
```

The `throw` expression can be used to throw exceptions. These have to extend `java.lang.Throwable`.

I would advise against using exceptions for control flow. Use the `Option` and `Result` types instead. 

The `Result::safeCall*` functions can be used to convert exceptions into `Result::Err` values.

<br>

# Imports

Karina uses a hierarchical import system based on the file structure.
Each file is a unit that can contain multiple structs, enums, interfaces and functions. \

```karina
import org::example::mylib::MyStruct // import the MyStruct struct inside the mylib.krna file
import org::example::mylib::* // import everything inside the mylib.krna file
import org::example::mylib::MyEnum { Case1, Case2 }
import org::example::mylib MyStaticFunctionOrField 

// collision handling
import java::util::List 
import java::awt::List as AwtList // import the List class from java.awt and rename it to AwtList
```

You are only allowed to rename imports when there is a collision. The new name has to contain the original name as a substring.