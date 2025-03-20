# Enum Type

Karina supports **sum types**, aka **enums**, as a way to define a type that can have a fixed set of values. Enums are a powerful way to represent a set of related constants, and they can be used to define a type that can only have a specific set of values.

Just to be clear, a enum can only be one case at a time.

```karina

enum Color {
    Red
    Green
    Blue
}

```

This is not a special new type, but a syntactic construct that is equivalent to the following Java code:
```java

public sealed interface Color permits Red, Green, Blue {

    record Red() implements Color {}
    record Green() implements Color {}
    record Blue() implements Color {}

}

```

---

## Enum with Associated Values

Enums can also have associated values, which can be used to store additional information about each enum value. This is useful when you want to associate some data with each enum value.

```karina
enum Option<T> {
    Some(value: T)
    None

    fn orElse(self, defaultValue: T) -> T {
        if this is Some {
            self.value
        } else {
            defaultValue
        }
    }
}
```

To construct a specific value just instantiate the enum variant like you do with classes

```karina
    let value = Option.Some { value: 42 }
    assert(value is Option.Some)
    assert(value is Option)
```



---

## Special handling of sealed types

Since all possible types are known at compile time, Karina can check if all branches of a match or an if case match are covered.

Example:

```karina
    enum Result<T, E> {
        Ok(value: T)
        Err(error: E)

        fn getAsString(self) -> string {
            let toStr = fn(str) Objects.toString(str) //null safe toString
            f self is Ok ok {
                toStr(ok.value)
            } else is Err err { //special syntax for the else case
                toStr(err.error)
            }
        }
    }
```


---
