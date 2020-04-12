# Bash script

## Basics

The first line of the script is

```bash
#!/bin/bash
```

If the script is in the $PATH \(e.g. `/usr/bin`\), we can just type `my.sh` instead of `./my.sh` to run it.

Best practice to increase portability:

```bash
# instead of
#!/usr/local/bin/python
# use
#!/usr/bin/env python
```

### Differences between shell functions and other scripts \(e.g. python scripts\)

Functions are executed in the current shell environment whereas scripts execute in their own process. Thus, functions can modify environment variables, e.g. change your current directory, whereas scripts can’t. Scripts will be passed by value environment variables that have been exported using `export`



### Variables

```bash
a=Hello # no spaces!
# a = Hello means run a with = and Hello as arguments

echo $a # Hello
echo "$a" # Hello
echo '$a' # $a
echo a # a

# add attributes to variables
declare -i d=123 # d is an integer
declare -r e=456 # e is read-only
declare -l f="LOLCats" # f is lolcats
declare -u g="LOLCats" # g is LOLCATS

# Built-in varibles
# $HOME $PWD $MACHTYPE $HOSTNAME $RANDOM
# $BASH_VERSION related: bash --version
# $0 name of the script
# $SECONDS time since this bash session had run / since the script started
# env: shows all environment variables

d=$(pwd) # run pwd and put the result in d.

# Arithmetic (only integers are supported)
val=$((expression))
# operators: **, %, +-*/%

e=5
e+=2 # string concatenation
((e+=2)) # arithmetic
```

### Command substitution

```bash
d=$(pwd) # run pwd and put the result in d.
```

### Process substitution

```bash
command -a <(CMD) # execute CMD, put output in a temp file,
# replace <() with the temp file name.
# useful when command expects input as a file instead of STDIN

# Example:
mkdir foo bar

# This creates files foo/a, foo/b, ... foo/h, bar/a, bar/b, ... bar/h
touch {foo,bar}/{a..h}
touch foo/x bar/y
# Show differences between files in foo and bar
diff <(ls foo) <(ls bar)
# Outputs
# < x
# ---
# > y
```

### Comparisons

```bash
[[ expression ]] # Note the spaces
# false/failure: returns 1
# true/success: returns 0 

[[ "cat" == "cat" ]]
echo $? # $? stores the previous value

# comparison for integers

# 20 > 100 is true because its string comparison
[[ 20 -lt 100 ]]
# -lt less than, -gt, -le less than or equal to, -ge, -eq, -ne

# &&, ||, ! logic operators

#string is null value (empty string)
[[ -z $a ]] # is null?
# -n => is not null?
```

### Strings

```bash
# concatenation
a=Hello
b=World
c=$a$b
echo $c # HelloWorld

# get string's length
echo ${#a} # 5

# substring
# start index, length
echo ${c:3:4} # olWo
# last four characters. Note the space before dash.
f=${c: -4} # orld

# replace
fruit="apple banana banana cherry"
# replace the first occurance
echo ${fruit/banana/durian} # apple durian banana cherry
# replace all occurance
echo ${fruit//banana/durian} # apple durian durian cherry
# replace only if banana is the beginning of the string
echo ${fruit/#banana/durian}
# replace only if banana is the end of the string
echo ${fruit/%banana/durian}
# replace regex matches
echo ${fruit/b*/durian} # apple durian
```

### Reading and Writing Text Files

```bash
echo "Some text" > file.txt # If already exsits, override, else, create.
> file.txt # clear content of a file
echo "Some text" >> file.txt # append to end

# read
cat < file.txt

# read line by line
while read f; do
    echo $f
done < file.txt

# Example: set a list of instructions
# create a file ftp.txt with commands
ftp -n < ftp.txt
```

### Control Structures

#### if

```bash
if [ expression ] # or if [[ $a =~ regex ]] (( integer comparison )) or no brackets at all
if expression; then
    ...

if expression
then
    echo "true"
elif
    ...
else
    ...
fi
```

#### while/until

```bash
i=0
while [ $i -le 10 ]; do
    echo i;$i
    ((i+=1))
done

j=0
until [ $j -ge 10 ]; do
    echo j:$j
    ((j+=1))
done
```

#### for loop

```bash
for i in 1 2 3 # or 1 in {1..100}
do
    echo $i
done

# C style for loop
for (( i=1; i<=10; i++ ))
do
    ...
done

# array
arr=("apple" "banana")
for i in ${arr[@]}
do
    ...
done

# associative array (BASH 4.0+)
declare -A arr
arr["name"]="scott"
arr["id"]="123"
for i in "${!arr[@]}" # use quote to handle spaces in keys
do
    echo "$i: ${arr[$i]}"
done

# command substitution
for i in $(ls)
do
    echo "$i"
done
```

#### Case \(switch in C\)

```bash
a="dog"
case $a in
    cat) echo "Feline";;
    dog|puppy) echo "Canine";; # dog or puppy
    *) echo "No match!";; # default
esac # case spelled backwards
```

### Array

```bash
a=() # empty array
b=("apple" "banana" "cherry")
echo ${b[2]} # cherry
b[5]="kiwi"
b+=("mango") # append
echo ${b[@]} # the whole array
echo ${b[@]: -1} # last element

# Associate arrays (BASH4.0+)
declare -A myarray
myarray[color]=blue
# if key/value has spaces, use quotes
```

### Functions

```bash
function greet {
    echo "Hi"
}

# invoke
greet

# with arguments
function sayHi {
    echo "Hi, $1"
}

sayHi David

function numberThings {
    i=1
    for f in $@; do # $@ is the arguments array
        echo $i: $f
        (( i +=1 ))
    done
}

numberthings $(ls)
```

### Arguments

Anything with space in it needs quotes.

* `$0`: name of the script
* `$1`: first argument
* `$#`: the number of arguments
* `$@`: the arguments array
* `$?`: return code of previous command
* `$$`: PID of current script
* `$_`: last argument of previous command

```bash
#!/bin/bash

echo "Starting program at $(date)" # Date will be substituted

echo "Running program $0 with $# arguments with pid $$"

for file in $@; do
    grep foobar $file > /dev/null 2> /dev/null
    # When pattern is not found, grep has exit status 1
    # We redirect STDOUT and STDERR to a null register since we do not care about them
    if [[ $? -ne 0 ]]; then
        echo "File $file does not have any foobar, adding one"
        echo "# foobar" >> "$file"
    fi
done
```

### Interact with the User

#### Working with flags

```bash
# Case one: get u and p values
# my.sh
while getopts u:p: option; do
    case $option in
        u) user=$OPTARG;;
        p) pass=$OPTARG;;
    esac
done

# in shell, call with ./my.sh -u scott -p 123

# Case two: boolean flags
while getopts :u:p:ab option; do # ab has no colon after them. They are boolean flags.
# `:` before u => we care about unknown flags
    case $option in
        u) user=$OPTARG;;
        p) pass=$OPTARG;;
        a) echo "A";;
        b) echo "B";;
        ?) echo "I don't know $OPTARG";;
    esac
done
```

#### Get input during execution

```bash
echo "What is your name?"
read name # wait for user to input, and store in name
read -s pass # silent. Don't print the input password on screen
read -p "What's your age?" age # -p prompt

select option in "cat" "dog" "bird" "quit"
do
    case $option in
        cat) echo "you selected $animal";;
        quit) break;;
        *) echo "I don't understand"
    esac
done
```

#### Ensuring a response

What if user just press enter?

```bash
if [ $# -lt 3 ]; then
    cat <<- EOM
        some error message
EOM
else
    # the program here
fi

read -p "Name? " n
while [[ -z "$n" ]]; do
    read -p "Need a name! " n
done

# provide a default answer
read -p "Name? [David] " n
while [[ -z "$n" ]]; do
    n=David
done

# input validation by regex
read -p "What year? [nnnn] " a
while [[ ! $a =~ [0-9]{4} ]]; do
    read -p "A year, please! [nnnn] " a
done
```

### Date and Printf

```bash
date
date +"%d-%m-%Y"
date +"%H:%M:%S"
printf "Name:\t%s\nID:\t%04d\n" "Name" "12" # 4 digit with padding zeros
printf -v d "strings" # store the string in variable d
#command substitution strips out the newlines
```

## Advanced Topics

### Debug Mode

```bash
# run script in normal mode
$ bash a.sh

# run in debug mode
# prints out each command as it is run
# or you can set -x in a.sh
# set -e => exit script when there is an error
$ bash -x a.sh
```

### Brace Expansion

```bash
~ $ echo {apple,banana}
apple banana
~  $ echo {apple, banana}
{apple, banana} # does not work with spaces
~  $ touch {apple,banana} # creates two files
~  $ echo {1..10}
1 2 3 4 5 6 7 8 9 10
~  $ echo {A..z}
A B C D E F G H I J K L M N O P Q R S T U V W X Y Z [  ] ^ _ ` a b c d e f g h i j k l m n o p q r s t u v w x y z # uppercase letters are before lowercases
~  $ echo {a..Z}
a ` _ ^ ]  [ Z # reverse order
~  $ echo {apple,banana}_{1..3}
apple_1 apple_2 apple_3 banana_1 banana_2 banana_3 # combinations

# in bash 4+
~  $ echo {01..1000} # add padding zeros
~  $ echo {1..10..2} # specify intervals
```

### Print Colored Text

ANSI escape codes: e.g. -e can enable escape sequences \(start + string to print + end\).

Start: `'\033[number1;number2m'`. Number1 and number 2 are foreground and background colors. m indicates the end of sequence. A style number \(followed by `;`\) before number1 is optional.

End: usually it's `'\033[0m'`, to clear all the formatting.

| Color | Foreground | Background |
| :--- | :--- | :--- |
| Black | 30 | 40 |
| Red | 31 | 41 |
| Green | 32 | 42 |
| Yellow | 33 | 43 |
| Blue | 34 | 44 |
| Magenta | 35 | 45 |
| Cyan | 36 | 46 |
| White | 37 | 47 |

Style table

| Style | Value |
| :--- | :--- |
| No Style | 0 |
| Bold | 1 |
| Low Intensity | 2 |
| Underline | 4 |
| Blinking | 5 |
| Reverse | 7 |
| Invisible | 8 |

```bash
echo -e '\033[34;42mColor Text\033[0m'
# store formatting in variables
flashred="\033[5;31;40m"
end="\033[0m"
echo -e $flashred"Error"$end
```

Alternative: use `tput`.

### Here Documents

```bash
cat << endoftext
this is a
multiline
text
endoftext

# with <<-, bash will strips off leading tabs
cat <<- endoftext2 > log.txt
        this is a
        multiline
        text
endoftext2

ftp -n <<- label
    commands
label
```

## References

* [Learn Bash in Y minutes](https://learnxinyminutes.com/docs/bash/)

