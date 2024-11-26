# Annotations

Annotations are a way to add metadata to your code. 

You can annotate structs, functions, enums, fields and interfaces with any annotation name.
Every annotation has an json value, but not specifying it, will result in `true`.

```karina
@MyAnnotation // = true
fn myFunction() {
    // ...
}

@MyAnnotation = { key: "value" }
enum MyEnum {
    // ...
}

@Alias = [ "MyInterface", "MyInterface2" ]
interface MyInterface {
    // ...
}
```
