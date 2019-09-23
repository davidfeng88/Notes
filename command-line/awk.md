# AWK

AWK is a programming language for text processing, usually used in data extraction and reporting. Input file is parsed into multiple records \(by default, one line is one record\), and each record is parsed into multiple fields \(by default, field separator is whitespaces\). Use AWK to parse HTML is generally a bad idea. However, it is common to use AWK to generate HTML.

## Basic Usages

```text
awk '{print $3}' dukeofyork.txt # prints the third field of each line
# $0 is the whole line. $1 is the first field, and so on.
awk '{print $2, $1}' names.txt # comma: insert a field separator (default is a space) between fields
awk '{print $2 ", " $1}' names.txt # insert a comma between fields by concatenation
awk '{print NF, $0}' dukeofyork.txt # NF: number of fields
awk '/up/{print NF, $0}' dukeofyork.txt # /up/ is a regex
awk 'NF==6{print}' dukeofyork.txt # print or print $0 prints the whole original line
awk -f swap.awk names.txt # use swap.awk as the awk program. In swap.awk we don't need to wrap the program in single quotes
awk -v hi=HELLO '{print $1, hi}' # -v: specify the value of an awk variable
```

## General Structure

`awk 'pattern{actions}' input_filename.txt`

perform the actions on the lines that match the pattern. Default action: print the whole line. Default pattern: all lines. Multiple actions are separated by `;`.

### Input

```text
# file
awk '{print $3}' dukeofyork.txt
awk '{print $3}' dukeofyork.txt names.txt # awk concatenates multiple files

# <
awk '{print $3}' < dukeofyork.txt

# pipe
uptime | awk '{print $1}'

# stdin
awk '{print $3}'
one two three
three # awk's output
four five six # another input
six # awk's output
# ctrl-C or ctrl-D to stop
```

#### Use AWK in shell script

```text
# store awk program in a shell script
# the line breaks are part of the awk program, not the shell script

# prog.sh
awk -Ft 'BEGIN {
    #actions
}

NR > 1 { # skip the first line
    actions
}

END {
    # actions
}' $1 > officers.html # $1 is for the shell script

# in bash, run ./prog.sh
```

### Output

```text
# stdout
awk '{print $3}' dukeofyork.txt
# >
awk '{print $3}' dukeofyork.txt > output.txt
# pipe
awk '{print $3}' dukeofyork.txt | sort -n
```

## Records and Fields

### Field Separator

Specify field separator:

**Method 1**: `-F`

```text
# -F, -F , is the same. -Ft => use tab
awk -F , '{print $2}'
one,two,three
two
one one,two two,three three
two two  # now whitespaces are considered part of the fields
one,,three
# No output. Now we can have empty fields

# Separator can be multiple character strings
awk -F ABC '{print $2}'
oneABCtwoABCthree
two

# Regex
# must be enclosed in quotes if it contains characters special to the shell
awk -F '[,!]' '{print $2}'
one!two,three
two
```

**Method 2**: assign `FS` variable. Note that awk first divides records into fields, then call the actions. To use the new FS in the first line, use the BEGIN pattern \(before any lines\). Similarly there is an END pattern \(after all lines in input files\).

```text
awk 'BEGIN{FS=","} {print $2}'
one,two,three
two
```

### Record Separator \(RS\)

```text
# Each line is a field, and records are separated by empty lines
awk 'BEGIN{RS="";FS="\n"} {name=$1;address=$2;print name "," address}' address.txt
```

### OFS and ORS

* OFS: output field separator. Default is single space.
* ORS: output record separator. Default is newline.

```text
awk 'BEGIN{OFS=", ";ORS="!"} {print $2, $1}' input.txt # comma in print means use OFS
```

## Variables and Operators

* NF: number of fields on the current line.
* NR: current line number. Note that if there are more than one input files, awk concatenates them, so the NR includes lines read from previous files.
* FILENAME: current filename.
* FNR: NR in the current file.
* $0: the whole line.
* $n: the nth field. E.g. $1 is the first field. n can be anything that has numeric values, does not have to be number literal. For example:
  * $NF: the last field
  * $\(NF-1\): the penultimate field.
  * $NF-1: if the last field is not a number, $NF is 0, so -1 will be printed
  * $\($1\): if $1 is a number \(e.g. 3\), this is equivalent of $3.

```text
$ awk '{print $NF-1}'
1 3 5 7
6 # $4 - 1 => 7 - 1
```

### Assignment

Assignment of $n changes $0 and NF. Assignment of $0 changes $n and NF. Change is in memory only. AWK never changes the original file.

```text
awk '{$2="TWO"; print}' dukeofyork.txt
The TWO old Duke of York
...

# Can assign to field that does not yet exist
# awk would add necessary separators and empty fields
awk 'BEGIN{OFS=","}{$12="TWO"; print}' dukeofyork.txt
The,grand,old,Duke,of,York,,,,,,TWO
```

### User Defined Variables

Names are case sensitive. Variables are treated as numbers or strings depending on context. Except user-defined functions, all variables are global.

```text
awk '{a=1;b=3;print a + b}'
4

awk '{a=1;b=3;print a b}'
13

awk '{a=1;b="Bob";print a + b}' # + treats operands as numbers, and the numeric value Bob is 0.
1

awk '{a=1;b=3;print a / b}' # integers and floating points are converted as necessary
0.33333
```

* Convert number to string: concat with `""`
* Convert string to number: +0

```text
awk '{print "\"" $1 "\" + 0 = " $1 + 0}'
123
"123" + 0 = 123
6.5
"6.5" + 0 = 6.5
4e3
"4e3" + 0 = 4000
-2.3
"-2.3" + 0 = -2.3
0023
"0023" + 0 = 23
66abc
"66abc" + 0 = 66
abc66
"abc66" + 0 = 0
```

### Operators

* `2 ^ 3` is 8
* `a++` and `++a` are available
* Strings can be compared \(e.g. `<=`\) as well.
* Concatenation: no operator

## Regex

```text
# Regex operators: ~ (match), !~
awk '$4 ~ /up/{print}' dukeofyork.txt

# ^ and $: matches the beginning and end of the string being compared, not alwasy the whole line.
awk '$4 ~ /^the/{print}' dukeofyork.txt # the beginning of $4 (not the whole line!) should be "the"

awk '/up/{print "UP:", NF, $0} /down/{print "DOWN:", NF, $0}' dukeofyork.txt
# the last line contains both "up" and "down", and it will be printed twice.
```

## Control Structure

### if/else

0 and empty string are falsey. Other values are truthy.

```text
# run awk on command line
awk '{ if (NF<8) {print "short line:", $0} else {print "long line:", $0}}' input.txt

# store awk program in a file
# program.awk
{
    if (NF < 8) {
        print "short line:", $0
    } else {
        print "long line:", $0
    }
}

awk -f program.awk input.txt
```

### Arrays and For Loop

Arrays: only one dimensional, e.g. `a[1]=$1`. Use C-style for loop to iterate.

```text
for ( i=1; i<=NF; i++ ) {
    words[tolower($i)]++;
}
```

### Associative arrays

Keys can be any string. Use for-in loop to iterate.

```text
for ( index in array) {
    # body
}
```

We can use associative arrays to simulate multi-dimensional array, e.g. `a["1:2"] = 5`.

## Printf\(\)

Unlike print, printf does not use OFS, ORS. Need to specify `\t`, `\n` explicitly.

* `%30s %30d`: minimal width is 30, right justified.
* `%-30s`: left justified.
* `%6.2f`: total 6 characters. 2 after decimal point, 3 before decimal point, and 1 for the decimal point itself
* `%06.2f`: left padding with 0.

## Strings

AWK counts characters in strings from 1. Common methods:

* `length([string])`: if no string is provided, $0 is used.
* `index(string, target)`: returns 0 if not found.
* `match(string, regex)`: similar to index, but also sets RSTART to the start position, RLENGTH to the length of the match \(note that regex is greedy\)
* `substr(string, start [, length])`: returns the substring from start to end of line if length is omitted.
* `sub(regex, newval [, string])`: $0 is used if no string is provided. Replaces the first match with newval.
* `gsub(regex, newval [, string])`: Replaces all matches with newval.
* `split(string, array [, regex])`: splits string into pieces, and stores them in array using regex as the separator. Returns the number of pieces it found. FS is used if regex is omitted.

## Misc

### Common Math Functions

* `int(x)`: int\(3.6\) == 3; int\(-3.6\) == -3
* `rand()`: a random number in \[0,1\). To simulate a 6-sided die: `int(rand()*6) +1`
* `srand([x])`: x is the optional seed. Default is current date and time. rand\(\) uses the same seed every time you invoke it. To get different number on your first calls, use srand\(\).
* `sqrt(x)`
* `sin(x)`
* `cos(x)`
* `log(x)`: natural log of x
* `exp(x)`: e to the x

### AWK and Excel

Use AWK to parse the csv generated by Excel:

```text
# Excel uses MS-DOS line ending (^M or \r)
BEGIN { RS="\r" }
{
    # if the field contains newline, Excel will replace new line with \r and adds quote around each field.
    # Do the following to parse it:
    # if the last field starts with a quote but does not end a quote (the field is not finished)
    while ( $NF ~ /^".*[^"]$/ ) {
        # use getline to store the next line in x
        getline x;
        # concat x with the current line with \n
        # assign a value to $0 causes awk to reparse the current line
        $0 = $0 "\n" x
    }
    # if your field is wrapped with double quotes in Excel
    # Excel will wrap the field with double quotes and
    # replaces the original double quotes with two double qoutes
    for ( i=1; i<=3; i++ ) {
        # remove the outer double quotes
        gsub("^\"|\"$", "", $i);
        # replace the two double quotes with one double quotes
        gsub("\"\"", "\"", $i);
    }
    print "!" $1 "!" $2 "!" $3 "!"
}
```

If the field contains comma, there is no easy way for AWK to handle it. Go back to Excel and export as tsv file, and use `awk -Ft`. It's hard to embed a tab in a field in Excel, so that AWK can properly parse it.

## AWK tutorials

* [Learn AWK in Y minutes](https://learnxinyminutes.com/docs/awk/)
* [AWK tutorial \(Chinese\)](http://www.ruanyifeng.com/blog/2018/11/awk.html)

