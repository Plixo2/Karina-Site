# Introduction


Karina is a statically typed language that combines the best features of functional and object-oriented programming. 
Karina is designed to run seamlessly on the Java Virtual Machine (JVM). Built with ease of use and strong interoperability in mind, Karina makes it effortless to work alongside Java and other JVM languages.

This documentation serves as your complete guide to Karina’s syntax, features, and best practices — everything you need to start building with confidence.

---

# Overview 

The following code snippet demonstrates some of the key features of Karina — clean syntax, compact code and powerful type inference.

```karina

fn main(args: [string]) {

    // Sum Types
    let get_circle_shape = fn(range) {
        Primitive::newCircle(Vector2D::ZERO, range)
    } 
    let area = get_circle_shape().area()
    println('Area: $area')

    // Java Interop
    let list = java::util::ArrayList {}
    list.add("Hello") 
    list.add(" ")
    list.add("World!")

    list.forEach(fn(ref) print(ref))
    println()

    // Java Interop with functional Interfaces 
    let emptyFilter = fn(txt: string) impl java::util::Predicate<string> {
        // '==' and '!=' call the 'equals' method, while '===' and '!==' compare identity
        txt != " "
    }

    let filted = list.stream().filter(emptyFilter).toList()


    // Result and Option Types
    let result = Result::safeCall(fn() filted.get(-1))
    let patternIf = if value is Result::OK(value) { value } else { "" }

    let failable_cast = Option::instanceOf(Primitve::Circle, get_circle_shape(5))
    let radius = failable_cast.mapOrElse(fn(circle) circle.radius, 0)
}


// Defines two suptypes 
enum Primitve {
    Circle(position: Vector2D, radius: float)
    Square(position: Vector2D, width: float, height: float)

    // Static constructor method
    fn newCircle(position: Vector2D, radius: float) -> Primitve {
        Circle { _: position, _: radius }
    }

    // manual method
    fn height(self) -> float {
        match self {
            Circle(radius) => radius * 2
            Square(width, height) => height
        }
    }

    // automatic reference to position in both cases
    fn position() -> Vector2D

    // Implements a interface 
    impl Shape {
        fn area() -> float {
            match self {
                Circle(radius) => 3.14 * radius * radius
                Square(width, height) => width * height
            }
        }
    }
}

interface Shape {
    fn area() -> float
}

struct Vector2D {
    const ZERO: Vector2D = Vector2D::new(0)
    
    x: float
    y: float


    // Static constructor method
    fn new(d: float) -> Vector2D = Vector2D { x: d, y: d }

    fn distance(self, other: Vector2D) -> float {
        let dx = self.x - other.x
        let dy = self.y - other.y
        return Math.sqrt(dx * dx + dy * dy)
    }

    //...
}



```