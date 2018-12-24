# Index

1. [Introduction](#introduction)
    * [What is Bash?](#what-is-bash)
    * [Where Bash scripts run?](#where-bash-scripts-run)
    * [The Shebang (#!)](#the-shebang)
1. [Variables](#variables)
    * [Setting / Reading variables](#setting--reading-variables)
    * [Special variables](#special-variables)
    * [Command line arguments](#command-line-arguments)
    * Local variables
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

## Variables

Variables are used to hold information. Theur purpose is to label and store data in memory, which will be used throughout the program you write.

### Setting / Reading variables

Bash variables don't have to be declared. You can have a varialbe by simply assigning a value to its reference.

```bash
#!/bin/bash
str="hello world!"
echo $str
```

The second line above creates a variable called `str`, and assigns the string "hello world" to it. Then the *value* of the `str` variable is retrieved by adding `$` to the variable's name. NOTE: If you ommit the `$` and use `echo str`, the "str" string will be echoed.

> "A quote"

When we want to assign a single word to variable, we don't need any quotes. The following command works just fine:

```bash
name=maroun
```

However, if we want to store more complex values,we need to use quotes.

There are two types of quotes: `'` (single quote), and `"` (double quotes).

*Single quotes* treats every character **literally**, whereas *double quotes* allows **substitution**. This snippet dempnstrates the difference:

```bash
var=world
echo "hello $var"
echo 'hello $var'
```

The output would be:

```
hello world
hello $var
```


#### Command substitution

Allows us to take an output of a command, and assign it to a variable. This happens when a command is enclosed as `$(command)` or `` `command` ``.

Bash executes the command in a subshell environment, and replaces the command substitution with the standard output of the command.

the `$(command)` form is preferred over the backticks one, as it's the newer POSIX form. If this reason is not enough, you should compare the following two, and decide which one is more readable:

```bash
echo $(echo $(echo hello))
echo `echo \`echo hello\``
```

### Special variables

Bash has some built in variables. The following list sums up these variables:

| Variable | Description   |
| :---     | :---          |
| `$0`     | The name of the file that's running the current script |
| `$n`     | Arguments with which the script was invoked (`$1` is the first argument, `$2` is the second and so on)  |
| `$$`     | The process ID of the current shell  |
| `$!`     | The process ID of the last background command  |
| `$@`     | All arguments passed to the script  |
| `$#`     | Number of arguments passed to the script  |
| `$?`     | The exit status of the most recently process  |
| `$_`     | The last argument of the last command  |

### Command line arguments

You are already familiar with command line arguments, and you've probably used it many times before. For example, when we use the command:

```bash
ls -l /etc
```

we're actually providing two arguments for the `ls` program. The first one (`$1`) is the `-l` flag, and the second one (`$2`) is the file destination, `/etc`.

As we saw in the table above, `$n` holds the arguments with which the script was invoked. For example, if we write the script `example.sh`:

```bash
First argument is $1, second one is $2
```

Now when we run `./example.sh Hello World!`, we'll get the output

> First argument is Hello, second one is World!  