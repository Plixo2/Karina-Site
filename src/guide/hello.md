
# Quick Start Guide



## 1. Installation

[Download the installer here](https://github.com/Plixo2/KarinaC/releases/latest)

If you'd prefer to compile the program yourself, you can follow the steps provided in the repository's [README](https://github.com/Plixo2/KarinaC/).

---

<br>

To verify the installation, run the following command:

```
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
    println("Hello, World!")
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
Hello, World!
```

Use the `--help` flag to see all available commands

## Features 
Take a look at the features and syntax [here](overview.md)

## Tooling 

The Language Server was removed until Karina is more stable.


[This](https://github.com/Plixo2/KarinaC/tree/master/resources/karina) Textmate Grammar can be used for simple syntax highlighting in supported editors, like VSCode or IntelliJ.

## Learning



If you're already a Java or Kotlin developer, follow the [*Getting started for Java developers*](starters.md) guide to get up to speed with Karina's syntax and features.

Or quickly learn the essentials of the Karina programming language with our beginner tour to grasp the fundamentals: 

[*Karina Tour*](../under_construction.md)


