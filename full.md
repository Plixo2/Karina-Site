<h1 align="center">Karina Language</h1>


<details open>
<summary>Guide</summary>
 
- [Introduction](#introduction)
- [Goals](#goals-for-karina)
- [Implementation](#implementation)
   - [Basic Parsing](#implementation)
   - [The Typed Tree](#tree)
   - [Stages](#phases-in-the-compiler)
      - [Import Stage](#import-stage)
      - [Variable Stage](#variable-stage)
      - [Infer Stage](#infer-stage)
      - [Rewrite Stage](#rewrite-stage)
   - [Compiling](#compilation)
- [Language Reference](#reference)

</details>

# Introduction 
Over the last few years took some interest in Programming Languages. Now i put this Language together, from what i learned over many failed projects. 


This blog is written as a source of information from what i wished i knew earlier and to show what i learned. 

The first Language i did, was a Interpreted, but staticly typed one. I rewrote the Compiler to be self hostet, but eventually was limited by my knowledge and the old codebase without any LSP or assistance. The self hostet "Schematic" Code is still up. 

So i decided to start new. This time the Compiler was written in Java. Self hosting still has to wait till i got an LSP up and running. I made the decision to make it compatible with the Java VM. That means compiling it to bytecode, witch was essentially the easy part. The more difficult part was the interaction between my language and existing jvm classes. 
I didn't implemented any Generics, as ASM didnt offer a parser for the java generic bytecode format. I eventually archived my goal tho, as i got ~90% of features with the jvm. This Language is probably the better to actually program in, as you got java support, operator overloading, extension functions, macros and other convenient functionality. Maybe i will come to this Language back again. 
The Source for "Stella" is also still up.


# Goals for Karina:

Karina is static and strongly typed language for the jvm. The goal for this Language is a clean and small codebase, better tooling and faster Compiler. The Language should eventually be "production ready" with an actual specification and an Ecosystem with an LSP. 

The Language should definitely include:
- Enums (as in Tagged Unions)
- Structs 
- No Inheritance 
- Easy Generics 
- Interfaces
- Extension Methods
- Good Pattern Matching
- Basic JVM interaction 
- Runtime Polymorphism


Optional Features:
- Perfect JVM interaction 
- Generic Bounds
- Support Safe Multithreading
- Destructors
- (User) Macros
- alternative Backend 

And finally some nice key points:
- Fun
- Productivity
- Simple but Expressiv





# Implementation 
After we designed our Language with all features we have to adopt it to a syntax. We use this syntax to identify and parse common expressions in our language

The first step for most* parser is to identify Tokens in the source text. (Insert Token definition). We need this step to allow our Lexer to build upon these discrete types.
*Parser Generators will typically combine Tokenizing and Parsing. 

An Algorithm called a Tokenizer is used to convert the Source String into a List of Token records. 

In my Language a Token is the underlying rule and name an specific lexeme will hold:

struct Token {
 name: String
 doesMatch(in: String) -> bool
}

We could use a Regex for these Rules, but we keep it abstract to allow custom logic for speed and flexibility e.g String with escape characters. 

While a TokenRecord is a matched Token for a given String:

struct Tokenrecord { 
 token: Token
 content: String
 region: Region
}

In your Language you typically have keywords, that have specific names in the Source code, or flexible words, like any other words, number or strings. That might seem trivial, but its an important distinction to make. 

For example you have a Number token:
let numberToken = new RegexToken { name: "number", regex: "[0-9]+\\.?[0-9]*"}
Or a keyword for the definition of a function:
let fnToken = LiteralToken { name: "fn" }

We define a custom name, so we can identify a token, when we defining the grammar.

The next step is The Parsing phase. 
Explain Parsing Phase

You can take many approaches when deciding on your approach 

The Parsing can be defined in a custom grammar and compiled to actual code that you can Integrate. That approach is called a Parser Generator or a "Compiler-Compiler". A pretty good Parser generator is Antlr (ANother Tool for Language Recognition). Check out their online tool http://lab.antlr.org/

The other approach is to handcraft your own Parser for this Phase.
The Parser for the Language Effect is handwritten for example. (https://github.com/effekt-lang/effekt/blob/master/effekt%2Fshared%2Fsrc%2Fmain%2Fscala%2Feffekt%2FRecursiveDescent.scala) 

This approach has multiple benefits to auto generated parsers: Better error messages, error recovery, custom objects and probably better performance if you know what your doing. 

This approach is often taken by big languages for exactly these reasons. 

The biggest drawback a handwriting parser has, is the Iteration speed when changing the syntax, as you grow your language. 

But i took an middle point of both words:
i used a custom syntax for defining my language:
...
parameters ::= (expr ("," expr!)*)?
...

Every rule can match expression with subrules and keywords. These Expressions can be grouped, made optional with "?", repeated with "*" (Zero or More) or made Necessary with "!" (An error will be thrown if an expression an not be matched)

For example the rule "parametes" will match an comma separated list of expressions. This rule will throw an error if the expression "expr" cannot be matched after an comma. 

Note that this Rule will also match against an empty input.


I used an Algorithm to read these rules and match them agains a stream of tokens. I probably will use an Handwritten Parser once i got the syntax tuned in. 

The Nature of these rules gives us an Node tree:

struct Node {
 name: string
 record: Option<TokenRecord>
 children: List<Node>
}

We use these Nodes now to create custom Data Types for our powerful Tree data structure to futher process the Language Expressions. 

connection between token, tokenrecord and parser

The best Data structure for Programming Language processing: The Tree with a Twist 

Well, think about it. A Programm consist of Packages with Files with Structs with functions with expressions with subexpression...

We will also use this as our model for structuring the codebase. We will use normal typed classes with fields. But Think about it more as a Tree, this will come in handy in a second.


## Tree
Now we got simple Types for every concept for our Language. For example the following object represents a Struct in our Language. 

struct Struct {
 region: Region
 name: String
 path: ObjectPath
 fields: List<Field> 
 functions: List<Function>
...
}

The Region Object represents a start and end position and a file where the struct is defined, so it can be used to identify where an error occured

Note that we abstract over the file system for better error reporting and flexibility. 


The Object "ObjectPath" is simply a immutable List of Strings with a special meaning. 
This List is a path from the root of our programm Tree. 

Our whole tree is Immutable and we only use the programm Tree for informations and state of our Compilation process. 

We do not keep track of our types in a global dictionary or anything else. We only use Informations that is currently in the Phase available. You might think that this limits our knowledge we could gather or resolve. 

We use the ObjectPath to locate everything. 
For Example a Type in my language is just an ObjectPath along with a mapping for generic types. 
If we need an field of an type we simply look up the Struct object on the tree and get the field from there and map all generic types along the way. 


The immutable Datastructure allows pretty good parallelism and partial Compilation. That speeds the compiler up, and allows for an easier LSP implementation. 



Now that we got the objects for our language features we can perform different transformations or "phases"


## Phases in the Compiler 

The use different Phases on our Tree to gather information, to typecheck and eventually compile. 

Our Tree is after all just objects with fields. We have to make carefull design decisions when modeling the Features of the Languages. For example a LetExpression may not have a Type yet for the Variable that it is defining. We need another Object to represent the Type some time later. 

We model this by using a Interface for tagging the current Stage, so that we have type guarantees while programming our Compiler. 

For Example the first Stage will receive an expression from type Expression<Stage1> and will return a type Expression<Stage2>.
We than can model the LetExpression as 
LetStage1Expression extendeds Expression<Stage1> 
and 
LetStage2Expression extendeds Expression<Stage2> 
respectfully 

We only use this for Expressions, not for structs, packages, fields etc


In each Stage we fully rebuild an new tree from the ground up, with the informations from the last stage.


In each Phase we got informations flowing down our Tree, with new computed and checked Information and garantees coming up. 

We got the following stages in the Compiler
 • Import Stage
 • Variable Stage
 • Infer Stage
 • Rewrite Stage


### Import Stage
This is the first Stage.
For each Unit (a File) we lookup all imports, including the automatic ones. 

We lookup the Path from the imports. All imports must point towards an Unit from the root
The we build a map for all structs, top level functions, etc to be looked up for all occurrences on an type in the items:

let typeLookUp = Map[Name -> ObjectPath]{}


We than transform items inside the unit (enums, interfaces, structs, top level functions) 
by distributing the lookup table. These Functions now look up all occurences of unqualified types and build new objects with the full qualified name replaced (a path from the root pointing to the Item)

When an already qualified type is occured 
(e.g. "java.util.List" instead of just "List") the name is  checked if its a valid path (also starting from the root). You can also implement "partial" imports here,  were you start from a Package you imported.

We also look for conflicting imports or names in this step and check for valid names of packages and files. 

### Variable Stage
In this stage, we can Link, check and create Variables. We just keep a Context with all Variables in a Scope and than replace the Expressions with a Expression Object with an Variable Object. 
A Variable is also immutable and has just a name an a Region where the Variable was defined. The type will be resolved in the next Stage

In this stage we can also determine the Environment for closures for the next Stage.

### Infer Stage
This stage is the main stage for type checking and inferring. This stage is probably the hardest and biggest stage, although we made this stage as easy as it gets, thanks to the Tree and previous Stages.

We now transform all Expressions into Objects with a type. 

For this Stage we have to implement an function for checking if type A can be assigned the a value from type B:
canAssign(type, type) -> bool
If your language has subtyping, canAssign(a, b) != canAssign(b, a), so be careful what you need. 

An even more powerful pattern, is to use a function to automatically Convert from one type to another when checking:

convertTo(expression,type) -> Option<Expression> 

You can implement Boxing/Unboxing here or make a automatic casting possible or check for functions that can convert one type to another. 

When we have these functions we can check and infer. We also now create an Context for variable types. E.g:

fn transformExpression(context: Context, expression: Expression<StageVariable>) -> (Context, Expression<StageInfer>) {
match expression {
case LetExpression e {
  let value = transformExpression(context, e.value).1
  if e.typeHint.exists {
     value = convertTo(value, e.typeHint.get).      expectWithRegion(e.region, "Type Missmatch")
  }
let newContext = context.insertVariable(e.variable, value.type)
let newLet = LetExpressionTyped(e.region, value.type, value)  
(newContext, newLet)
}
...
}

We also check here for function calls, assignment to finals, Handling of Variables in Closures and so much more.

We also ensure that a Function returns the correct value and does not return early. 


Generics are Handled by inner mutability:
struct GenericType extends Type {
 resolved: Option<Type>
} 

When calling the canAssign function with an generic type, we resolve it to the other type, if the generic was not already solved. Otherwise we test the resolved type. This can lead to cyclic values, so be carefull when doing so.

This inner mutability is pretty powerful, as one doesn't have to specify the type immediately, but be solved by the other code surrounding it automatically. This is of course not the best solution for solving Generics, but it feels pretty natural to programm with this approach.


### Rewrite Stage
This is the final stage before compilation 
Here the Compiler transforms the Closures to classes with all necessary interfaces. 
This stages also rewrites other Loops to while loops. 



## Compilation




# Language Reference 


## Structs
```rust
struct Object<T> {
  pub m: int
  w: Other
  next: Option<T>

  fn isZero() -> bool {
    self.m == 0
  }

  impl intoInterator<T> {
    fn toInterator() -> Interator<T> {
      //...
    }
  }
}
```

Structs can define fields, functions and Interfaces. 

Interfaces are Implemented with the `impl` keyword. Inside this block you can implement all functions. 

> [!IMPORTANT]
> All interfaces have to be implemented at the end of each struct. This is inforced. 


Fields are automatically private, unless specified as public. 

> [!Note]
> It is recommended, that getter and setter functions are not prefixed with "get" or "set". 
> There might be annotations for this in the future. 


## Enums
Other Languges might refer to this Datatype as _Sum types_ or _tagged unions_.

```rust
enum Option<T> {
  Some(value: T)
  None
}
```

Cases without arguments dont require parentheses. 

Every argument has to be named to access the value without destructuring. This also describes the meaning of the arguments.


When creating cases, prefix the case with the enum name: 

```rust
Option.Some("hello") // Option<string>.Some <: Option<string>
Option.None() // Option<?>.None <: Option<?>, where the type has to be infered
```


The generic type can also be specified when creating enums:

```rust
Option.None<string>() // Option<string> 
```

You still have to use the parentheses, even when the case has no arguments.


## Interfaces
```rust
interface Function<T, R> {
  fn apply(t: T) -> R
}
```

You can also extend interfaces to other interfaces. 

```rust
interface Getter<T> {
  fn get() -> T
  impl Suppllier<T> 
}
```

> [!IMPORTANT]
> All extended interfaces have to be specified at the end of the interface.
 

## Functions
```rust
fn isZero(value: int) -> bool {
  value == 0
}
```

The return keyword is not needed. The last expression in a scope is the return value. You still can use the return keyword to return a value early.

The Language also allows an short notation without an Block with "="

```rust
fn get<T>(getter: Getter<T>) -> T = getter.get()
```

> [!Note]
> It is recommended, that the short syntax is only used for one or two lines.

For Functions inside a scope, you can use the keyword `self` to refer to the object the function is called on.

```rust
struct Some {
  name: string
  fn getName(trim: bool) -> string {
    if trim {
      self.name.trim() 
    } else {
      self.name
    }
  }
}
```

## Extension Functions

Karina also allows to extend existing objects with functions. 

```rust
fn toDouble() -> double extends int {
  return self as double 
}
```

The `self` keyword is used to refer to the object the function is called on.

This can also limit the domain of the function with generics:

```rust
fn join() -> string extends [string] {
 //...
}

fn sum() -> int extends [Integer] { 
  //...
}
```

These functions can only be called on the specified type.

> [!TIP]
> Only use extension functions when you cannot modify the original object. > Overusing this feature can lead to confusion for other developers. 
> This feature only works in the current project anyway.


We also support method overloading with the same rules as in java. 

```rust
fn toString(obj: Object) -> string {
 return obj.toString()
}

fn toString(i: int) -> string {
 return String.valueOf(i)
}
```


## Imports

```java
//src/
//├── core/
//│   └── math.krna
//└── main.krna

import Pow core.math 
import * main

```

The \* will import all items from a specific file. You can import enums, structs, interfaces and functions separately.

You can refer to Objects now with the name or the full path from the root. 

Java classes have to be imported with `java::<path>` and cannot be referred with the path:

```java
import java::com.google.gson.Gson
```

You can also give an alias to java imports to avoid name conflicts:

```java
import AWTList java::java.awt.List 
import List java::java.util.List 
```

> [!TIP]
> Use name aliases only when necessary.
> Importing java.util.String as Text is just confusing for other developers.

## Expressions
First of all, you dont need the return keyword, any last value im a scope, is the value of the scope. Also there are no statements, only Expression, with some  expressions, like while having the type void. 

### If 
the if expression doesnt need parentheses. 
This expression can also return a value, if an else case is present:

let a = if true { 0 } else { 1 }

And yes, braces are needed for now. you can still use "else if" tho

if is
You can check for enum cases, or if a struct implements an interfaces

if a is Object b {
 //use a as type b
}

if a is Some(b: Bool) {
  //you can also destructure enums, the type is optional 
}

While 

while true {
 break
}
you can use break in while and you need braces


### Match
```
match option {
 case Some(arg: String) -> true
 case t : None -> false
}
```
You can destructor Enums. The Type hint for arg is Optional

Otherwise you can use "t: Type" or the default case 
"_" for Enums and Objects to test for interfaces. 

If you match an enum, all enum cases have to be satisfied. This will Always return a Value or Void

When you test for interfaces you dont need all cases satisfied. You need an default method to return a value. Otherwise the method will return void. 


### Binary 
You can of course use all binary and unary expression as you guessed with the order of
("||", "&&", ("==" | "!="), ...)

You may be able to overwrite these methods with interfaces in the future. 

Let
You can define a variable with or without a type hint
let d: double = 0.0
let i = 1 //int

The type is than infered and my be solved later


### Closure 

You can create anonymous functions. 


Every anonymous function in java is in reality a class with an interface method defined. 
You can define underlying interface, if it cannot be inferred. 

fn (a) -> void impl Consumer<Bool> { 
   println(a)
}
// the type of a can be infered as a Bool and the interface is defined

let some = fn (a: int, b) { 
 b
}
let other = !some()
The type can be infered as an bool, but not the interface. The language will create one, behind the scenes otherwise. 

We keep track of every interface a function is called on. And Then create a class with all interfaces when compiling.


The parameter Type can be optional, as well as the return type. 


### for

lists 
create a list with brackets 
[ a, b, c + 1]

This will create an Arraylist behind the scene. 

you can call the default methods on it

[1, 2].add(3)

But you can index the list with list[0] 

numbers
Numbers can be defined with binary 
0b.. or hex 0x..

Also you can use underscore to write big numbers
100_000_000
strings 
Strings can be defined with quotation marks. You can use the Escape characters "\" of course. 

You can interpolate Strings with a $name oder ${expr}. You can also escape $ with a "\".

### new

You create a new struct with its name, followed by the named parametes in braces:

Object {
m: 0
w: Other{}
next: Some("Hi")
}
The generic type can be specified after the name:
Object<T> {
...
}

### assignment 
You can set a specific element in an array with list[index] = obj

You can also reassign local variables

or fields:
object.field = value

### indexing 
You can index into java type Arrays and List with list[index]

### calling
you can call a function type or a method. 
When a method got a a Generic Type, you can define it before you call it, when it cannnot be infered:

callMe(a, b, c) 
generic<int, bool>()

That should be all expressions.

### as, in, continue

### break

### null

### 

## Keywords 

 |   |   |   |   |   |   |
 | --- | --- | --- | ---- | --- | --- |
 | fn | yield* | let | native* | if | case |
 | is | struct | matches* | string | async* | double |
 | in | while | long | short | byte | void |
 | as | false | for | char | pub | extend* | 
 | of* | virtual* | super* | override* | continue | default* | 
 | null* | break | self | final* | mut | static* |
 | import | return |  await* | package* | throw* | try* | 
 | extends* | impl | float | assert* | catch* | int |
 | match | enum | bool | true | else | 
 
 
All Keywords with an \* are reserved, but not used yet.

To use a keyword as a name, escape the keyword with an backsplash. This is mainly used for java compatibility.

## Types

Primitives:
You have of course all java primitives, like int, bool (not boolean), long, byte etc...

You can define a struct with its name or path. 
You can import a unit with import path.to.unit
Java classes can be imported with import java path.to.class

After the name of path you can specify the generic type:

Some.Type\<Integer\>

Functions can be defined with fn(TypeA, TypeB, ..) -> RType
-> RType is optional, otherwise void

[Type] is an alias for java.util.List\<Type\>

Array<Type> is the Type for an java style Array


string (lower case) is an alias for java.lang.String
You still need to use java.lang.String for the methods

## Naming and other conventions