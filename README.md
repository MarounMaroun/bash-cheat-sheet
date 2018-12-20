# Index

1. [Introduction](#introduction)
    * [What is Bash?](#what-is-bash)
    * [Where Bash scripts run?](#where-bash-scripts-run)
    * [The Shebang (#!)](#the-shebang)
1. Variables
    * Setting / Reading variables
    * Special variables
    * Command line arguments
    * Quotes
    * Exporting variables
    * Types of variables
    * Arrays variables
2. Input
    * Reading input from STDIN
    * Asking user for input
3. Arithmetic
    * `declare`
    * `expr`
    * `let`
4.  Conditional statements
    * `if` statements
    * `test`
    * Boolean operations
    * `case` statements
5. Repetitive tasks
    * `for` loops
    * `while` loops
    * `until` loops
6. Functions
7. Signals


## Introduction

### What is bash?

Short and fast definition: "Bash" is **B**ourne **A**gain **SH**ell. It's a command line interpreter that executes commands from the standard input (STDIN) and is capable to run commands from a file (called a shell script).

### Where Bash scripts run?

When we run a script, the Bash process starts a new process in which the script runs.

For example, if we have three terminals, and each runs a command, there will be *three* processes running the command.

### The Shebang

When we execute a command, the OS needs to know how to execute it. The Shebang is a path to the Bash interpreter. For example:

```bash
#!/bin/bash
```

Note that it doesn't have to be an absolute path, but since you'll be running your script from different locations, providing the absolute path would be the safest option.

Once the **first** line is set to `#!/bin/bash`, the content of the script will be passed to the `/bin/bash` program to be executed.

Example:

```bash
#!/bin/bash

echo $PATH
```

If we now run  `./test.sh`, the `echo $PATH` command will be passed to the `/bin/bash` program, which is the same as running:

```bash
/bin/bash test.sh
```


