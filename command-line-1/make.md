# make

## What is Make?

Make is a build tool, which reads rules from a makefile.

A rule looks like this:

```bash
target: prerequisites
    recipe
```

If the prerequisite files are newer than the target, make will run the recipe to rebuild the target.

* There is a TAB before the recipe.
* The prerequisite files can be both data and code.
* If the target is a directory, the timestamp is **NOT** automatically updated when files are copied into it. Thus the last step is usually `touch $@`.
* Make prints actions as it executes them. Using `@` at the start of an action tells Make not to print this action. Therefore usually there is a `@` before `echo`.

### Automatic Variable

* `$@`: the target of the current rule.
* `$^`: the dependencies of the current rule.
* `$<`: the first dependency of the current rule.

## Common usages

* `make`: builds the first target in makefile.
* `make target1`: searches the rule for `target1` and builds it.
* `make clean`: usually `clean` is a phony target, which removes all temporary files.

### **Options**

* `-f`: read from other files than makefile, e.g. `make -f common.mk`.
* `-n`: dry run - show the commands it will execute without actually running them.
* `-C`: change the directory and then run make.

### [Phony Targets](https://www.gnu.org/software/make/manual/html_node/Phony-Targets.html)

With the following rule, `make clean` will run the recipe regardless of whether there is a file named clean.

```bash
.PHONY: clean
clean:
    rm *.o temp
```

## A real example

With the following `makefile`, run `make` to remake all three programs, or specify as arguments the ones to remake \(e.g. `make prog1 prog3`\).

```bash
all : prog1 prog2 prog3
.PHONY : all

prog1 : prog1.o utils.o
    cc -o prog1 prog1.o utils.o

prog2 : prog2.o
    cc -o prog2 prog2.o

prog3 : prog3.o sort.o utils.o
    cc -o prog3 prog3.o sort.o utils.o
```

### Use phony target as subroutine

When one phony target is a prerequisite of another, it serves as a subroutine of the other. With the following `makefile`, `make cleanall` will delete the object files, the difference files, and the file `program`.

```bash
.PHONY: cleanall cleanobj cleandiff

cleanall : cleanobj cleandiff
    rm program

cleanobj :
    rm *.o

cleandiff :
    rm *.diff
```

### Pattern Rules

```bash
%.dat : books/%.txt countwords.py
    python countwords.py $< $*.dat # can also use $@ here
```

This rule can be interpreted as: “In order to build a file named \[something\].dat \(the target\) find a file named books/\[that same something\].txt \(the dependency\) and run countwords.py \[the dependency\] \[the target\].”

The Make `%` wildcard can only be used in a target and in its dependencies. It cannot be used in actions. In actions, you may however use `$*`, which will be replaced by the stem with which the rule matched.

## Advanced Topics

### Variables/Macros

Variables make the makefile more readable. For example:

```bash
CC = gcc # variable assignment
CCFLAGS = -Wall -Wextra -Werror -std=c99 -pedantic -g

main: main.o moreCode.o
    $(CC) $(CCFLAGS) -o main main.o moreCode.o # variable reference
```

We could also separate configuration from computation: use `include config.mk` at the top of the main makefile and put all configuration \(LANGUAGE, SRC\_FILE\_NAME, etc\) in `config.mk`.

Variables can be overridden on command line. e.g.

```bash
#Makefile
TEXT = default
target1:
	@ echo $(TEXT)

$ make
default

$ make TEXT=abc
abc
```

### Functions

Use `wildcard` function to get lists of files matching a pattern. Use `patsubst` function to rewrite file names.

```bash
TXT_FILES=$(wildcard books/*.txt)
DAT_FILES=$(patsubst books/%.txt, %.dat, $(TXT_FILES))

# output
TXT_FILES: books/abyss.txt books/isles.txt books/last.txt books/sierra.txt
DAT_FILES: abyss.dat isles.dat last.dat sierra.dat
```

### Self-Documenting Makefile

Add a comment that begins with `##` before each block and add a help block in the `makefile`. For example:

```text
# makefile
## clean       : Remove auto-generated files.
.PHONY : clean
clean :
    rm -f $(DAT_FILES)
    rm -f results.txt

.PHONY : help
help : makefile
    @sed -n 's/^##//p' $<```
```

And in terminal we could run `make help`:

```text
$ make help
 clean       : Remove auto-generated files.
```

## References

* [Automation and Make \(Software Carpentry\)](http://swcarpentry.github.io/make-novice/)
* [GNU Make manual](https://www.gnu.org/software/make/manual/make.html)
* [跟我一起写Makefile](https://github.com/seisman/how-to-write-makefile)

