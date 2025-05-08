
# Quick Start Guide


## 1. Installation

#### Download Precompiled Binaries
If you want to avoid building the compiler yourself, you can download the precompiled binaries:

[Download Precompiled Binaries from GitHub](#)


#### Build from Source
If you'd prefer to compile the program yourself, you can follow the steps provided in the repository's [README](#).

### Post-Download Instructions:
1. Move the executable to a convenient location.
2. Add it to your system's `PATH`.


To verify the installation, run the following command:

```bash
>> karina --version
```


## 2. Create a new Project

You can use the karina CLI to create a new project with the following command:

```bash
>> karina new example
```

Your project folder should now look like this:
```
example/
└── src/
    └── main.krna
```

The `main.krna` file should contain the following code:


```karina
fn main(args: [string]) {
    println("Hello World!")
}
```

You can change the current directory into the new project folder:

```bash
>> cd example
```

Then, you can run the program with the following command:

```bash
>> karina run
```
```
Hello World!
```


See [the CLI documentation](cli.md) for more information on the `karina` command.



## Learning



If you're already a Java or Kotlin developer, follow the [*Getting started for Java developers*](starters.md) guide to get up to speed with Karina's syntax and features.

Or quickly learn the essentials of the Karina programming language with our beginner tour to grasp the fundamentals: 

[*Karina Tour*](../under_construction.md)


