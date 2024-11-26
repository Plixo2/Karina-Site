# Functions and Closures 

Karina supports **closure** and **function types** as a powerful way to define and work with anonymous functions. These functions are treated as interfaces under the hood, allowing for rich type inference, interface implementation, and seamless integration into the type system of java.

---

## Defining Anonymous Functions

Anonymous functions in Karina are defined using the `fn` keyword. Every anonymous function is, in essence, a class that implements one or more interfaces.

Here's an example where the function explicitly implements interfaces:

```karina
// fn(a: bool) -> void
fn (a) impl Consumer<Bool>, Setter<Bool> { 
   println(a)
}
```

- **Explicit Interface Implementation**: You can specify the underlying interfaces your function should implement using the `impl` keyword. This is useful when the interface cannot be inferred automatically.
-  **Multiple Interfaces**: Functions can implement more than one interface, providing flexibility in how they interact with the type system.

---

## Implicit Interface Creation

If no interface is explicitly defined, Karina will infer the function's type and automatically create an interface during compilation. For instance:

```karina
// fn(int, bool) -> bool
let some = fn (a: int, b) { 
    b
}
let other = !some()
```

### What Happens Here:
- The type of `a` is explicitly defined as `int`, while `b`'s type is inferred.
- The return type of the function is the same as the parameter `b`.
- When using the NOT operator `!` on the function, a `bool` is expected as the return type of the function. As the return type of the function is inferred as `bool` and the return type is the same as the parameter `b`, the parameter `b` is also inferred as `bool`.
- Since no interface is specified, Karina tracks all interfaces the function is called with and generates a compatible class at compile time.

---

## Optional Parameter and Return Types

In Karina, parameter types and return types can be omitted if they are inferable:

```karina
// fn (int) -> int
let square = fn (x) { 
    x * x
}
let result = [1, 2, 3].map(square)
```

In this example:
- The types of `x` are inferred based on how the function is used.
- The return type is deduced as the same type of `x`.
- The underlying object with the `java.util.function.Function<int, int>` interface is created at compile time, as the map function expects an object with this interface.
---

Karina ensures that every function can fulfill its role in multiple contexts by allowing it to implement multiple interfaces. 
You can these functions in java code and Karina code seamlessly.

