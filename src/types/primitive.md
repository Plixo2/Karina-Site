# Primitives

Karina provides a all Java primitives, ensuring straightforward interoperability.

---

## Supported Primitive Types

Karina includes all the standard Java primitive types:

- **`int`**: A 32-bit signed integer. Ideal for most numerical operations.
- **`long`**: A 64-bit signed integer for larger numerical ranges.
- **`short`**: A 16-bit signed integer, useful for compact storage.
- **`byte`**: An 8-bit signed integer, often used for raw data manipulation.
- **`char`**: A 16-bit Unicode character type, representing single characters.
- **`bool`**: A boolean type representing `true` or `false`. Note: Karina uses `bool` (not `boolean`)
- **`float`**: A 32-bit floating-point number, suitable for less precise decimal values.
- **`double`**: A 64-bit floating-point number, for high-precision decimal calculations. Use this for most floating-point operations, as you benefit from the higher precision without a significant performance hit on modern hardware.



---

## Strings in Karina

> Both `java.lang.String` and `string` refer to the same underlying type and can be used interchangeably in Karina.

Strings in Karina are defined using quotation marks (`"`). As expected, escape characters such as `\n` for newlines or `\"` for embedding quotes are fully supported.

### Interpolating Strings

Karina supports **string interpolation**, allowing you to embed variables and expressions directly within a string using the `$` syntax. Interpolation works in two forms:

1. **Variable Interpolation**: Embed a variable by prefixing it with `$`.
2. **Expression Interpolation**: Use `${}` to embed more complex expressions.


```karina
let name = "Karina"
let age = 3

// Variable interpolation
let greeting = "Hello, $name!" 
println(greeting) // Output: Hello, Karina!

// Expression interpolation
let info = "I am ${age + 1} years old."
println(info) // Output: I am 4 years old.
```

### Escaping Interpolation

If you need to include a literal `$` in your string, escape it using a backslash (`\`).

```karina
let message = "This is not an interpolation: \$name"
println(message) // Output: This is not an interpolation: $name
```



## Joining Strings

You can join two strings in Karina using the `++` operator. If one or both of the values are not strings, their `toString` method is automatically called to convert them into a string.


```karina
let part1 = "Hello "
let part2 = "World!"

// Joining two strings
let combined = part1 ++ part2
println(combined) // Output: Hello World!

let result = 1 ++ 2
println(result) // Output: 12.
```
