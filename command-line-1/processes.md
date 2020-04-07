# Processes

## See all processes

`ps`

## Kill a process

| Signal | Effect | How to send |
| :--- | :--- | :--- |
| SIGINT | Process kills itself | Ctrl + C |
| SIGKILL \(can't be ignored\) | Kernel kills process | kill -9 |

SIGKILL - process may be killed before it releases its resources, causing some unexpected results.

```bash
kill -9 pid # when multiple processes share the same name
killall -9 process_name
```

## Suspend a process

| Signal | How to send |
| :--- | :--- |
| SIGTSTP | Ctrl + Z or `kill -TSTP pid` |
| SIGSTOP \(can't be ignored\) | `kill -STOP pid` |

## Jobs

Run a command in background: `command &`

foreground =&gt; background: Ctrl + Z

background =&gt; foreground: `fg`

list all jobs: `jobs`

