---
title: Bash Notes
tags:
---

# Notes
`./script.sh` and `sh script.sh` runs the script in a new subshell of current shell, while `source script.sh` executes it in current shell.

Debug a script using `bash -x script.sh`. It will output every command before executing it. `set -x` activates the debugging mode, and `set +x` deactivates it.

`set -f` disables file name globbing, and `set -v` enables verbose mode.



# General
* \#! (sha-bang) at first line: the path to the program that interprets the command in the script, usually is the bash. For example, `#! /bin/bash`
* invoking the script: `sh scriptname`, or `bash scriptname`. Or to make the script executable with `chmod`. Then `./scriptname` should do the job
* \# is the comment character, can follow the end of a command
* exit: 0 means successful, other code means unsuccessful, and returns the error code
* arguments passed to the script in command line is $0, $1, etc. $0 is the script name
* When current directory is changed, the directory is pushed into the `DIRSTACK` variable by `pushd`. It can be removed by `popd`.


## Redirecting
* stdin, stdout(1), stderr(2)
* redirecting: `>`

Example:

* stdout to file: `command > file`
* stderr to file: `command 2> file`
* stdout to stderr: `command 1>&2`
* stdout and stderr to file: `command &> file`

## Pipe
`|` connects output to previous program to the next one.

## Selection
```bash
OPTIONS="op1 op2"
select opt in "$OPTIONS"; do
	if [ "$opt" == "op1" ]; then
		:
	elif [ "$opt" == "op2" ]; then
		:
	else
		: # error
	fi
done
```

It creates a simple select menu with each word in `OPTIONS` as a choice.

## Command line arguments
`$0` is the command itself, `$1` is the first argument, etc. This can build a useful command line interface.

## Read user input
```bash
read NAME				# read everything into $NAME
read FNAME LNAME 	# read first word into $FNAME, second word into $LNAME
```

# Variable
* Variable name cannot start with digits.
* Define a variable: `v=1`. Whitespaces around the equal sign will cause error. Put right-hand-side strings in quotes is a good habit.
* Use a variable `$v, ${v}, "$v", "${v}"`
* `${VAR:=value}` defines the named variable if not existed.
* `$( ls )` captures the output from `ls` and store it in an anonymous variable. It's the same as in \`cmd\`.
* Quoting variables preserves whitespace. When whitespace is presented in variable definition, then the right-hand-side value should be double quoted.
* An uninitialized variable has a null value
* `local var=1` defines `var` as local.

## Special variables
* `$@`: positional parameters, starting from one. When the expansion occurs within double quotes, each parameter expands to a separate word. `$*` means the same but it should be avoided.
* `$#`: the number of positional parameters in decimal.
* `$$`: the ID of current shell. `$!`: the ID of most recent background process.
* `$?`: the return value (0 if successful) of the previous command.

## Quoting characters
* `\`: escapes the special characters.
* Single quotes preserve **the literal value of each character enclosed within the quotes**. A single quote may not occur between single quotes, even when preceded by a backslash.
* Double quotes preserve the literal value of all characters enclosed, except for the dollar sign, the backticks, and the backslash.

## Shell expansion
* Brace `{}`: `sp{i,e}l` is equivalent to `spil spel`.
* Tilde `~`: `~` alone is subsituted with `HOME` or home directory of current user executing this command, `~+` is `PWD`, and `~-` is `OLDPWD` (the previous working directory).
* `"${v}"` expands to the parameter `v`. The `{}` is required if `v` is a positional parameter of more than one digit.
* `"${!v}"` expands to the parameter with name as value evaluated from `v`. For example, `"${!N*}"` expands to all variables starting with `N`, rather than the variable with name `N*`.
* `$(cmd)` and \`cmd\` replaces the variable with the output from executing `cmd`.

# Arithmetic evaluation
* `((1+1))` or `[1+1]` permits arithmetic expansion and evaluation, thus C-style arithmetic evaluation in bash. `[1+1]` will do no checks.
* `let` can also do this.

# Array
* `declare -a array=(elem1 elem2)` can declare an array and initialize the array.
* `name[index]=value` can create an array.
* To access an element from an array, use `${name[index]}`.
* `${array[@]}` is the whole array.
* Arrays are zero-based. Members don't need to be indexed continuously.
* `${#arrayname[@]}` is the length of the array. `${#arrayname[n]}` is the length of the nth element in an array.
* `${array[@]:3:2}` extracts 2 elements starting from the position 3 from an array. `${array[2]:0:4}` extracts first 4 elements from `array[2]`.
* `${array[@]/old/new}` replaces "old" with "new" in an array and return the new array. It does not change the original array.
* `array=("${array[@]}" elem5 elem6)` appends two more elements to an array.
* `unset array[n]` removes nth element from array. All other elements remain at their original place. `array=(${array[@]:0:$n} ${array[@]:$(($n + 1))})` will remove nth element from array and fill the gap.
* `new=("${old[@]}")` copies an array.
* `concat=("${array1[@]}" "${array2[@]}")` concatenates two arrays.
* `unset array` is used to delete an entire array.
* `array=( "$multiline" )` puts each line as an element in the array.
 
# Condition

## if-else
```bash
if TEST-COMMANDS; then
	:
elif TEST-COMMANDS; then
	:
else
	:
fi
```

Note: `then` and `fi` are considered separate statements in the shell, so use semi-colon if they are not in their own lines.

`TEST-COMMANDS` is true of the return status is zero. Usually use `[ ]` as the test expression.

If there are no elif or else statements, it is equivalent to

```bash
test EXPR && ( CMD )
```

where EXPR is testing expressions inside `[]`.

File test operators: `[ -e filename ]`

* `-e` and `-a` true if file exits 
* `-f` the file is a regular file, not a directory or device file
* `-d` it's a directory
* `-h` it's a symbolic link
* `-r` user has read permission
* `-w` user has write permission
* `-x` execute permission
* `[ FILE1 -nt FILE2 ]` and `[ FILE1 -ot FILE2 ]` true of FILE1 is newer/older than FILE2 or FILE1 exists while FILE2 does not.


Integer comparison operators: `[ var1 -eq var2 ]`

* `-eq` is equal to
* `-ne` is not equal to
* `-gt` is greater than 
* `-ge` is greater or equal to
* `-lt` is less than
* `-le` is less than or equal to
* `<`, `<=`, `>`, and `>=` can be used inside double parentheses: `(("$a" < "$b"))`

String comparison operators:

* `-z` true if the length of string is zero.
* `-n` or `[ str ]` true of the length of string is non-zero.
* `=` and `==` are for equality test. Whitespace should frame `=`: `if [ "$a" = "$b" ]`
* `!=` is not equal to, same usage as `=`
* `<` and `>` for comparison in ASCII alphabetical order. In single bracket use `\<` and `\>` instead: `if  [[ "$a" < "$b" ]]` but `if [ "$a" \< "$b" ]`

Combining expressions:

* Negation: `[ !EXPR ]` true if EXPR evaluates to false.
* And: `[ EXPR1 -a EXPR2 ]`
* Or: `[ EXPR1 -o EXPR2 ]`
* () can be used to override precedence of operators.

# case

```bash
case EXPR in
	CASE1) : ;;
	CASE2) : ;;
esac
```

# Loop
for loop:

```bash
for i in some_array; do 
	:	
done
```

Example:

```bash
for i in $( ls ); do
	echo item: $i
done
```
	
C-style for:

```bash
for i in `seq 1 10`; do
	echo $i
done
```

while loop:

```bash
while [ condition ]; do
	:
done
```

until loop:

```bash
until [ condition ]; do
	:
done
```

# Function

Definition:

```bash
function my_func {
	exit
}
```
	
Call function with parameter:

```bash
my_func e f g
```

so `e` is stored in `$1`, `f` is in `$2`, etc.

#String editing
## sed
The sed program can perform text pattern substitutions and deletions using regular expressions, like the ones used with the grep command.

Editing commands:

* `a\`: Append text below current line.
* `c\`: Change text in the current line with new text.
* `d`: Delete text.
* `i\`: Insert text above current line.
* `p`: Print text.
* `r`: Read a file.
* `s`: Search and replace text.
* `w`: Write to a file.

Options:

* `-n`: silent mode, do not print every line.

Usage:
`sed cmd file` where cmd is a string in single quotes as follows:

* Search for patterns: `/pattern/p` where `pattern` can be a regex, and `p` is the editing commands.
* Range of lines: `start,endp` where start and end can be line numbers (`$` can be used to represent end of file), and `p` is the editing commands. `/start_pattern/, /end_pattern/p` means range between first occurrence of `start_pattern` and `end_pattern`.
* Find and replace: `s/old/new/` finds and replaces the first occurrence of `old` with `new` and `s/old/new/g` finds and replaces all.

## awk
Run awk:

* `awk PROGRAM inputfile` runs awk program on the specified input file.
* `awk -f program_file inputfile` runs awk program specified in program_file.

Variables:

* Variables are either strings or numeric values.
* `$FS` defines the input field separator.
* `$OFS` defines the output field separator.
* `$ORS` defines the output record separator. Each line processed is considered as a record. Thus it specifies separators between lines. It defaults to newline.
* `$NR` holds the number of lines (records) processed. It is incremented by 1 after a line is read.
* `$0` holds each line that awk reads.
* `$i` holds the ith field in the current line.
* awk initializes an undefined variable to null string.
* C-style shorthands like `VAR+=VALUE` is also accepted.

Methods:

* `print` outputs parameters to stdout. It takes arbitrary number of parameters **separated by commas** and they are space-separated by default in `$OFS`. If no comma is used, awk treat a series of parameters as one and concatenate them into one. Use `\t` and `\n` for tab and newline. 
* `printf` has more precise control over the output format. The formatting is the same as C-language `printf` statement.

Metacharacters (`$, ", /`) should be escaped.

Statements:

* `BEGIN { cmd }` executes cmd before all lines are processed. It is helpful to set `$FS` in BEGIN using `FS=":"`.
* `END { cmd }` executes cmd after all lines are processed.

## cut
TBD