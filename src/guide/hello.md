
# Quick Start Guide


## Requirements

- Java 23+

<br>

<details open>

<summary class="biggerText">Visual Studio Code Extension</summary>


- Install the extension from the [VSCode Marketplace](https://marketplace.visualstudio.com/items?itemName=karina.karina-lsp) or search for "Karina" in the extensions tab of VSCode.
- Download the [Karina Language Server](https://github.com/Plixo2/KarinaC/releases/latest/download/karina-lsp.jar) and configure the path in the extension settings (`karina.lspLocation`).

- Create a new folder called `src` in your workspace and create a `main.krna` file in it with the following content:
  ```karina
  fn main(args: [string]) {
      println("Hello, World!")
  }
  ```
- Create a `karina-build.json` file in the root of your workspace with the following content:
  ```json
  {
    "source": "src"
  }
  ```
- Set a Keybind for the `karina.run.main` command or use the command palette to run.

<br>

</details>

<details>

<summary class="biggerText">Local Installation</summary>

[Download the installer here](https://github.com/Plixo2/KarinaC/releases/latest)

If you'd prefer to compile the program yourself, you can follow the steps provided in the repository's [README](https://github.com/Plixo2/KarinaC/).

---

<br>

To verify the installation, run the following command:

```
>> karina --version
```

#### Create a new Project

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


</details>

## Features 
Take a look at the features and syntax [here](overview.md)

## Tooling 

Install the [Karina Language Server](https://github.com/Plixo2/Karina-VSCode?tab=readme-ov-file#karina-vscode-extension) for VSCode



