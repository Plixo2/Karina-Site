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

//import the pow function from core/math.krna inside main.krna
import pow core.math 

//import all items from main.krna inside core/math.krna
import * main 
```

### Referring to Imported Objects

Once imported, objects can be referenced either by their name or their full path from the src directory. You can import static functions, structs, interface and enums

---

## Importing Java Classes

Java classes can be imported using the `java::<path>` syntax. Unlike Karina units, Java classes cannot be referred to by their full qualified path. You can only use them by their short name, if imported.

Example:

```karina
import java::com.google.gson.Gson // Use the com.google.gson.Gson class by its short name Gson
```

### Aliasing Java Imports

To handle name conflicts between Java classes, you can use aliases by specifying a new name for the import.

Example:

```karina
import AWTList java::java.awt.List   // Alias as AWTList
import List java::java.util.List     // Alias as List
```

Now, you can use `AWTList` and `List` to differentiate between the two classes.

> Note:
> Use aliases only when absolutely necessary. Over-aliasing, like renaming `java.util.String` to `Text`, can confuse other developers and make your code harder to follow.
