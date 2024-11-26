# Interfaces

In Karina, **interfaces** are a powerful feature that define a contract of behavior for types. They act as blueprints, specifying methods that any struct implementing the interface must provide. This allows for consistent and flexible design patterns, enabling polymorphism and promoting code reusability.

Here’s a simple example of an interface:

```karina
interface Function<T, R> {
  fn apply(t: T) -> R
}
```

In this example, the `Function` interface defines a generic contract for a type that takes an input of type `T` and produces an output of type `R`. Any type implementing this interface must provide the `apply` method.

Let’s implement this interface for a struct: 

```karina
struct Incrementer {
    impl Function<int, int> {
        fn apply(t: int) -> int {
            t + 1
        }
    }
}
```

This shows how the `Incrementer` struct implements the `Function` interface by providing the `apply` method. The `Incrementer` struct can now be used wherever a `Function<int, int>` is expected.

> Note:
> Interface blocks have to be implemented at the end of the struct definition.

## Extending Interfaces

Karina also allows interfaces to extend other interfaces. This is useful when you want to create more specific contracts by building on existing ones. When an interface extends another, it inherits the methods of the parent interface. The extended interface must be explicitly declared at the end of the interface definition.

Here’s an example:

```karina
interface Getter<T> {
  fn get() -> T
  impl Supplier<T> 
}
```

In this case, the `Getter` interface extends the `Supplier` interface. Any type implementing `Getter` must also satisfy the contract defined by `Supplier`.



> Note: 
> When extending interfaces, all extended interfaces must be explicitly specified at the end of the interface definition.

