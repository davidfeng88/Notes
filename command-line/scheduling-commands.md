# Scheduling Commands

Ref: [http://faculty.salina.k-state.edu/tim/unix\_sg/advanced/schedule.html](http://faculty.salina.k-state.edu/tim/unix_sg/advanced/schedule.html)

You may want to schedule some programs to run at later time \(**at**\) or want them to run on a regular, repeating schedule \(**crontab**\).

Unix has a facility for running scheduled tasks called **cron**, but users do run **cron** directly. It is always running in the background to run scheduled commands at the appropriate times. We call system programs, such as **cron**, that run in the backgroud _daemons_.

The programs that users use to schedule programs are **crontab** and **at**.

## at

```bash
$ at 2350 -f query.sh
# run query.sh at 23:50
# in the file, use absolute paths

$ at 2340
echo abc
# (ctrl-D)
job 3 at Sun Jan 12 23:00:00 2020
# input scheduled command in terminal

$ atq
# show all scheduled work, same as at -l
2	Sun Jan 12 23:40:00 2020
1	Sun Jan 12 23:50:00 2020

$ atrm 2
# remove job #2, same as at -r 2

$ atq
1	Sun Jan 12 23:50:00 2020
# job #2 has been removed
```

**at** understands noon, midnight, teatime \(4PM\), tomorrow \(10AM\), now + 1 hour, 4pm + 3 days, etc. \(more examples [here](https://www.computerhope.com/unix/uat.htm)\)

## crontab

```bash
$ crontab -e
# edit (add/remove) scheduled jobs (time + script)
# for time format, see https://crontab.guru/
# for script, again, use absolute path
# (in the script also)
# in vim
6 10 19 11 * /Users/davidfeng/query.sh
# :wq save and quit
crontab: installing new crontab

# alternatively
# task.crontab
# 6 10 19 11 * /Users/davidfeng/query.sh
$ crontab task.crontab

$ crontab -l
# list all scheduled jobs
6 10 19 11 * /Users/davidfeng/query.sh

$ crontab -r
# Remove your crontab
# effectively un-scheduling all crontab jobs
```

