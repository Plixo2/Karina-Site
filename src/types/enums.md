# Enum Type

Karina supports **Algebraic Data Types**, aka **Enums**, as a way to define a type with a fixed set of values.
Enums give you a way of saying a value is one of a possible set of values. You can encode all those values in a single enum type.

Just to be clear, a enum can only be one case at a time.


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
    let value = Option::Some { value: 42 }
    assert(value is Option::Some)
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
            let toStr = fn(str) Objects::toString(str) //null safe toString
            f self is Ok ok {
                toStr(ok.value)
            } else is Err err { //special syntax for the else case
                toStr(err.error)
            }
        }
    }
```


---


## Java Interaction

Consider this Karina code:
```karina

enum ColorSpace {
    RGB(r: float, g: float, b: float)
    CMYK(c: float, m: float, y: float, k: float)
    HSL(h: float, s: float, l: float)
}

```

This is not a special new type, but a syntactic construct that is equivalent to the following Java code:
```java

public sealed interface ColorSpace permits RGB, CMYK, HSL {

    record RGB(float r, float g, float b) implements Color {}
    record CMYK(float c, float m, float y, float k) implements Color {}
    record HSL(float h, float s, float l) implements Color {}

}

```