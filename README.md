# Index

1. [Introduction](#introduction)
    * [What is Bash?](#what-is-bash)
    * [Where Bash scripts run?](#where-bash-scripts-run)
    * [The Shebang (#!)](#the-shebang)
2. [Variables](#variables)
    * [Setting / Reading variables](#setting--reading-variables)
    * [Special variables](#special-variables)
    * [Command line arguments](#command-line-arguments)
    * [Exporting variables](#exporting-variables)
    * [Types of variables](#types-of-variables)
    * [Arrays variables](#arrays-variables)
3. Functions
    * Scope of variables
    * Return codes
4. Input
    * Reading input from STDIN
    * Asking user for input
5. Arithmetic
    * `declare`
    * `expr`
    * `let`
6.  Conditional statements
    * `if` statements
    * `test`
    * Boolean operations
    * `case` statements
7. Repetitive tasks
    * `for` loops
    * `while` loops
    * `until` loops
8. Signals


## Introduction

### What is bash?

"Bash" is **B**ourne **A**gain **SH**ell. It's a command language interpreter for the GNU operating system that executes commands from the standard input (STDIN) and is capable to run commands from a file (called a shell script).

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

### Exporting variables

Since scripts run in their own process, variables are also limited to the process in which they run.

In some cases, you would want to break your script to multiple scripts, and run one from another. Consider this example:

```bash
# a.sh
var=hello
./b.sh
```

```bash
# b.sh
echo $var
```
We'll not get anything printed to the console. That's because `var` variable is unknown in the subprocess that `b.sh` runs inside.

To overcome this problem, we use the `export` keyword:

```bash
# a.sh
var=hello
export var
./b.sh
```

Now, `var` is "exported" and is available in the second script.

**Important note!**

The `var` in the second script is just a copy of the variable, changing it inside `b.sh` has no impact on the original varialbe in `a.sh`! The snippet below should demonstrate the issue:

```bash
# a.sh
var=hello
export var
./b.sh
echo $var
```

```bash
# b.sh
var=world
```

If we run `./a.sh`, we'll get "hello" printed, and not "world".

### Types of variables

So far, we didn't care much about the variables' type. The variables we saw could contain any value we assign to them. For example:

```bash
var=hello
var=4
var=$(date)
var=2.3
```

In some cases, you might want to make a constant variable, or assign only integers to it.

Introducing the `declare` built-in keyword!

`declare` allows us to limit the value assigned to a variable. Some of its option:

| Option   | Description       |
| :---     | :---              |
| -a       | Array variable    |
| -i       | Integer variable  |
| -r       | Read only variable |
| -u       | All characters converted to uper-case |
| -l       | All characters converted to lower-case |

The code below will provide a good explanation of the usage:

```bash
#!/bin/bash
declare -i i=5
echo $i
i=str
echo $i
declare -l s=HELLO
echo $s
declare -p s
declare -ai arr=(9 2 3 4 5)
arr[0]=1
echo ${arr[*]}
# declare -r c=const
# r=hello 
```

Run `./test.sh`:

```
5
0
hello
declare -l s="hello"
1 2 3 4 5
```

If we uncomment the last two lines, we'll get an error saying that we're trying to assign to a readonly variable:

>./test.sh: line 10: r: readonly variable

### Arrays variables

Array is simply a variable holding multiple values, of the same type or different types.

Bash provides one dimentional indexed and associative array variables. There is no max limit on the size of an array.

To explicitly declare an array, we use:

```bash
declare -a array_var
```

It's also possible to create an array using compound assignments in the following format:

```bash
array_var=(value1 value2 value3 ... valueN)
```

#### Dereferencing the variables

We use curly braces in order to access an element. The following example should provide a good explanation:

```bash
arr=(1 2 3 4 5 6)
echo ${arr[*]}  # prints 1 2 3 4 5 6
echo $arr[*]    # prints 1[*] ($arr prints 1, then the chars [*] are printed)
echo ${arr[4]}  # prints 5 (arrays are zero-based)
echo ${#arr[@]} # prints 6
```
