# Command Line Resources

## Command Line Article List \(Under Construction\)

* [SSH Examples, Tips & Tunnels](https://hackertarget.com/ssh-examples-tunnels/)
* [tldr in Go](https://github.com/isacikgoz/tldr)
* [the Art of Command Line](https://github.com/jlevy/the-art-of-command-line/blob/master/README-zh.md)
* [Search files in Linux](http://blog.jobbole.com/114561/)
* [Coupled commands with control operators in Bash](https://opensource.com/article/18/11/control-operators-bash-shell)
* [Top Ten One-Liners from CommandLineFu Explained](http://www.catonmat.net/blog/top-ten-one-liners-from-commandlinefu-explained/)
* [Bash Emacs Editing Mode Cheat Sheet](http://www.catonmat.net/blog/bash-emacs-editing-mode-cheat-sheet/)
* [Working Productively in Bash's Vi Command Line Editing Mode \(with Cheat Sheet\)](http://www.catonmat.net/blog/bash-vi-editing-mode-cheat-sheet/)
* [Helpful Terminal Commands for Beginners!](https://www.malikbrowne.com/blog/helpful-terminal-commands)

{% embed url="https://linuxjourney.com/" %}

{% embed url="https://wangdoc.com/bash/index.html" %}





awesome command line [https://www.kawabangga.com/posts/4012](https://www.kawabangga.com/posts/4012)

{% embed url="https://blog.robertelder.org/data-science-linux-command-line/" %}



The linux command line中文版书 [https://billie66.gitbooks.io/tlcl-cn/content/chap00/introduction.html](https://billie66.gitbooks.io/tlcl-cn/content/chap00/introduction.html)

The Unix Chainsaw - Gary Bernhardt [https://www.youtube.com/watch?v=sCZJblyT\_XM](https://www.youtube.com/watch?v=sCZJblyT_XM)

IBM - Introduction to text manipulation on UNIX-based systems [https://www.ibm.com/developerworks/aix/library/au-unixtext/index.html](https://www.ibm.com/developerworks/aix/library/au-unixtext/index.html)

PDF [https://www.ibm.com/developerworks/aix/library/au-unixtext/au-unixtext-pdf.pdf](https://www.ibm.com/developerworks/aix/library/au-unixtext/au-unixtext-pdf.pdf)

Unix text process [https://en.m.wikibooks.org/wiki/Ad\_Hoc\_Data\_Analysis\_From\_The\_Unix\_Command\_Line](https://en.m.wikibooks.org/wiki/Ad_Hoc_Data_Analysis_From_The_Unix_Command_Line) [http://vegardstikbakke.com/unix/](http://vegardstikbakke.com/unix/) [https://www.oreilly.com/openbook/utp/](https://www.oreilly.com/openbook/utp/)

example [https://news.ycombinator.com/item?id=19160659](https://news.ycombinator.com/item?id=19160659)

`cat` is short for `concatenate`.

Shell 脚本学习指南

; is to separate commands in the same line & run the first command in background. Continue following commands \(don’t wait the completion\) Options with no arguments can be combined -l -t abc.c =&gt; -lt abc.c Echo \f 清屏

Command &lt; file \( &lt; change stdin\)

> will create the file if not exist. If exist, will override

Pipe: faster x10 than temp file Use the command that would decrease the data first

不关心命令输出，只关心命令退出状态：&gt; /dev/null

read password &lt; /dev/tty 从终端读入密码 stty-echo / echo 关闭/打开 自动打印输入字符 PATH: 用:分开的一系列目录 用于查找命令 永久修改：加入bashrc PATH=$PATH:$HOME/bin

命令行参数 $1 ${10}

Who 显示所有用户

chapter2 Cc File.c \| grep -v ^$ &gt; file.c 删除空行并储存 -i 忽略大小写 -r 查找所有文件 -n 显示行数 -C5 显示上下文 5行

### Regex

{n} {n,} {n,m} ^ 在方括号内表示取反 [https://regex101.com/](https://regex101.com/)

替换 sed

Cut

### cd

$ cd / \# to the root $ cd ~ \# to /User/davidfeng \(home directory\)

### ls

`ls -R` show all files in the subfolders. ls -lah \[name of file\] \# see the permissions

whoami \# show the current user

chmod +rw \# change mode: add/subtract permissions $ chmod u+x hello\_world.sh \# u is user \(myself\)

$ which grep \# where the command grep is installed? $ history \# shows history of commands

move to the front/end of a command: ctrl +a/e

A better Mac Terminal experience [https://medium.com/@caulfieldOwen/youre-missing-out-on-a-better-mac-terminal-experience-d73647abf6d7](https://medium.com/@caulfieldOwen/youre-missing-out-on-a-better-mac-terminal-experience-d73647abf6d7)

终端使用技巧 [https://www.kawabangga.com/posts/2432](https://www.kawabangga.com/posts/2432)

=&gt; Unix shell fundamentals 9h command line programs including ls vi grep etc. [https://www.safaribooksonline.com/library/view/unix-shell-fundamentals/1930519915/c0000.html](https://www.safaribooksonline.com/library/view/unix-shell-fundamentals/1930519915/c0000.html)

Shell十三问 [http://bbs.chinaunix.net/thread-218853-1-1.html](http://bbs.chinaunix.net/thread-218853-1-1.html) Linux C编程 [https://akaedu.github.io/book/index.html](https://akaedu.github.io/book/index.html)

learn enough command line [https://www.learnenough.com/command-line-tutorial\#sec-exercises\_editing\_the\_line](https://www.learnenough.com/command-line-tutorial#sec-exercises_editing_the_line) Ten things I wish I knew about bash [https://zwischenzugs.com/2018/01/06/ten-things-i-wish-id-known-about-bash/](https://zwischenzugs.com/2018/01/06/ten-things-i-wish-id-known-about-bash/) shellcheck [https://github.com/koalaman/shellcheck](https://github.com/koalaman/shellcheck) safe way to do things in bash [https://github.com/anordal/shellharden/blob/master/how\_to\_do\_things\_safely\_in\_bash.md](https://github.com/anordal/shellharden/blob/master/how_to_do_things_safely_in_bash.md) [https://news.ycombinator.com/item?id=17057596](https://news.ycombinator.com/item?id=17057596)

bat is a better version of cat ack is a better version of grep

CLI improved 中文 [https://www.kawabangga.com/posts/3084](https://www.kawabangga.com/posts/3084) [https://remysharp.com/2018/08/23/cli-improved](https://remysharp.com/2018/08/23/cli-improved) [https://github.com/jlevy/the-art-of-command-line\#one-liners](https://github.com/jlevy/the-art-of-command-line#one-liners) [https://henrikwarne.com/2018/08/11/my-favorite-command-line-shortcuts/](https://henrikwarne.com/2018/08/11/my-favorite-command-line-shortcuts/) [http://www.catonmat.net/blog/bash-one-liners-explained-part-one/](http://www.catonmat.net/blog/bash-one-liners-explained-part-one/) [https://github.com/Bash-it/bash-it](https://github.com/Bash-it/bash-it)

Learn Enough command line $ echo 1 &gt; afile create a file named “afile” with content 1. If afile already exist, it overrides the content. echo 2 &gt;&gt; afile \(append 2\)

find .git/objects \# find subfolders find .git/objects -type f \# only find files, exclude directories [https://www.learnenough.com/command-line-tutorial](https://www.learnenough.com/command-line-tutorial) yes command: can take optional argument, output confirm messages until killed. man: manual space bar: turn to next page in a long doc. \(man, cat, etc\) q: quit ^: ctrl ^A: move to beginning of a line ^E: move to end of line ^U: clear the line from cursor to beginning option + left click: move the cursor move forward/backward by one word: mac terminal: option + left/right; iTerm2: esc + b/f

random source ~/.bashrc source can be replaced with a . =&gt; . ~/.bashrc

wc filename word count

kill a process kill -options id kill -15 12345 -15: gental kill. Give the process a chance to clean up temp files other options: 2, 1, 9

make a new bash command “ekill”: Listing 21: A custom escalating kill script. ~/bin/ekill 1 \#!/bin/bash 2 3 \# Kill a process as safely as possible. 4 \# Tries to kill a process using a series of signals with escalating urgency. 5 \# usage: ekill  6

## If the number of argument is less than 1, exit with a usage statement.

if \[\[ $\# -lt 1 \]\]; then echo "usage: ekill " exit 1 fi 7 \# Assign the process id to the first argument. 8 pid=$1 9 kill -15 $pid \|\| kill -2 $pid \|\| kill -1 $pid \|\| kill -9 $pid

To make ekill work: ~/bin is on the system path. echo $PATH if /Users/davidfeng/bin is not listed, edit bashrc export PATH="~/bin:$PATH" \# sometimes $HOME works instead of ~ make the script itself executable chmod +x ~/bin/ekill confirm: which ekill =&gt; gives the full path to the script

ps aux: show all processes tail: get a hanging process

curl -0L url/filename \#download unzip xyz.zip

grep -n Waldo \* \#-n for line number use grep in vim: -n is added automatically. -i: ignore case.



