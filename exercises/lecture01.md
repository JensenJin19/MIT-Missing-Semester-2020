# Lecture 1: Exercises

## ex01

For this course, you need to be using a Unix shell like `Bash` or `ZSH`. If you are on Linux or macOS, you don’t have to do anything special. If you are on Windows, you need to make sure you are not running cmd.exe or PowerShell; you can use Windows Subsystem for Linux or a Linux virtual machine to use Unix-style command-line tools. To make sure you’re running an appropriate shell, you can try the command `echo $SHELL`. If it says something like /bin/bash or /usr/bin/zsh, that means you’re running the right program.

### Answer

```Bash
wsl --install
```

## ex02

Create a new directory called missing under /tmp.

### Answer

```Bash
mkdir /tmp/missing
```

## ex03

Look up the touch program. The man program is your friend.

### Answer

```Bash
man touch
```

## ex04

Use touch to create a new file called semester in missing.

### Answer

```Bash
touch /tmp/missing/semester
```

## ex05

Write the following into that file, one line at a time:

```Bash
#!/bin/sh
curl --head --silent https://missing.csail.mit.edu
```

The first line might be tricky to get working. It’s helpful to know that `#` starts a comment in Bash, and `!` has a special meaning even within double-quoted (`"`) strings. Bash treats single-quoted strings (`'`) differently: they will do the trick (起作用) in this case. See the Bash quoting manual page for more information.

### Answer

```Bash
jensenjin:~$ echo '#!/bin/bash' > semester
jensenjin:~$ echo curl --head --silent https://missing.csail.mit.edu >> semester
jensenjin:~$ cat semester
#!/bin/bash
curl --head --silent https://missing.csail.mit.edu
jensenjin:~$
```

## ex06

Try to execute the file, i.e. type the path to the script (./semester) into your shell and press enter. Understand why it doesn’t work by consulting the output of ls (hint: look at the permission bits of the file).

### Answer

```Bash
jensenjin:~$ ./semester
-bash: ./semester: Permission denied
jensenjin:~$ ls -l
total 16
-rw-r--r-- 1 jensenjin jensenjin  63 Jun  1 13:24 semester
-rw-r--r-- 1 jensenjin jensenjin 308 May 31 18:30 wget-log
-rw-r--r-- 1 jensenjin jensenjin 466 May 31 18:58 wget-log.1
-rw-r--r-- 1 jensenjin jensenjin 302 May 31 18:58 wget-log.2
```

The permission bits of `semester` is `rw-r--r--`, so I don't have `x` permission execute this file

## ex07

Run the command by explicitly starting the `sh` interpreter, and giving it the file `semester` as the first argument, i.e. `sh semester`. Why does this work, while `./semester` didn’t?

### Answer

`sh semester` works because it directly invokes the `sh` interpreter with the script file as an argument.

## ex08

Look up the chmod program (e.g. use `man chmod`).

### Answer

```Bash
man chmod
```

## ex09

Use `chmod` to make it possible to run the command `./semester` rather than having to type `sh semester`. How does your shell know that the file is supposed to be interpreted using sh? See this page on the [shebang](https://en.wikipedia.org/wiki/Shebang_(Unix)) line for more information.

### Answer

```Bash
chmod a+x semester
```

Because the term "shebang" (sharp-exclamation, sha-bang, hashbang, pound-bang,or hash-pling.) refers to the character sequence `#!` at the beginning of a script file in Unix-like operating systems. This sequence is used to indicate which interpreter should be used to execute the script that follows.

`#!/bin/bash`: Execute the file using the `Bash shell`

## ex10

Use `|` and `>` to write the “last modified” date output by semester into a file called `last-modified.txt` in your home directory.

### Answer

```Bash
jensenjin:~$ cat last-modified.txt
last-modified: Sat, 18 May 2024 14:18:01 GMT
```

## ex11

Write a command that reads out your laptop battery’s power level or your desktop machine’s CPU temperature from `/sys`. Note: if you’re a macOS user, your OS doesn’t have `sysfs`, so you can skip this exercise.

### Answer

```Bash
jensenjin:~$ cat /sys/class/power_supply/BAT1/capacity
98
```
