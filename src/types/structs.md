# Structs

Karina uses Structs as primary data structures, allowing you to define custom data types with named fields. 
They are for now similar to Java classes, but with a more lightweight syntax.

Like in Java, struts are reference types, meaning they are stored in the heap and **always** passed by reference.


Structs define the following:
- [Constants](#constants)
- [Fields](#fields)
- [Methods & Constructors](#methods--constructors)
- [Interfaces](#interfaces)


<div class="warning">

> These **have** to be defined in this order, otherwise the compiler will throw an error.

</div>


# Constants

The first thing that can be defined in a struct are constants.
Constants are static fields that are immutable and can be used in the struct definition. They are defined using the `const` keyword.

```karina
struct Person {
    const MAX_AGE: int = 100
    const MIN_AGE: int = 0

    //...
}
```

<div class="note">

> They may have inner mutability, so be careful when using them.

</div>


# Fields




# Methods & Constructors



```karina
struct Person {
    name: mut string
    
    fn setName(self, name: string) {
        self.name = name
    }

}
```
Instance methods are defined using self as the first parameter.

Every method without self is a static method.

```karina
struct Person {
    name: string
    age: int

    fn default() -> Person = Person { name: "Karina", age: 25 }
}
let default_person = Person::default()
```
---
</br>

Each structs has a default constructor, for initializing all fields.
```karina
struct Person {
    name: string
    age: int
}
let person = Person {
    name: "Karina",
    age: 3
}
```

You can also define your own constructors

```karina
struct Person {
    name: string
    age: mut int
    fn (self, name: string) {
        self.name = name
        self.age = 0
    }
}
let person = Person {
    name: "Karina"
}
person.age = 4
```



<div class="note">

> A default constructor will not be created if you define your own constructors.

</div>

</br>

# Interfaces

Interfaces are the last parameter in the struct definition. They behave exactly like in Java

```karina
struct Person {
    name: string
    age: int

    impl Serializable 

    impl AutoCloseable {
        fn close(self) {
            // ...
        }
    }
}
```
