# Setting up a dev container for Go

* Primary author: [Matseoi](https://github.com/JukTong/)
* Reviewer: [Dennis Wang](https://github.com/DennisComp210)
* Reference:
  * [Starting a Static Website Project with MkDocs](https://comp423-25s.github.io/resources/MkDocs/tutorial/?h=mkdocs+tutorial)
  * [Tutorial: Get started with Go](https://go.dev/doc/tutorial/getting-started)


## Prerequisites

Before we dive in, make sure you have:

1. **A GitHub account:** If you don’t have one yet, sign up at [GitHub](https://github.com).
2. **Git installed:** [Install Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) if you don’t already have it.
3. **Visual Studio Code (VS Code):** Download and install it from [here](https://code.visualstudio.com/).
4. **Docker installed:** Required to run the dev container. [Get Docker here](https://www.docker.com/products/docker-desktop).
5. **Command-line basics:** Your COMP211 command-line knowledge will serve you well here. If in doubt, review the Learn a CLI text!

## Part 1. Project Setup: Creating the Repository

### Step 1. Create a Local Directory and Initialize Git

(A) Open your terminal or command prompt.

(B) Create a new directory for your project. (Note: Of course, if you'd like to organize this tutorial somewhere else on your machine, go ahead and change into that parent  directory first. By default this will be in your user's home  directory.):

```
mkdir go-dev-tutorial
cd go-dev-tutorial
```

(C) Initialize a new Git repository:

```
git init
```

(D) Create a README file:

```
echo "# COMP 423 notes for Go" > README.md
git add README.md
git commit -m "Initial commit with README"
```

### Step 2. Create a Remote Repository on GitHub

(1) Log in to your GitHub account and navigate to the [Create a New Repository](https://github.com/new) page.

(2) Fill in the details as follows:

- **Repository Name:** `go-dev-tutorial`
- **Description:** "Course notes organized as a static website using Material for MkDocs."
- **Visibility:** Public

(3) Do not initialize the repository with a README, .gitignore, or license.

(4) Click **Create Repository**.

### Step 3. Link your Local Repository to GitHub

(1) Add the GitHub repository as a remote:

```
git remote add origin https://github.com/<your-username>/go-dev-tutorial.git
```

Replace `<your-username>` with your GitHub username.

(2) Check your default branch name with the subcommand `git branch`. If it's not `main`, rename it to `main` with the following command: `git branch -M main`. Old versions of `git` choose the name `master` for the primary branch, but these days `main` is the standard primary branch name.

(3) Push your local commits to the GitHub repository:

```
git push --set-upstream origin main
```

(4) Back in your web browser, refresh your GitHub repository to see that the same commit you made locally has now been *pushed* to remote. You can use `git log` locally to see the commit ID and message which should match the ID of  the most recent commit on GitHub. This is the result of pushing your  changes to your remote repository.

## Part 2. Setting Up the Development Environment

### Step 1. Add Development Container Configuration

1. In VS Code, open the `go-dev-tutorial` directory. You can do this via: File > Open Folder.
2. Install the **Dev Containers** extension for VS Code.
3. Create a `.devcontainer` directory in the root of your project with the following file inside of this "hidden" configuration directory:

**`.devcontainer/devcontainer.json`**

The `devcontainer.json` file defines the configuration for your development environment. Here, we're specifying the following:

- **`name`**: A descriptive name for your dev container.
- **`image`**: The Docker image to use, in this case, the latest version of a Python environment. [Microsoft maintains a collection of base images for many programming language environments](https://hub.docker.com/r/microsoft/vscode-devcontainers), but you can also create your own!
- **`customizations`**: Adds useful configurations to VS Code, like installing the Python extension. When you search for VSCode extensions on the marketplace, you will find the  string identifier of each extension in its sidebar. Adding extensions here ensures other developers on your project have them installed in  their dev containers automatically.

```
{
    "name": "COMP423 Course Notes for Go",
    "image": "mcr.microsoft.com/vscode/devcontainers/go:latest",
    "customizations": {
      "vscode": {
        "settings": {},
        "extensions": ["golang.go"]
      }
    }
}
```

### Step 2. Reopen the Project in a VSCode Dev Container

Reopen the project in the container by pressing `Ctrl+Shift+P` (or `Cmd+Shift+P` on Mac), typing "Dev Containers: Reopen in Container," and selecting  the option. This may take a few minutes while the image is downloaded  and the requirements are installed.

*After this point, you are using a base Go image from Microsoft and have installed the official Go VSCode Plug-in.*

------

## Part 3. Start Writing Go

### Dependency Tracking through the `mod` Command

In Go, the `mod` system (i.e., **Go Modules**) is used for dependency management and versioning. It enables Go projects to track and manage dependencies explicitly, ensuring reproducibility and consistency across different environments.

For demonstration purposes, we will follow the official guide using the example module `example/hello`.

```cmd
$ go mod init example/hello
go: creating new go.mod: module example/hello
```

This command initializes a new Go module in the current directory, creating a `go.mod` file that records module metadata.

### Writing Your First "Hello, World" Program in Go

Open any text editor, create a file named `hello.go`, and add the following code:

```go
package main  

import "fmt"  

func main() {  
    fmt.Println("Hello COMP423")  
}
```

This simple Go program:

- Defines a `main` package (the entry point for a Go executable).
- Imports the `fmt` package, which provides formatted I/O.
- Implements the `main()` function that prints `"Hello COMP423"` to the console.

### Running Your Code

There are two primary ways to execute Go programs: using `run` or `build`. Let’s explore the differences between them.

#### Method 1: `run`

The `go run` command compiles and immediately executes the Go program without creating an executable file. This method is useful for quick testing or scripting.

```cmd
$ go run hello.go
Hello COMP423
```

Since `go run` compiles and runs the code in a single step, it does not produce a separate binary file.

#### Method 2: `build`

The `go build` command compiles the Go program and generates an executable file, which can be executed independently.

```cmd
$ go build hello.go
$ ls
hello    hello.go    go.mod
```

Now, you can run the compiled binary:

```cmd
$ ./hello
Hello COMP423
```

### Summary

- Use `go run` for quick execution without generating an executable file.
- Use `go build` to compile and generate a standalone executable.
- Go modules (`go mod`) help manage dependencies efficiently.

Now that you've successfully written and executed your first Go program, you're ready to explore more advanced topics like functions, error handling, and concurrency! 🚀