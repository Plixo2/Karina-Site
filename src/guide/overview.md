

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

1. [Functions](#functions)
2. [Expressions](#expressions)
3. [Structs](#structs)
4. [Enums](#enums)
5. [Closures](#closures)
6. [Interfaces](#interfaces)
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
A default constructor and a `toString` method is automatically created. 

```karina
let v = Vec2 { x: 1.0, y: 1.5 }
println(v) // Vec2{x=1.0, y=1.5}
```

Structs, Fields and Functions are private by default. Use the `pub` keyword to make them public.
Fields are also immutable by default. Use the `mut` keyword to make them mutable.
Local Variables are mutable by default. 

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
struct User {
    name: string
    
    impl ToStr {
        fn toStr(self) -> string {
            let name = self.name
            'User{name=$name}'
        }
    }
}
```

<br>

# Expressions



- Variables
- If
- Creating Enums and objects
- String interpolation
- Closures
- Function calls
- Unwrapping
- Arrays
- `using` expression
- Cast
- instance check
- Literals
- Statics and Paths
- while, for, continue, break
- Super calls
- Throw

<br>

Generally speaking, everything is a expression in Karina.

```karina
let counter = {
    let value: int = 0
    if value > 0 { value } else { 0 }
}
```

### Examples

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