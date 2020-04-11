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

```bash
man wc | wc
      80     483    3587
# 80 lines, 483 words, 3587 bytes
```

\`\`

* `sort`: sort in alphanumerical order. `-n`: sort numerically. Does not change the file. `-r`: reverse order.
* `uniq`: removes adjacent duplicated lines. Usually `sort a.txt | uniq`. `-c`: show the count of occurances of each line.
* `cut`: `cut -d , -f 4 a.txt` use comma as the delimeter, output 4th field.
* `wc`: word count. `-l` only output the line counts. `-w` words, `-c` characters.

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

