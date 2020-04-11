# Basics

## Echo

```bash
echo Hello
Hello
echo Hello World
Hello World
echo Hello    World # multiple whilespaces
Hello World
echo 'Hello   World' # quotes
Hello   World
echo ~ # shell will expand ~ and pass to echo
```

## Working with Files and Directories

### mkdir

Can create multiple directories at the same time: `mkdir dir1 dir2`.

### touch

If file already exists, updates its timestamp, else creates the file.

### mv

Can move both files and directories. Can act as renaming. Move multiple files: `mv file1 file2 new-path`.

*  `-i` for confirmation.

### cp

* `-r` for directory.

### rm

* `-r` for directory.
* `-i` for confirmation.
* wildcards: `*?`. The shell expands the wildcards, not `rm`. `[AB]`: `A` or `B`.

## Navigation

### pwd

Print current working directory

### ls

List

* -l: long form.
* -a: show all. Include entries start with a dot
* -h: human readable size
* -R: recursively list sub directory
* -t: order by time of last change
* -r: reverse order

### cd

Change directory.

* No argument: go to home directory.
* **`-`: go to previous directory.**
* Absolute path begins with a `/`.
* ..: go up one level

## Text Manipulation

### cat

```bash
cat a.txt
cat a.txt b.txt
cat < a.txt
```

### head / tail

```bash
head a.txt # show first 10 lines of a.txt
head -n 5 a.txt # show first 5 lines
head -n 5 a.txt | tail -n 2 # show line 4-5
tail -f a.txt # watch a.txt
```

### less / more

* `/`: search.
* n/N: go to next/previous match.
* q: quit.

### wc

word count

* -l: line count only
* -w: words.
* -c: characters

```bash
man wc | wc
      80     483    3587
# 80 lines, 483 words, 3587 bytes
```

### Sort

Sort in alphanumerical order. Does not change the file.

* `-n`: sort numerically.
* `-r`: reverse order.

### Uniq

Removes adjacent duplicated lines. Usually used with sort: `sort a.txt | uniq`.

* `-c`: show the count of occurrences of each line.

### Cut

`cut -d , -f 4 a.txt` use comma as the delimiter, output 4th field.

## Multiple Commands

```bash
com1; com2 # will run com2 regardless of result of com1
com1 && com2 # only run com2 when com1 succeeds
com1 || echo "Failed" # only run echo when com1 fails
```

## Standard Stream

stdin \(0\), stdout \(1\), stderr \(2\). 0, 1, 2 are their file descriptor \(fd\). & is stdout + stderr.

### Redirect

```bash
cat < file1 # redirect stdin
echo "hello" > file2 # override
echo "world" >> file3 # append
command2 2>&1 # combine stderr into stdout
cp -v * ../otherfolder 1> ../success.txt 2>../error.txt
ls > /dev/null # discard the output.
```

## Environment Variables

```bash
env # show all environement variables
export name=David # set
export full_name="David Feng" # use quotes if value contains space
```

## Processes

### See all processes

`ps`

### Kill a process

| Signal | Effect | How to send |
| :--- | :--- | :--- |
| SIGINT | Process kills itself | Ctrl + C |
| SIGKILL \(can't be ignored\) | Kernel kills process | kill -9 |

SIGKILL - process may be killed before it releases its resources, causing some unexpected results.

```bash
kill -9 pid # when multiple processes share the same name
killall -9 process_name
```

### Suspend a process

| Signal | How to send |
| :--- | :--- |
| SIGTSTP | Ctrl + Z or `kill -TSTP pid` |
| SIGSTOP \(can't be ignored\) | `kill -STOP pid` |

### Jobs

Run a command in background: `command &`

foreground =&gt; background: Ctrl + Z

background =&gt; foreground: `fg`

list all jobs: `jobs`

\`\`

\`\`

### 

### Spaces in Names

Use `"$1"`, `"$filename"` to when the variable contains spaces.

### History

* `history | tail -n 5`: show the last 5 commands. `!123`: rerun command `123`.
* `!!`: rerun last command.
* `C-r`: search in history.
* `!$`: last word of last command.

### grep

`grep searchterm filename`

* `-i` case insensitive
* `-w`: only match whole-word match. `grep -w The a.txt`, `Thesis` won't match.
* `-n` show the line number
* `-v`: invert. Show the lines that does NOT contain the search term.
* `-E`: use regex as search term and tell shell not to expand the regex. `grep -E '^.o' a.txt`.

`grep -i break-in auth.log | awk {'print $12'}` Use awk to print the 12th thing on each line

### find

```bash
find . # shows all files and directories
find . -type d # only show directories. type f is for files
find . -name '*.txt' # use quotes around txt so that shell does not expand it
wc -l $(find . -name '*.txt') # subshell commands run first. Just like expanding the wildcards.
```

### man

man page is opened in `more`.

* `/`: Search.
* `n`: Go to next match.

### ssh

* logout: exit or ^-D \(in shell it means end of input\)
* run remote command without going into the remote shell: `ssh user@hostname "command"`
* ssh keys are stored in `~/.ssh`

### scp

* `scp localfile user@hostname:path/`
* `-r`: for directories

### permissions

* Unix groups are stored in `/etc/group`
* `chmod u=rw g=r a= a.txt` \(user, group, all\)
* `chmod 000 *_015*`
* search by permission: `$ find . -type f -perm -u=x`
* -x for directory: right to traverse the directory \(can see inner directories\), but not to look at the content.

### Job control

`ps`: list \(your active\) processes. `-f` gives more info.

* PID: process ID.
* PPID: parent's ID.
* UID: user ID.
* STIME: start time.
* TIME: how much time the process has used.
* CMD: the program that process is executing.
* TTY: ID of the terminal that the process.
* C: percentage of CPU utilization.
* `$ command &`: run `command` in background.
* `jobs`: list backgrounded processes.
* `fg`: bring one process to foreground. Can use `fg %1` to select to bring which process. The ID is the one shown in `jobs`.
* `^-Z`: pause a program and return to shell. Then `bg` resumes it as a background job. `fg`: resume as a foreground job.
* `kill %1`

### Aliases

`alias up='cd ..'`. We can remove a shortcut with `unalias`. e.g. `unalias upupup`.

The `.bashrc` file is executed whenever entering interactive non-login shells whereas `.bash_profile` is executed for login shells. If the `.bash_logout` file exists, then it will be run after exiting a shell session.

`rc` is short for `run control`.

Need to `source ~/.bashrc` for it to work in the same session.

```bash
# .bash_profile
if [ -f $HOME/.bashrc ]; then
    source $HOME/.bashrc
fi
```

Prompt is `PS1`.

### Shell Variables

```bash
$ set # shows shell variables
# if you don’t give "set" any arguments, it might as well show you things you could set.

echo $PATH # show the value of a variable
a=abc # create a variable
a=def # change a variable

# .bashrc
export a=123 # so a is available outside of .bashrc as well
```

All shell variables’ values are strings, even those \(like UID\) that look like numbers. It’s up to programs to convert these strings to other types when necessary.

Some variables \(like `PATH`\) store lists of values. In this case, the convention is to use a colon `:` as a separator. If a program wants the individual elements of such a list, it’s the program’s responsibility to split the variable’s string value into pieces.

### Misc

* `unzip abc.zip`.
* Move to start/end of line: `C-a`/`C-e`.

