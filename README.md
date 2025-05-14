<!-- check-out single branch from repo:
        git clone -b B12 --single-branch https://github.com/sgra64/mdse.git
 -->

# Assignment B1: C-Data


The assignment demonstrates the use of data structures by the example of a
list type that stores consecutively addressable elements. The type dynamically
expands as elements are added.

```c
/**
 * Data structure of a list type with consecutively addressable elements
 * that dynamically expands as elements are added.
 * List elements are of a simple type (int) or are sub-lists.
 * 
 * Examples:
 *  - simple list: [1, 2, 3]
 *  - nested list of lists: [[4, 5], [6]]
 */
struct list {
    int cap;            // capacity
    int len;            // current length
    int is_simple;      // is simple list, e.g. [1, 2, 3] using idata[]
    union {
        int (*idata)[];             // pointer to array of int
        struct list *(*ldata)[];    // pointer to array of (struct list *)
    } data;
};
```

Methods of the type are:

```c

/**
 * Create empty simple list.
 */
extern struct list *list_create();

/**
 * Create simple list initialized with values.
 */
extern struct list *list_from_values(int argn, int values[]);

/**
 * Create empty list that can store sub-lists.
 */
extern struct list *list_with_sublists();

/**
 * Create copy of list.
 */
extern struct list *list_copy(struct list *list);

/**
 * Add simple element to list.
 */
extern struct list *list_add(struct list *list, int e);

/**
 * Add list to list with sub-lists.
 */
extern struct list *list_add_list(struct list *list, struct list *e);

/**
 * Print list.
 */
extern void list_print(struct list *list);
```


&nbsp;

Steps

1. [Setup](#1-setup)

1. [Create Source Code](#2-create-source-code)

1. [Build and Run](#3-build-and-run)

1. [Functions](#4-functions)

1. [Refactoring](#5-refactoring)



&nbsp;
## 1. Setup

Create a project folder named `b1-data` and within a file named `.gitignore`:

```
#------------------------------------------------------------------------------
# .gitignore contains patterns of files and directories to be ignored by git.
# #
# see: https://www.atlassian.com/git/tutorials/saving-changes/gitignore
# example: https://github.com/github/gitignore/blob/master/Java.gitignore
#------------------------------------------------------------------------------

# allow README.md to be deleted in local project
README.md

# Compiled Object files
*.o
*.obj

# Compiled Dynamic libraries
*.so
*.dll

# Compiled Static libraries
*.a
*.lib

# Executables
*.exe
*.out
```

Initialize the project as a *git* project:

```sh
git add .gitignore                  # stage '.gitignore' file
git commit -m "add .gitignore"      # commit '.gitignore' file

git tag t1                          # tag commit with 't1'

git log --oneline                   # show commit log/history
```



&nbsp;

## 2. Create Source Code

Create source files:

- `list.h` (from content above) and [list.c](list.c),

- create main file `main.c`,

- create `makefile`.

`main.c`:

```c
#include <stdio.h>      // printf
#include "functions.h"

extern struct list *pset(struct list *list);

// static data stored in DATA segment
int d1[] = {1, 2, 3};

int main() {
    printf("Hello World!\n");
    // 
    // local data stored on stack
    int d2[] = {8, 5, 4, 7, 6, 9};
    // 
    // create lists 'l1' and 'l2' from data
    struct list *l1 = list_from_values(sizeof(d1)/sizeof(int), d1);
    struct list *l2 = list_from_values(sizeof(d2)/sizeof(int), d2);
    // 
    // create list of sub-lists 'l1' and 'l2'
    struct list *l3 = list_with_sublists();
    list_add_list(l3, l1);
    list_add_list(l3, l2);

    printf("l1: "); list_print(l1); printf("\n");
    printf("l2: "); list_print(l2); printf("\n");
    printf("l3: "); list_print(l3); printf(" - (list of sub-lists 'l1' and 'l2')\n");
    printf("---\n");
    //
    return 0;
}
```

&nbsp;

Read about C and C++'s build tool [*makefile*](https://makefiletutorial.com).

Create a text file with name `makefile` in the project directory:

```makefile
# simple makefile following instructions in
# https://www.cs.colby.edu/maxwell/courses/tutorials/maketutor/
#
# a more comprehensive survey of 'make' and 'makefile' is
# https://makefiletutorial.com/

CC = cc
CFLAGS =

# rule to make .o files from .c files
# make sure to use TAB '\t' to ident the rule code
%.o: %.c
    $(CC) -c $< $(CFLAGS) -o $@
#^^^ --> use TAB '\t' for indent

# main goal
all: main

# link compiled object files to 'main' executable file
main: main.o list.o
    $(CC) -o $@ $^ $(CFLAGS) $(LIBS)
#^^^ --> use TAB '\t' for indent

main.o: main.c list.h
list.o: list.h list.c


# phony goals are used for commands and ignore files of the same name
.PHONY: all clean

clean:
    rm -f *.o main main.exe
```



&nbsp;

The project should have the following content:

```sh
ls -la                  # show project content
```
```
total 44
drwxr-xr-x 1 svgr2 Kein    0 Apr 17 09:50 .
drwxr-xr-x 1 svgr2 Kein    0 Apr 17 09:29 ..
drwxr-xr-x 1 svgr2 Kein    0 Apr 17 09:50 .git
-rw-r--r-- 1 svgr2 Kein  591 Apr  2 00:31 .gitignore
-rw-r--r-- 1 svgr2 Kein 4244 Apr 17 09:45 list.c
-rw-r--r-- 1 svgr2 Kein 1358 Apr 17 09:42 list.h
-rw-r--r-- 1 svgr2 Kein  565 Apr 17 09:48 main.c
-rw-r--r-- 1 svgr2 Kein  571 Apr 17 09:50 makefile
```



&nbsp;
## 3. Build and Run

`make -n` shows commands that would be executed:

```sh
make -n                 # test-build the project
```
```
cc -c main.c  -o main.o
cc -c list.c  -o list.o
cc -o main main.o list.o
```

Build the project

```sh
make                    # build the project
ls -la
```
```
total 124
drwxr-xr-x 1     0 Apr 17 10:04 ./
drwxr-xr-x 1     0 Apr 17 09:29 ../
drwxr-xr-x 1     0 Apr 17 09:50 .git/
-rw-r--r-- 1   591 Apr  2 00:31 .gitignore
-rw-r--r-- 1  4244 Apr 17 09:45 list.c
-rw-r--r-- 1  1358 Apr 17 09:42 list.h
-rw-r--r-- 1  2961 Apr 17 10:04 list.o
-rw-r--r-- 1   565 Apr 17 09:48 main.c
-rwxr-xr-x 1 69899 Apr 17 10:04 main.exe*
-rw-r--r-- 1  1335 Apr 17 10:04 main.o
-rw-r--r-- 1   571 Apr 17 09:50 makefile
```

Run the *main* program or *make* and *run* the program:

```sh
main                    # run the main program

make && main            # make and run the program
```
```
Hello World!
l1: [1, 2, 3]
l2: [8, 5, 4, 7, 6, 9]
l3: [[1, 2, 3], [8, 5, 4, 7, 6, 9]] - (list of sub-lists 'l1' and 'l2')
```

Commit the project in steps with:

1. `makefile` (commit) followed by

1. `*.[ch]` files.

Show the commit log:

```sh
git log --oneline                   # show commit log/history
```



&nbsp;
## 4. Functions

Implement the following functions in separate files and integrate
into `makefile`:

```c
#ifndef FUNCTIONS
#define FUNCTIONS

#include <stdio.h>  // declares: printf, NULL
#include "list.h"


/**
 * Return same list with elements reversed (in-place).
 * Example: [1, 2, 3] -> [3, 2, 1]
 * @param list list to reverse
 * @return same list reversed
 * 
 * implemented in file: reverse.c
 */
extern struct list *list_reverse(struct list *list);

/**
 * Return the sum of a list, including nested sub-lists.
 * Example: [1, 2, 3] -> 6; [[1, 2, 3], [4, 5]] -> 15
 * @param list list to reverse
 * @return sum of elements of list
 * 
 * implemented in file: sum.c
 */
extern int list_sum(struct list *list);

/**
 * Return same list with elements sorted (in-place).
 * Example: [8, 4, 7, 2, 2] -> [2, 2, 4, 7, 8]
 * @param list list to sort
 * @return same list sorted
 * 
 * You may use C's built-in qsort() function:
 * https://stackoverflow.com/questions/1787996/c-library-function-to-perform-sort
 * 
 * implemented in file: sort.c
 */
extern struct list *list_sort(struct list *list);

/**
 * Return a new list with the powerset of the input list (simple
 * list only).
 * Example: [1, 2, 3] ->
 *      [[], [1], [2], [3], [1, 2], [2, 3], [1, 3], [1, 2, 3]]
 * @param list input for powerset
 * @param return new sub-list with powerset of input elements
 * 
 * implemented in file: pset.c
 */
extern struct list *list_pset(struct list *list);

#endif //FUNCTIONS
```

Implement functions in respective files:

- `reverse.c`

- `sum.c`

- `sort.c`

- `pset.c`

Show demo code:

```c
int main() {
    // ...
    // 
    printf("Hello World!\n");
    printf("l1: "); list_print(l1); printf("\n");
    printf("l2: "); list_print(l2); printf("\n");
    printf("l3: "); list_print(l3); printf(" - (list of sub-lists 'l1' and 'l2')\n");
    printf("---\n");
    //
    printf("reverse(l1) --> "); list_print(list_reverse(l1)); printf("\n");
    printf("reverse(l2) --> "); list_print(list_reverse(l2)); printf("\n");
    printf("reverse(l3) --> "); list_print(list_reverse(l3)); printf("\n");
    printf("---\n");
    //
    printf("sum(l1) --> %d\n", list_sum(l1));
    printf("sum(l2) --> %d\n", list_sum(l2));
    printf("sum(l3) --> %d\n", list_sum(l3));
    printf("---\n");
    //
    printf("sort(l1) --> "); list_print(list_sort(l1)); printf("\n");
    printf("sort(l2) --> "); list_print(list_sort(l2)); printf("\n");
    // printf("sort(l3) --> "); list_print(list_sort(l3)); printf("\n");   // cannot sort list of sub-lists
    printf("---\n");
    //
    printf("pset(l1) --> "); list_print(list_pset(l1)); printf("\n");
    //
    return 0;
}
```

Running the code should produce:

```
Hello World!
l1: [1, 2, 3]
l2: [8, 5, 4, 7, 6, 9]
l3: [[1, 2, 3], [8, 5, 4, 7, 6, 9]] - (list of sub-lists 'l1' and 'l2')
---
reverse(l1) --> [3, 2, 1]
reverse(l2) --> [9, 6, 7, 4, 5, 8]
reverse(l3) --> [[9, 6, 7, 4, 5, 8], [3, 2, 1]]
---
sum(l1) --> 6
sum(l2) --> 39
sum(l3) --> 45
---
sort(l1) --> [1, 2, 3]
sort(l2) --> [4, 5, 6, 7, 8, 9]
---
pset(l1) --> [[], [1], [2], [1, 2], [3], [1, 3], [2, 3], [1, 2, 3]]
```

Re-run the code for `pset()` with inputs `[1, 2, 3, 4]` and `[1, 2, 3, 4, 5]`:

```
[[], [1], [2], [1, 2], [3], [1, 3], [2, 3], [1, 2, 3], [4], [1, 4], [2, 4], [1, 2, 4], [3, 4], [1, 3, 4], [2, 3, 4], [1, 2, 3, 4]]

[[], [1], [2], [1, 2], [3], [1, 3], [2, 3], [1, 2, 3], [4], [1, 4], [2, 4], [1, 2, 4], [3, 4], [1, 3, 4], [2, 3, 4], [1, 2, 3, 4], [5], [1, 5], [2, 5], [1, 2, 5], [3, 5], [1, 3, 5], [2, 3, 5], [1, 2, 3, 5], [4, 5], [1, 4, 5], [2, 4, 5], [1, 2, 4, 5], [3, 4, 5], [1, 3, 4, 5], [2, 3, 4, 5], [1, 2, 3, 4, 5]]
```



&nbsp;
## 5. Refactoring

*Refactoring* is the process in software engineering to improve the structure
of a code base without changing its function. No new features are added, changed
or removed during refactoring. After the refactoring, the code does exactly what
it did before the refactoring.

Refactor the project such that source files (`*.h`, `*.c`) appear in the
following structure:

```sh
<b1-data>
  +-- makefile                  # central makefile
  +-- main                      # main executable produced here
  |
  +-<include>                   # source folder for header *.h files
  |  +-- list.h
  |  +-- functions.h
  |
  +-<src>                       # folder with source code, *.c files
  |  +-- main.c                 # main program
  |  |
  |  +-<functions>              # source folder for functions implementations
  |  |  +-- pset.c
  |  |  +-- reverse.c
  |  |  +-- sort.c
  |  |  +-- sum.c
  |  |
  |  +-<list>                   # folder for list implementation
  |     +-- list.c
  |
  +-<bin>                       # compiled code, binaries
     +-- list.o, main.o, pset.o, reverse.o, sort.o, sum.o
```
