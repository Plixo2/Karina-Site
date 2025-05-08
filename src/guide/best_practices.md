
# Best Practices and Conventions

To ensures that your code is consistent in other JVM languages that might be used in the same project, following the [Java Naming Conventions](https://www.oracle.com/java/technologies/javase/codeconventions-namingconventions.html) is recommended. 
</br>
This however does not include local variables:

The names of local variables define inside methods should be all lowercase with words separated by underscores ("_"), otherwise referred to as `snake_case`:

```karina
fn exampleFunction(listLength: int) {
    let new_list = ArrayList<Integer> { initialCapacity: listLength }
}
```

A single underscore can be used for unused variables and parameters:

```karina
fn exampleFunction(_: int) {
    let _ = 1 // Unused variable
}
```








