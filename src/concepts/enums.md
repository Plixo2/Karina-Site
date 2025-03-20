# Enums

Enums, also known in other languages as _Sum Types_ or _Tagged Unions_, are a versatile and powerful datatype in Karina. They allow you to define a set of distinct cases, each of which can optionally carry associated data. Here's how enums work in Karina, along with some unique characteristics that set them apart.

## Defining an Enum

In Karina, enums are declared using the `enum` keyword. Each case can have named arguments or no arguments at all. Importantly, all arguments in a case **must be explicitly named**, a feature that ensures clarity and consistency.

```karina
enum Option<T> {
  Some(value: T)
  None
}
```

In the example above:

- `Option.Some` represents a case that includes a value of type `T`.
- `Option.None` represents a the absence of a value.

We than use the `Option` enum to represent a value that may or may not be present. Only one of the two cases can be true at any given time.

Cases without arguments do not require parentheses in their definition.

---

## Instantiating Enums

When creating an instance of an enum case in Karina, prefix the case name with the enum's name. **Parentheses are always required**, even for cases without arguments.

```karina
Option.Some("hello") // Option<string>
Option.None()        // Option<?>, where the type can be inferred later
```

### Specifying Generic Types

If the generic type cannot be inferred, you can explicitly specify it during instantiation:

```karina
Option.None<string>() // Option<string>
```

