# Functions and Closures 

Karina supports **closure** and **function types** as a powerful way to define and work with anonymous functions. These functions are treated as interfaces under the hood, allowing for rich type inference, interface implementation, and seamless integration into the Java type system.

---

## Defining Anonymous Functions

Anonymous functions in Karina are defined using the `fn` keyword. Every anonymous function is, in essence, a class that implements one or more interfaces.

Here's an example where the function explicitly implements an interfaces:

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

If no interface is explicitly defined, Karina will infer the function's type and automatically use an existing interface during compilation. For instance:

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
- Since no interface is specified, Karina will default to a default interface (karina.lang.function.Function2_1) that matches the function's signature. Some build in interfaces extend common java interfaces, so they can be used in some java functions, when the interface cannot be inferred automatically.

```karina
import java::util::function::UnaryOperator

fn call() {
    // fn(bool) -> bool impl UnaryOperator<bool>, karina::lang::function::Function1_1<bool, bool>
    operate(fn (x: bool) { 
        return !bool 
    }, 1)

    //UnaryOperator<bool> is inferred as the interface from the operate function. 
    //karina::lang::function::Function1_1 is the default interface for one parameter and one return type.
}

fn operate<T>(function: UnaryOperator<T>, element: T) -> T {
    function.apply(element)
}

```

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
- The type of `x` is inferred based on how the function is used.
- The return type is deduced as the same type of `x`.

---

Karina ensures that every function can fulfill its role in multiple contexts by allowing it to implement multiple interfaces. 
You can these functions in java code and Karina code almost seamlessly.

