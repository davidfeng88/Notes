# Basics

## Multiple Commands

```bash
com1; com2 # will run com2 regardless of result of com1
com1 && com2 # only run com2 when com1 succeeds
com1 || echo "Failed" # only run echo when com1 fails
```

## Standard Stream

stdin \(0\), stdout \(1\), stderr \(2\). 0, 1, 2 are their file descriptor \(fd\).

### Redirect

```bash
cat < file1 # redirect stdin
echo "hello" > file2 # override
echo "world" >> file3 # append
command2 2>&1 # combine stderr into stdout
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

