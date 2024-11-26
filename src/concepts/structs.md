# Structs

The basic building block is the _struct_ or _structure_. A struct is a way to group together related data in a meaningful way. 


```karina
struct Album {
    title: string
    artist: string
    releaseDate: Date
    songs: [Song]
}
```

Fields are defined with a name and a type, not separated by commas.

To create a new instance of a struct, you can use the type name or path followed by curly braces with the names and values of the field. 

```karina
let album = Album {
    title: "My World",
    artist: "Artist",
    releaseDate: SimpleData {
        year: 2023,
        month: 5,
        day: 8
    },
    songs: []
}
```

All fields have to be initialized and in the correct order. There are no default values for some types like in java.

## Methods

You can create functions inside a struct. We will refer to these functions as methods troughout the documentation.

```karina
struct Album {
    title: string
    artist: string
    releaseDate: Date
    songs: [Song]

    fn shuffle() -> [Song] {
        self.songs.shuffle()
    }
}
```

To refere to the current instance of the struct with the `self` keyword inside the function.

Call these methods on the instance or by using the path to the function and passing the instance as the first argument.

```karina
let album = //...
album.shuffle()
//or
Album.shuffle(album)
```
## Interfaces

Structs can also implement interfaces with the `impl` keyword. See 
[Interfaces](interfaces.md) for more information.


## Standart Functions

The standart struct extends the `java.lang.Object` class. This means that you can call methods like `toString()` on every struct.

The compiler will automatically generate a custom `toString()` method for you. You can still override this method by defining your own `toString()` method.


The compiler will also generate `equals()` and `hashCode()` methods for you.


