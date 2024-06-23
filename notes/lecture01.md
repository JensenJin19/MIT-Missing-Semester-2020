# Course overview + the shell

## What is the shell

We will focus on the Bourne Again SHell, or **“bash”** for short. This is one of the most widely used shells, and its syntax is similar to what you will see in many other shells. To open a **shell prompt (提示符) **(where you can type commands), you first need a **terminal (终端)**

## Using shell

```Bash
missing:~$ 
```

1. `missing`: On the machine `missing`.
2. `~`: Current working dicectory (short for 'home').
3. `$`: Not the root user.

```Bash
missing:~$ date
Fri 10 Jan 2020 11:49:31 AM EST
missing:~$
```

`date`: Print the current date and time.

```Bash
missing:~$ echo hello
hello
```

`echo`: Print out its arguments.

> NOTE
> The shell **parses the command** by splitting it by whitespace, and then runs the **program** indicated by the first word, supplying **each subsequent word as an argument** that the program can access.

Shell is a programming environment:

```Bash
missing:~$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
missing:~$ which echo
/bin/echo
missing:~$ /bin/echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

`echo $PATH`: When run the `echo` command, the shell sees that it should execute the program `echo`, and then searches through the `:`-separated list of directories in `$PATH` for a file by that name.

`which`: Find out which files is executed for a given program name.

`/bin/echo $PATH`: Bypass $PATH entirely by giving the path to the file we want to execute.

## Navigating in the shell

A **path** on the shell is a delimited list of directories; separated by `/` on Linux and macOS and `\` on Windows.

On Linux and macOS, the path `/` is the “root” of the file system, under which all directories and files lie, whereas on Windows there is one root for each disk partition (磁盘分区) (e.g., `C:\`).

```Bash
missing:~$ pwd
/home/missing
missing:~$ cd /home
missing:/home$ pwd
/home
missing:/home$ cd ..
missing:/$ pwd
/
missing:/$ cd ./home
missing:/home$ pwd
/home
missing:/home$ cd missing
missing:~$ pwd
/home/missing
missing:~$ ../../bin/echo hello
hello
```

`pwd`: Print working directory

`cd`: Change directory

`.` (single dot): Represents the **current directory**

`..` (double dot): Represents the **parent directory**

`ls` command: See what lives in a given directory:

```Bash
missing:~$ ls
missing:~$ cd ..
missing:/home$ ls
missing
missing:/home$ cd ..
missing:/$ ls
bin
boot
dev
etc
home
...
```

Unless a directory is given as its first argument, `ls` will print the contents of the current directory.

Most commands accept **flags (标记)** and **options (选项, flags with values)**

`-h` or `--help` flag: Print some help text that tells what flags and options are available. For example, `ls --help` shows:

```Bash
-l                         use a long listing format
```

and then:

```Bash
missing:~$ ls -l /home
drwxr-xr-x 1 missing  users  4096 Jun 15  2019 missing
```

`drwxr-xr-x`: `d` means `missing` is a directory

`rwx/r-x/r-x`: Three groups of three characters indicate what permissions the owner of the file (`missing`) (文件所有者), the owning group (`users`) (用户组), and everyone (所有人) else respectively have on the relevant item. A `-` (dash) indicates that the given principal (主体) doesn't have the given permission.

- `r`: Read permissions on directory.
- `w`: Modify permissions (i.e., add/reove files in directory).
- `x`: Execute/Search permissions on directory.

> NOTE
> To enter a directory, a user must have “search” (represented by “execute”: `x`) permissions on that directory (and its **parents**).

> NOTE
> All the files in `/bin` have the `x` permission set for the last group - everyone else..\

```Bash
lrwxrwxrwx   1 root root       7 Nov 23  2023 bin -> usr/bin
```

Other handy programs:
- `mv`: rename/move a file
- `cp`: copy a file
- `mkdir`: make a new directory
- `man`: Take as an argument the name of a program, and shows its `manual page`. Press `q` to exit

```Bash
missing:~$ man ls
```

## Connecting programs

In the shell, programs have two primary “streams” associated with them: their **input stream** and **their output stream**:

- When the program tries to read input, it reads from the input stream.
- When it prints something, it prints to its output stream. 

Normally, a program’s input and output are both your terminal. That is, **your keyboard as input and your screen as output**. However, we can also rewire (重定向) those streams!

### `> file` and `< file`

The simplest form of redirection is `< file` and `> file`. These let you rewire the input and output streams *of a program to a file* respectively:

```Bash
missing:~$ echo hello > hello.txt
missing:~$ cat hello.txt
hello
missing:~$ cat < hello.txt
hello
missing:~$ cat < hello.txt > hello2.txt
missing:~$ cat hello2.txt
hello
```

- `>`: The `>` symbol is used to redirect the standard output (stdout) *of a command to a file*. **If the file already exists, it will be overwritten. If the file does not exist, it will be created.**
- `<`: The `<` symbol is used to redirect the standard input (stdin) *of a command from a file*.
- Can also use `>>` to **append to a file**. Where this kind of input/output redirection really shines is in the use of pipes. The `|` operator **“chain”** programs such that the output of one is the input of another:

```Bash
missing:~$ ls -l / | tail -n1
drwxr-xr-x 1 root  root  4096 Jun 20  2019 var
missing:~$ curl --head --silent google.com | grep --ignore-case content-length | cut --delimiter=' ' -f2
219
```

- `tail`: Output the last part of files (By default, shows the last 10 lines of a file)
- `curl`: Transfer datas with URL
- `grep`: Print lines that match patterns

### Pipe `|`

**`|`: Pipe `|` redirects the output of one command to the input of another.**

The output of command1 becomes the input of command2 : 
```
command1 | command2
```

## A versatile and powerful tool

On most Unix-like systems, the `root` user is above (almost) all access restrictions. Since you won't be logging into the system as root very often, can use `sudo` command.

`sudo`: Lets you `do` something `as su` (short for *super root*, or root). When you get permission denied errors, it is usually because you need to do something as root. Though make sure you first **double-check** that you really wanted to do it that way!

```Bash
$ sudo find -L /sys/class/backlight -maxdepth 2 -name '*brightness*'
/sys/class/backlight/thinkpad_screen/brightness
$ cd /sys/class/backlight/thinkpad_screen
$ sudo echo 3 > brightness
An error occurred while redirecting file 'brightness'
open: Permission denied
```

> NOTE
> `sysfs` does not exist on Windows or macOS.

Operations like `|`, `>`, and `<` are done **by the shell**, not by the individual program. `echo` and friends do not “know” about `|`. They just read from their input and write to their output, whatever it may be. In the case above, the shell (which is authenticated just as your user) tries to open the brightness file for writing, before setting that as sudo echo’s output, but is prevented from doing so since the shell does not run as root. Using this knowledge, we can work around this:

```Bash
$ echo 3 | sudo tee brightness
```

Since the `tee` program is the one to open the /`sys` file for writing, and it is running as root, the permissions all work out.