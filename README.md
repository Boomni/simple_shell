# ALX Simple_shell Team Project
A group project to make our own simple shell in `C` that works like the bash. Our shell is named `hsh`.

## Resource

- [Unix shell](https://en.wikipedia.org/wiki/Unix_shell)
- [Thompson shell](https://en.wikipedia.org/wiki/Thompson_shell)
- [Ken Thompson](https://en.wikipedia.org/wiki/Ken_Thompson)
- [Everything you need to know to start coding your own shell](https://www.notion.so/C-Programming-f13cdb9661db464f8ea326c5a2654e8e)

---
## General Requirement for the project
- Allowed editors: `vi`, `vim`, `emacs`
- All your files will be compiled on Ubuntu 20.04 LTS using `gcc`, using the options `-Wall -Werror -Wextra -pedantic -std=gnu89`
- All your files should end with a new line
- A `README.md` file, at the root of the folder of the project is mandatory
- Your code should use the `Betty` style. It will be checked using [betty-style.pl](https://github.com/holbertonschool/Betty/blob/master/betty-style.pl) and [betty-doc.pl](https://github.com/holbertonschool/Betty/blob/master/betty-doc.pl)
Your shell should not have any memory leaks
No more than 5 functions per file
All your header files should be include guarded
Use system calls only when you need to ([why?](https://alx-intranet.hbtn.io/rltoken/EU7B1PTSy14INnZEShpobQ))
Write a `README` with the description of your project
You should have an `AUTHORS` file at the root of your repository, listing all individuals having contributed content to the repository. Format, see [Docker](https://alx-intranet.hbtn.io/rltoken/UL8J3kgl7HBK_Z9iBL3JFg)

<details>
<summary>List of allowed functions and system calls</summary>

+ `access` (man 2 access)
+ `chdir` (man 2 chdir)
+ `close` (man 2 close)
+ `closedir` (man 3 closedir)
+ `execve` (man 2 execve)
+ `exit` (man 3 exit)
+ `\_exit` (man 2 \_exit)
+ `fflush` (man 3 fflush)
+ `fork` (man 2 fork)
+ `free`(man 3 free)
+ `getcwd` (man 3 getcwd)
+ `getline` (man 3 getline)
+ `getpid` (man 2 getpid)
+ `isatty` (man 3 isatty)
+ `kill` (man 2 kill)
+ `malloc` (man 3 malloc)
+ `open` (man 2 open)
+ `opendir` (man 3 opendir)
+ `perror` (man 3 perror)
+ `read` (man 2 read)
+ `readdir` (man 3 readdir)
+ `signal` (man 2 signal)
+ `stat` (\_\_xstat) (man 2 stat)
+ `lstat` (\_\_lxstat) (man 2 lstat)
+ `fstat` (\_\_fxstat) (man 2 fstat)
+ `strtok` (man 3 strtok)
+ `wait` (man 2 wait)
+ `waitpid` (man 2 waitpid)
+ `wait3` (man 2 wait3)
+ `wait4` (man 2 wait4)
+ `write` (man 2 write)

</details>

<details>
<summary>The shell will be compiled this way:</summary>
 
 ```
$ gcc -Wall -Werror -Wextra -pedantic -std=gnu89 \*.c -o hsh
 ```
 
</details>

## Output
- Unless specified otherwise, your program must have the exact same output as `sh` (`/bin/sh`) as well as the exact same error output.
- The only difference is when you print an error, the name of the program must be equivalent to your `argv[0]` (see below)

<details>
<summary>Example of error with sh:</summary>
 
```
$ echo "qwerty" | /bin/sh
/bin/sh: 1: qwerty: not found
$ echo "qwerty" | /bin/../bin/sh
/bin/../bin/sh: 1: qwerty: not found
$ 
```
 
</details>

<details>
<summary>Same error with your program hsh:</summary>
 
```
$ echo "qwerty" | ./hsh
./hsh: 1: qwerty: not found
$ echo "qwerty" | ./././hsh
./././hsh: 1: qwerty: not found
$ 
```
 
</details>

## Testing

<details>
<summary>The shell should work like this in interactive mode:</summary>
 
```
$ ./hsh
($) /bin/ls
hsh main.c shell.c
($)
($) exit
$ 
```
 
</details>

<details>
<summary>But also in non-interactive mode:</summary>
 
```
$ echo "/bin/ls" | ./hsh
hsh main.c shell.c test\_ls\_2
$
$ cat test\_ls\_2
/bin/ls
/bin/ls
$
$ cat test\_ls\_2 | ./hsh
hsh main.c shell.c test\_ls\_2
hsh main.c shell.c test\_ls\_2
$
```
 
</details>

## Project Description

**hsh** is a simple UNIX command language interpreter that reads commands from either a file or standard input and executes them.

### How **hsh** works
* Prints a prompt and waits for a command from the user
* Creates a child process in which the command is checked
* Checks for built-ins, aliases in the PATH, and local executable programs
* The child process is replaced by the command, which accepts arguments
* When the command is done, the program returns to the parent process and prints the prompt
* The program is ready to receive a new command
* To exit: press Ctrl-D or enter "exit" (with or without a status)
* Works also in non interactive mode

### Compilation

`gcc -Wall -Werror -Wextra -pedantic -std=gnu89 *.c -o hsh`

### Invocation

Usage: **hsh** [filename]

To invoke **hsh**, compile all `.c` files in the repository and run the resulting executable.

**hsh** can be invoked both interactively and non-interactively. If **hsh** is invoked with standard input not connected to a terminal, it reads and executes received commands in order.

Example:
```
$ echo "echo 'hello'" | ./hsh
'hello'
$ 
```

If **hsh** is invoked with standard input connected to a terminal (determined by [isatty](https://linux.die.net/man/3/isatty)(3)), an *interactive* shell is opened. When executing interactively, **hsh** displays the prompt `$ ` when it is ready to read a command.
echo "echo 'hello'" | ./hsh
Example:
```
$ ./hsh
$ 
```

Alternatively, if command line arguments are supplied upon invocation, **hsh** treats the first argument as a file from which to read commands. The supplied file should contain one command per line. **hsh** runs each of the commands contained in the file in order before exiting.

Example:
```
$ cat test
echo 'hello'
$ ./hsh test
'hello'
$
```

### Environment

Upon invocation, **hsh** receives and copies the environment of the parent process in which it was executed. This environment is an array of *name-value* strings describing variables in the format *NAME=VALUE*. A few key environmental variables are:

<details><summary>HOME</summary>
 
 The home directory of the current user and the default directory argument for the <b>cd</b> builtin command.

```
$ echo $HOME
/home/Boomni
```
</details>
<details><summary>PWD</summary>
 
The current working directory as set by the <b>cd</b> command.

```
$ echo $PWD
/home/Boomni/my_repositories/simple_shell
```
</details>
<details><summary>OLDPWD</summary>
 
The previous working directory as set by the <b>cd</b> command.

```
$ echo $OLDPWD
/home/Boomni
```
</details>
<details><summary>PATH</summary>
 
A colon-separated list of directories in which the shell looks for commands. A null directory name in the path (represented by any of two adjacent colons, an initial colon, or a trailing colon) indicates the current directory.

```
$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
```
 
</details>

### Command Execution

After receiving a command, **hsh** tokenizes it into words using `" "` as a delimiter. The first word is considered the command and all remaining words are considered arguments to that command. **hsh** then proceeds with the following actions:
1. If the first character of the command is neither a slash (`\`) nor dot (`.`), the shell searches for it in the list of shell builtins. If there exists a builtin by that name, the builtin is invoked.
2. If the first character of the command is none of a slash (`\`), dot (`.`), nor builtin, **hsh** searches each element of the **PATH** environmental variable for a directory containing an executable file by that name.
3. If the first character of the command is a slash (`\`) or dot (`.`) or either of the above searches was successful, the shell executes the named program with any remaining given arguments in a separate execution environment.

### Exit Status

**hsh** returns the exit status of the last command executed, with zero indicating success and non-zero indicating failure.

If a command is not found, the return status is `127`; if a command is found but is not executable, the return status is 126.

All builtins return zero on success and one or two on incorrect usage (indicated by a corresponding error message).

### Signals 

While running in interactive mode, **hsh** ignores the keyboard input `Ctrl+c`. Alternatively, an input of end-of-file (`Ctrl+d`) will exit the program.

User hits `Ctrl+d` in the third line.
```
$ ./hsh
$ ^C
$ ^C
$
```

### Variable Replacement

**hsh** interprets the `$` character for variable replacement.

<details><summary>$ENV_VARIABLE</summary>
 
`ENV_VARIABLE` is substituted with its value.

Example:
```
$ echo $USER
Boomni
```
 
</details>
<details><summary>$?</summary>
 
`?` is substitued with the return value of the last program executed.

Example:
```
$ echo $?
0
```
 
</details>
<details><summary>$$</summary>
 
The second `$` is substitued with the current process ID.

Example:
```
$ echo $$
5540
```
 
</details>

### Comments 

**hsh** ignores all words and characters preceeded by a `#` character on a line.

Example:
```
$ echo 'hello' #this will be ignored!
'hello'
```

### Operators

**hsh** specially interprets the following operator characters:

<details><summary>; - Command separator</summary>
 
Commands separated by a `;` are executed sequentially.

Example:
```
$ echo 'hello'; pwd; cat test       
'hello'
$ /home/Boomni/my_repositories/simple_shell
$ echo 'hello'
```
 
</details>
<details><summary>&& - AND logical operator</summary>
 
`command1 && command2`: `command2` is executed if, and only if, `command1` returns an exit status of zero.

Example:
```
$ error! && echo 'hello'
./hsh: 1: error!: not found
$ echo 'all good' && echo 'hello'
'all good'
$ 'hello'
```
 
</details>

<details><summary>|| - OR logical operator</summary>

 `command1 || command2`: `command2` is executed if, and only if, `command1` returns a non-zero exit status.

Example:
```
$ error! || echo 'but still runs'
./hsh: 2: error!: not found
'but still runs'
```

The operators `&&` and `||` have equal precedence, followed by `;`.

</details>

### Builtin Commands
<details><summary>cd</summary>

 - Usage: `cd [DIRECTORY]`
 - Changes the current directory of the process to `DIRECTORY`.
 - If no argument is given, the command is interpreted as `cd $HOME`
 - If the argument `-` is given, the command is interpreted as `cd $OLDPWD` and the pathname of the new working directory is printed to standad output.
 - If the argument, `--` is given, the command is interpreted as `cd $OLDPWD` but the pathname of the new working directory is not printed.
 - The environment variables `PWD` and `OLDPWD` are updated after a change of directory.

Example:
```
$ 
$ pwd
/home/Boomni/my_repositories/simple_shell
$ cd ..
$ pwd
/home/Boomni/my_repositories
$ cd -
/home/Boomni/my_repositories/simple_shell
$ pwd
/home/Boomni/my_repositories/simple_shell
```

</details>
<details><summary>alias</summary>

 * Usage: `alias [NAME[='VALUE'] ...]`
  * Handles aliases.
  * `alias`: Prints a list of all aliases, one per line, in the form `NAME='VALUE'`.
  * `alias NAME [NAME2 ...]`: Prints the aliases `NAME`, `NAME2`, etc. one per line, in the form `NAME='VALUE'`.
  * `alias NAME='VALUE' [...]`: Defines an alias for each `NAME` whose `VALUE` is given. If `name` is already an alias, its value is replaced with `VALUE`.

Example:
```
$ alias show=ls
$ show
_atoi.c     getLine.c  shell.h
builtin1.c  history.c  shell_loop.c
builtin.c   hsh        string1.c
environ.c   lists1.c   string.c
errors1.c   lists.c    test
errors.c    main.c     tokenizer.c
exits.c     parser.c   vars.c
getenv.c    README.md
getinfo.c   realloc.c
```
</details>
<details><summary>exit</summary>

 * Usage: `exit [STATUS]`
  * Exits the shell.
  * The `STATUS` argument is the integer used to exit the shell.
  * If no argument is given, the command is interpreted as `exit 0`.

Example:
```
Boomni@acer-aod255e:~/my_repositories/simple_shell$ ./hsh
$ 
$ exit
Boomni@acer-aod255e:~/my_repositories/simple_shell$ 
```

</details>
<details><summary>env</summary>

 * Usage: `env`
  * Prints the current environment.

Example:
```
$ ./hsh
$ env
...
LC_NUMERIC=en_NG
OLDPWD=/home/Boomni
_=./hsh
```

</details>
<details><summary>setenv</summary>

 * Usage: `setenv [VARIABLE] [VALUE]`
  * Initializes a new environment variable, or modifies an existing one.
  * Upon failure, prints a message to `stderr`.

Example:
```
$ ./hsh
$ setenv NAME Poppy
$ echo $NAME
Poppy
```
 
</details>
<details><summary>unsetenv</summary>
 
 - Usage: `unsetenv [VARIABLE]`
 - Removes an environmental variable.
 - Upon failure, prints a message to `stderr`.

Example:
```
$ ./hsh
$ setenv NAME Girl
$ echo $NAME       
Girl
$ unsetenv NAME
$ echo $NAME
```
 
</details>

### What we learned:
* How a shell works and finds commands
* Creating, forking and working with processes
* Executing a program from another program
* Handling dynamic memory allocation in a large program
* Pair programming and team work
* Building a test suite to check our own code

## Authors
#### Jonathan Boomni
   - [Github](https://github.com/Boomni)
   - [Email](rejoiceoye1@gmail.com)

#### Benjamin Ifeanyichukwu Asogwa
- [GitHub](https://github.com/BENKINGS-CODE)
- [Email](ifeanyichukwubenjamin59@gmail.com)

