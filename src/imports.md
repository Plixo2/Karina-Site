# Imports

Karina’s import system provides a flexible and clear way to include external files and Java classes in your program. You can import specific items, entire files, or Java classes, with options for aliasing to handle naming conflicts.

---

## Importing Units

Karina files can be organized into directories and files, and imports allow you to bring specific items or entire units into your current scope.

### Import Syntax

- **Import Specific Items**: Import an item by specifying its name.
- **Import Everything**: Use `*` to bring in all items from a specific units.

Example:

```karina
// File structure:
// src/
// ├── core/
// │   └── math.krna
// └── main.krna

//import the pow function from src/core/math.krna
import src.core.math pow 
import src.main { println, exit, main } //import multiple items

import src.main.Substruct { staticFunction } //import static functions inside a struct, inside a file

//import all items from src/core/main.krna, this includes, structs, enums, interfaces etc
import src.core.math * 
```

### Referring to Imported Objects

Once imported, objects can be referenced either by their name or their full path from the src directory. You can import static functions, structs, interface and enums

---

## Importing Java Classes

Java classes can be imported using the exact same syntax. 

Example:

```karina
import com.google.gson.Gson // Use the com.google.gson.Gson class by its short name Gson

// fully qualified names are still supported
fn toJson(value: string) -> com.google.gson.JsonElement {
    // ...
}
```

### Aliasing Java Imports

To handle name conflicts between Java classes, you can use aliases by specifying a new name for the import.
This feature is only available if you have ambiguous name conflicts of classes. Aliasing Types without conflicts is not supported, since it can lead to confusion.

Example:

```karina
import java.awt.List as AWTList   // Alias as AWTList
import java.util.List as List  // Alias as List
```

Now, you can use `AWTList` and `List` to differentiate between the two classes.

> The Compiler also checks if aliased name contains the conflicted class name. 
> For example you cannot alias `java.awt.List` as `Text` since it doesnt contains the conflicted class name `List`.

