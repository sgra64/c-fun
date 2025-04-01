# Project: *c-fun*

Goal of this exercise is to set up the environment for program development
in *C* and *C++*. 


&nbsp;

## Challenge

File [*hello.c*](hello.c) contains the classic
[*hello-world*](https://en.wikipedia.org/wiki/%22Hello,_World!%22_program)
program:

```c
#include <stdio.h>

int main() {
   printf("Hello World!\n");
   return 0;
}
```

Compilation of the program creates an executable program: `hello` or
`hello.exe` (Windows):

```sh
gcc hello.c         # compile hello.c
make hello          # or: compile with the 'make' build tool
```

Run the program:

```sh
$ hello             # run the compiled program
Hello World
```


&nbsp;

## Steps

In order to perform these tasks, you need a working *C* and *C++* development
environment:

1. [*Terminal*](https://github.com/sgra64/markup/blob/main/terminal/README.md)
    -- *Mac* and *Linux* have decent terminal software installed.
    A Unix-emulator such as
    [*cygwin*](https://github.com/sgra64/markup/blob/main/setup_cygwin/README.md)
    is recommended for Windows (
        [*cygwin setup*](https://github.com/sgra64/markup/tree/main/setup_cygwin)
    ).

1. Understand the basics of [*Dotfiles*](https://github.com/sgra64/dotfiles).

1. Have an editor or IDE, e.g. [*VSCode*](https://code.visualstudio.com/download).

1. Source code management software: [*git*](https://git-scm.com/).

1. Build-tool for *C* and *C++*: [*make*](https://makefiletutorial.com/).

1. *C* and *C++* compiler, libraries and toolset.
    [*Qt*](https://www.qt.io)
    is a *C* and *C++* development environment that is particularly popular
    in the automotive industry for embedded software development.

    Download and installation requires registration. You can use the BHT
    account: `sgraupner@bht-berlin.de`, password: `NbG7fxHW#9!YgZ6`.

    Install Qt components: *Qt Creator* and *Qt Design Studio*, which includes
    the *gcc* compiler that can be added to `PATH` to use outside *Qt*.


&nbsp;

## Verification (3 Pts)

Test your environment by opening a new terminal and perform the following tasks:

```sh
# show content of the project directory
ls -la

# show versions of tools
git  --version
make --version
gcc --version

# create file and output
cat hello.c

# compile source file
gcc hello.c -o hello

# run program
hello
```

Results should show no errors:

<img src="https://github.com/sgra64/c-fun/blob/markup/img/show-versions-2.png?raw=true" width="800"/>


<!-- relative paths work for tags and branches -->
<!-- - Step 1 (tag: [*t0*](https://github.com/sgra64/se1-play/tree/t0)) - -->
<!-- - Step 1 (tag: [*t0*](../../tree/t0)) -
    initial commit with [*.gitignore*](.gitignore) `README.md` files.

- Step 2 (tag: [*t1*](../../tree/t1)) -
    commit with the [*.vscode*](.vscode) settings folder for the *VSCode* IDE.

- Step 3 (tag: [*t2*](../../tree/t2)) -
    commit with `.env.sh`, the script to *source* the project
    (see: setup the project environment).

- Step 4 (tag: [*root*](../../tree/root)) -
    commit with `src`, `tests` and `resources` folders added.

- Step 5: a separate branch: [*libs*](../../tree/libs)
    containing *.jar* - libraries are added that are required by the project. -->

<!-- 
The following steps must be performed by a developer on a laptop
for *onboarding* the project.

- Step 6, section [*Getting the Project*](#getting-the-project-se1-play).

- Step 7, section [*Project Setup*](#project-setup).

- Step 8, section [*Project Build*](#project-build).

- Step 9, section [*Running the Application*](#running-the-application).

- Summary: [*Complete Project Content*](#complete-project-content)
 -->
