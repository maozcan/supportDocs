# Linux BASH Scripting

When you need info:

`> man bash`

`> man bash | wc -l`

`> info bash`

## Script File basics

First two chars should be `#!` known as 'shebang', followed by path to Bash or env
`#!/usr/bin/env bash` or `#!/bin/bash`

When we run a *sh* code, it will look for the bash command first.. So, `./myscript.sh` with `#!/bin/bash` would have the kernel execute `/bin/bash ./myscript.sh`. The same structure also applies to other script languages; Python, Perl, etc..

To make a script executable, use `chmod +x myscript.sh` so that we run by only `./myscript.sh`. Otherwise we have to call the scrip from another executable like `bash myscript.sh`

### Time

Bash has builtin time command to report how much time a process consumed.
`time find / -name core`

### Vars

Variables in Bash are assigned a value with `=` with no spaces before and after the equal sign. If the value has special characters (including spaces) that we need to put the value in quotes:
`myvar="this is a test $ *"`

If we want Bash to remove a variable, we use `unset` command.

If we want to reference the value of a variable, then we have to use the `$` sign: `echo myvar is $myvar`

To get a copy of the shell variable:
`export mynewvar` or `declare -x mynewvar`

To export and assign in the same statement `export var2="var2 value"`

Use `export -f myfunc` to export and make visible the function myfunc.

Typing just `export` will print which variables are part of the shell's environment - those exported.

Bash functions use braces `{}` and can modify variables of the shell that calls the function. Paranthesis `()` can also be used to group things but it behaves differently.
```
a=1
(a=2)
echo $a
#prints 1
```
```
a=1
{a=2}
echo $a
#prints 2
```

### Bash Built-ins
To get a list of built-ins, type `enable` command

Bash would prefer built-ins, keywords, aliases and functions over the commands in the filesystem

See the built-ins:
```
> enable  
enable .
enable :
enable [
enable alias
enable bg
enable bind
enable break
enable builtin
enable caller
enable cd
enable command
enable compgen
enable complete
enable compopt
enable continue
enable declare
enable dirs
enable disown
enable echo
enable enable
enable eval
enable exec
enable exit
enable export
enable false
enable fc
enable fg
enable getopts
enable hash
enable help
enable history
enable jobs
enable kill
enable let
enable local
enable logout
enable mapfile
enable popd
enable printf
enable pushd
enable pwd
enable read
enable readarray
enable readonly
enable return
enable set
enable shift
enable shopt
enable source
enable suspend
enable test
enable times
enable trap
enable true
enable type
enable typeset
enable ulimit
enable umask
enable unalias
enable unset
enable wait

```
See the built-in keywords:

```
> compgen -k
if
then
else
elif
fi
case
esac
for
select
while
until
do
done
in
function
time
{
}
!
[[
]]
coproc

```


### Bash startup

`.bash_profile` is read when Bash is invoked as a login shell and `.bashrc` is executed when a new shell is started. To see the contents of these files, run `cat ~/.bash_profile` and `cat ~/.bashrc` in bash.

If you extend an exported variable (like PATH) in `.bashrc`, it will grow with each nested shell invocation.

### Sourcing Scripts
Sourcing is a common way to import variable assignments or functions.
Use *dot space* to source a script. `. example.sh` The shell executes the script in the shell's own process instead of in a new process.

When we run a sh file, it creates a new shell, executes and ends. So, if we're passing values to variables in our script, they wont be available after we run/end that script. However, if we run the script using `source ./myscript.sh`, the assigned variables would be available later.
```
> cat setx.sh
x=22
> chmod +x setx.sh
> echo $x
   (null)
> source ./setx.sh
> echo $x
22
>
```

### Aliases
The alias command allows short command alternatives: `alias ll="ls -la"`. Paths to commands cannot be used fo aliases.

To list the defined aliases, type `alias`

To remove an alias, do it with `unalias #alias_name` command.

### Echo
It's built into Bash and does not start a new process.

* `-n` don't print a trailing new line
* `-e` enable backslash escaped chars like `\n`and `\t`
* `-E` disable backslash escaped chars in cae they were enabled by default

`ls *` would list contents of directories, however `echo *` would show file and directory names.

We can use file direction techniques to send output to other files, like *stderr*:
`echo "Warning!" > &2` would send the output to the second file, which is `stderr` (first one is `stdout`)

## Variables, Control Structures and Arithmetic

### Local Variables and Typeset
Variables can be created in a function that will not be available outside of it.
the `typeset` command makes variables local, can provide a type or formatting. Arithmetic operations are faster when we define variables as integer.

```
> typeset -i x
# x must be an integer
```
Let allows for convenient arithmetic operations:
`let x++; let y=x**2; let x=x+3; let x*=5; ...`

#### Declare
`declare -l` uppercase values in the variable are converted to lowercase, whereas `declare -u` will make all uppercase.

`declare -r`will make the variable read-only.

`declare -a myArray` will make myArray an *indexed array*

`declare -A myArray` will make myArray an *associative array*

#### Read
Read command lets you read a line into a variable or multiple variables.

`read a b--` reads first word into `a`
 and the rest into `b`

#### While loop
Syntax:
```
while
    command list 1
do
    command list
done
# loops while command list 1 succeeds
```
Example 1: Loops through 1 to 10 and writes the outputs to files data.(1-10)
```
while
    ((x<10))
do
    echo loop $x; date >data.$x
    ((x=x+1))
done
```
Example 2: We're asking the while loop to get its data from data_file
```
while
    read a b
do
    echo a is $a b is $b
done <data_file
```
Example 3: Here we're piping the ls output to the while loop
```
ls -l | while
  read a b c d
  do
    echo owner is $c
  done  
```

#### For Loops
Syntax:
```
for <var> in <list>
do
  command list
done
```
<list> may come from various sources.
Example:
```
for i in dog cat bird
do
  echo i is $i
done
```
Seq command provides a sequence.
```
seq 1 5
# prints 1 2 3 4 5
for num in `seq 1 5`
# loops over 1 2 3 4 5
```
In Bash, when we use back-ticks it means that we're running a command between those back-ticks and pipes the output to the command calling it and it's not a comment.

We can also generate sequences using curly brackets and two dots in between: `{A..Z}`, `{1..10}`, etc. This can also be used instead of seq command.

Example: `$()` is an alternative to using back-ticks. The following code will cat the data_file and loop through the contents word by word (not by line by line)
```
for d in $(<data_file)
# loops over space/newline
# separated data in data_file
```
Example: `*.c` would retrieve all the file names with extension `c`in the current directory.
```
for j in *.c
# making a list with file globbing
for f in $(find . -name *.c)
# same result using a command to generate the list
```

#### Functions
Syntax:
```
function <name> {
  function body ...
}
```
Example: Shell evaluates the function like a new command. The code following the `return` command is not executed.
```
function printHello {
  echo Hello
  return
  echo "This won't print"
}
printHello
```
Functions produce results by writing output like commands do:
`hvar=$(printHello)`

`exit <value>` sets the exit status represented by `$?`to <value>

`exit` also terminates the shell process. If used in a function, *exit* would terminate the whole shell program but not just the function.

#### Redirection and pipes
Processes normally have three files open:
`0 ==> stdin, 1 ==> stdout, 2 ==> stderr`

```
command > stdout-here 2> stderr-here < stdin-from-here
```
The syntax above specifies that the *command* will take the input from `<`, write the output to `>` and errors will be directed to `2>`

If we write `&>` instead, it means that both the stdout and stderr will be directed to the file: `command &> file` (file is created or overwritten).

Pipe `|` changes stdin/stdout, piping the first commands output as the second command's input.
```
command1 | command2
# stdout of command1 is now the stdin of command2
```
```
command1 2>&1 | command2
# gets stdout and stderr from command1 and pipes it as stdin for command2
command1 |& command2
# this is the same as above
```
```
command >> file
# appends stdout to the end of file (does not overwrite). If file doesn't exists, it is created.
```
`<<` allows a way to embed input for stdin inside of a script.

```
sort <<END
cherry
banana
apple
END
```
Here, we used `END` to define the boundries for our input but we could use anything else, e.g. EOF, INPUT, etc.. When the script sees the exact same word at the beginning and at the end, it would understand that it's a boundry.

*Open and Close File Descriptors*
```
exec N< myFile
# opens file descriptor N for reading from file myFile
exec N> myFile
# opens file descriptor N for writing to file myFile
exec N<> myFile
# opens file descriptor N for reading and writing with file myFile
exec N>&- or exec N<&-
# closes file descriptor N
```
Example: Use lsof to see what file descriptors for a process are open
```
exec 7>/tmp/myFile7
lsof -p $$
# $$ is shell's PID
```
#### Case Statement
Syntax: Order of the expressions is important; the code is executed in sequence.
```
case expression in
pattern1 )
  command list ;;
pattern2 )
  command list ;;
...
esac
```
Example:
```
case $ans in
yes|YES|y|Y|y.x ) echo "will do";; # checks if the response is exactly written as...
n*|N*           ) echo "will not do";; # checks if the response starts with n or N
[nN][oO]        ) echo "will not do it";; # checks if 1st char is n or N, and 2nd char o or O
*) echo "oops";; # checks all other responses
esac

```
*if-then-else*
Syntax: else section is optional
```
if
  command list # last result is used
then
  command list
[else
  command list]
fi
```
Example:
```
if
  grep -q important myFile
then
  echo myfile has important stuff
else
  echo myfile does not have important stuff
fi
```
Example:
```
#!/bin/bash
if
   test -x /bin/ls
then
   if
   [ ! -w /etc/hosts ]
   then
      if
      echo about to look for foobar
      grep -q foobar /etc/passwd
      then
         echo foobar found in /etc/passwd
      else
         echo foobar not found
      fi
   fi
else
   echo Oh no, /bin/ls not executable
fi

```

*Test*
Builtin `test` is used to check various conditions and set the return code with the result. An alternative to test is `[[]]`or `(())`. Example:
```
if test -f afile
...
if [[ -f bfile ]]
...
if test $x -gt 5
...
test -d ex1     # success if ex1 is a directory
test -f ex1     # success if ex1 is a file
test -s ex1     # success if ex1 exists and not empty
test -x ex1     # success if you have execute permission on ex1
test -w ex1     # success if you have write permission on ex1
test -r ex1     # success if you have read permission on ex1
...
# numeric comparisons using brackets
[[ex1 -eq ex2]]
[[ex1 -ne ex2]]
[[ex1 -lt ex2]]
[[ex1 -gt ex2]]
[[ex1 -le ex2]]
[[ex1 -ge ex2]]
# with parenthesis
((ex1 == ex2))
((ex1 != ex2))
((ex1 > ex2))
((ex1 < ex2))
((ex1 >= ex2))
((ex1 <= ex2))
((ex1 && ex2))
((ex1 || ex2))
```
## Filters and Parameters

### Filters
If a program in Linux reads from stdin and writes to stdout, it's considered as a filter. e.g. `head` and `tail`.

```
ls -l | head -5      # first 5 lines of ls -l
ls -l | tail -5      # last 5 lines
ls -l | head -10 | tail -5     # lines 6-10
```

`wc`(word count) prints line, word and char counts.

```
wc -l       # prints the number of lines
ls | wc -l  # number of entries in directory
```
Example: (let's name it makeoutput.sh). This script will read a line (which is actually the date command here) 100 times, 4th word (d) is the time data.
```
#!/bin/bash
for i in {1..100}
do
  read a b c d e <<END
  $(date)
END
  echo $d
  sleep 1
done
```
Let's run the script:
```
$ ./makeoutput.sh
14:11:02
14.11.03
14.11.04
14.11.05
^C
```
Now we'll the script again but this time we'll send the output to a file. We also add `&` to make the script run in the background.
```
$ ./makeoutput.sh >output &
```
While the script is running in the background, we can now use `tail`to check the output from the file. (-n2 -> show the last 2 lines, -f -> follow). So, this command will be running until we manually stop it, and will show us the content from *output* file as soon as a new entry is added to it.
```
$ tail -n2 -f output
```

### sed and awk
`sed` command is a *stream editor*, it's not interactive, works as a filter and is ideal for batch editing tasks. It does the diting on all lines of the input at once. We can also use the `-i` option to change the contents of a file instead of just echoing the modified output to stdout.

`sed 's/old/new/' myfile` would substitute the first occurrence of *old* on each line for *new* in *myfile* and display the result on stdout. *old* is a pattern and can be a regular expression. This command would substitute only the first occurrences of *old*, but if we want to do it for all occurrences, we need to add a `g` to the end of the command to tell it we want *global* check. `sed 's/old/new/g' myfile`

Examples:
```
sed 's/@home/@work/; s/bike/car/'   
# perform two substitutions
sed -e 's/[xX]/Y' -e 's/b.*n/blue/'
# again performs two substitutions (with -e parameter), uses reg.exp.
sed -f sedscript -n sed4  
# with -f, we're getting the sed commands from the file <sedscript> and with -n, only some of the output is printed which are defined in file <sed4>
...
sed '/alpha/s/beta/gamma'
# if the line has alpha on it, then substitute beta to gamma
sed '/apple/,/orange/d'
# find a line that has apple on it, then go to the line that has orange on it and then delete all the lines in between
sed 'important/!s/print/throw/'
# if a line has important on it don't substitute, if it does not have then substitute
sed 's/apple/banana/' some.txt
# run the sed command on file some.txt
sed -f sed1 some.txt
# run the sed command on some.txt with instructions in sed1 file (content: s/apple/banana). A sed file can have multiple instructions in it separated by new line
```

`awk` is a pattern matching language that works as a filter and is good for writing reports. It processes a line at a time (like *sed*) and breaks each line into fields *$1, $2, etc.* (*$0* means the entire line) delimited by a variable (default: space)

Example: We pipe the output of *ps -el* to *awk*. If the line has *pts* on it, or col#8 matches 35 then *printf* with format 5 digit number, 5 digit number, string, new line with col#4, col#5, col#14
```
$ ps -el | awk '/pts/||$8~/35/{printf("%5d %5d %s\n", $4, $5, $14)}'
```
Example:
```
$ cat awk1
#!/bin/awk -f
{szsum+$9
rsssum+=$8}
END{printf("RSS\tSZ\n%d\t%d\n",rsssum, szsum)}

$ ps -ly | ./awk1
RSS         SZ
34629       237862
```

Example: Count words in an input file
```
$ cat words.awk
{for (i=1;i<NF;i++)
    words[$i]++}
END{printf("is=%d, ls=%d, the=%d\n",
words["is"], words["ls"], words["the"])}


$ man ls | col -b | awk -f words.awk
is=56,ls=14,the=130

```

### Parameters

Parameters of a shell script are considered as positional strings like $1, $2, etc. To reference multidigit, we use `{}` e.g. `${10}`. `$0` is the path to the program itself, e.g. `echo Usage: $0 arg1 ...`

`shift` moves parameters, e.g. $2 into $1, $3 into $2, etc.

`!` in curly braces helps us define an indirection. e.g.
```
x=abc
abc=def
echo ${!x}     # this prints def
```

`:` in curly braces helps us unset or null variables. e.g.
```
x=${var:-apple}
# with :- , if var is unset/null return value, otherwise return value of var

x=${var:=apple}
# with := , if var is unset/null, var is assigned the value and returned

# :? would display an error and exit script if var is unset/null
# :+ would return nothing if var is unset/null, otherwise will return value
${var:offset}      # value of var starting at offset
${var:offset:len}  # value of var starting at offset up to length len
${#var}            # length of var
${var#prefix}      # remove matching prefix from the var
${var%suffix}      # remove suffix from the var
```

Example:
```
$ cat pos.sh
#!/bin/bash
echo arg1 is $1 arg11 is ${11}
shift
echo now arg1 is $1 arg11 is ${11}
echo program is $0


$ ./pos.sh {A..Z}
arg1 is A arg11 is K
now arg1 is B arg11 is L
program is ./pos.sh
```

Example:
```
$ cat prepost.sh
p="/usr/local/bin/hotdog.sh"
echo Full path is $p
echo Prefix removed ${p#/*local/}
echo Suffix removed ${p%.sh}
cmd=${p#*/bin/}
cmd2=${cmd%.sh}
echo The command without .sh is $cmd2


$ ./prepost.sh
Full path is /usr/local/bin/hotdog.sh
Prefix removed bin/hotdog.sh
Suffix removed /usr/local/bin/hotdog
The command without .sh is hotdog

```

## Debugging
`bash prog` Runs the *prog* and does not need execute permissions.

`bash -x prog` Echoes commands after processing. We can also `set -x` or `set +x` inside the script. `set -u` reports usage of an unset variable (usually put to the top of the script)

`bash -n prog` Does not execute commands but checks for syntax errors only.

### Trap
Bash `trap`command is used for signal handling.  In the script, we can change behavior of signals, ignore them during critical sections, allow the script to die gracefully when Ctrl+C is pressed and perform some operations when the signal is detected. (We can list the active signal on linux using `kill -l`)

Example:
```
$ cat trapint.sh
trap "echo just got int; exit" INT     # Ctrl+C interrupt signal
trap "echo you cannot quit now" QUIT   # Ctrl+\ quit signal
cd /
while
true
do
    echo looping
    du -m * 2>/dev/null
    echo sleeping
    sleep 5
done

```

### Eval

`eval` command is used to have Bash 'evaluate' a string. Can be risky if the script asks user to input a string to run the evaluated command. Check http://mywiki.wooledge.org/BashFAQ/048 for more info.

### Getopt

`getopt`is used to process command-line options. It can process option names, long and single letter, etc. e.g. `opts='getopt' -o a: -l apple -- "$@"`   

Example:
```
function usage {
   echo Options are -r -h -b --branch --version --help
   exit 1
}
function handleopts {
    OPTS=`getopt -o r:hb::  -l branch::  -l help -l version -- "$@"`
    if [ $? != 0 ]
    then
        echo ERROR parsing arguments >&2
        exit 1
    fi
    eval set -- "$OPTS"
    while true ; do
        case "$1" in
            -r ) rightway=$2
                shift 2;;
            --version ) echo "Version 1.2";
                 exit 0;;
            -h | --help ) usage;
                 shift;;
            -b | --branch)  
                case "$2" in
                "") branchname="default"  ; shift 2 ;;
                 *) branchname="$2" ; shift 2 ;;
                esac ;;
            --) shift; break;;
        esac
    done
    if [ "$#" -ne 0 ]
    then
	echo Error extra command line arguments "$@"
	usage
    fi
}
rightway=no
handleopts $@
echo rightway = $rightway
echo branchname = $branchname
```
