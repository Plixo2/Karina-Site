
# Hello World Program

## Create a new Project

You can use the karina CLI to create a new project with the following command:

```bash
karina new example
```

Your project folder should now look like this:
```
example/
└── src/
    └── main.krna
```

The `main.krna` file should contain the following code:


```karina
import java::java.lang.System

fn main(args: Array<string>) {
    System.out.println("Hello World!")
}
```

You can change the current directory into the new project folder:

```bash
cd example
```

Than compile the program with the following command:

```bash
karina build src
```

this will create a `build` folder with the compiled program.

## Run the Program

To run the program, use the `java` command to execute the `build.jar` file.

```bash
java -jar build/build.jar
```

You should see the following output:

```
Hello World!
```