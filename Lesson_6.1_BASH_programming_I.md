UNIX Lesson 6.1: BASH Programming I
===================


# Bash Programming

> It's time to automate
 
Until now we have discussed how to use the bash shell. `Bash` itself is a
little programming language, and this chapter we're going to discuss how you can
write your own computer programs in `Bash`. Programming in Bash is useful to know
because of how seamlessly it integrates with all of the command line programs
you've already learned. By the end of this lesson you should be able to write
your own command line tools!

You can use `nano` to write all of the programs we're going to discuss in this
chapter, however I recommend using the [Sublime text](https://www.sublimetext.com/) text editor
because it's more user friendly.

Here the instruction to download and install Sublime text:

```
sudo apt-get update
sudo apt-get install sublime-text
```



**Now let's create a new file called `math.sh`** in the `~/bioinformatic_course/Code/` directory
and let's open that file with either `nano` or Sublime.


```bash
cd ~/bioinformatic_course/Code/
nano math.sh
```

You should now have a new, clean text file open. Any code block in the following sections that starts with the code (or similar) below should indicate to you that we're working on a particular text file.

```
#!/usr/bin/env bash
# File: math.sh
```

You do not need to add these lines to your file. **Note**: please type all of the lines out
for every program that we're going to write, do not copy and paste. Typing code
is a little different from typing an email, and you should practice typing the
code out yourself as much as possible. Both of these lines start with the pound
symbol (`#`) and in the Bash programming language anything that is typed after
a pound symbol is ignored (unless the pound symbol is between curly brackets
(`{ }`), but that's only in very specific situations). The pound symbol allows
you to make **comments** in
your code which you can use to annotate code so that another human being who is
reading your code can understand how your program is designed to function.

If you're using `nano` or another shell-based text editor you should perhaps
open up two terminals, one where you can edit and save the programs you're
working on, and one where you can run your programs. One advantage of using Sublime text is that you can keep Sublime open in a separate window and then run your programs in your terminal window.

<br />

## Arithmetic operations

The Bash programming language can do very basic arithmetic, which we'll
demonstrate in this section.
Now that you have `math.sh` open in your preferred text editor type the following
into your text editor:

```
#!/usr/bin/env bash
# File: math.sh

expr 5 + 2
expr 5 - 2
expr 5 \* 2
expr 5 / 2
```

Save `math.sh` and then run this script in your shell:


```bash
bash math.sh
```

```
## 7
## 3
## 10
## 2
```

Let's break down what's going on in the Bash script you just created. Bash
executes programs in order from the first line in your file to the last line.
The `expr` command can be used to **evaluate** Bash **expressions**.
**An expression is just
a valid string of Bash code that, when run, produces a result**. The arithmetic
operators that you're already familiar with for addition (`+`), subtraction
(`-`), and multiplication (`*`) work like you would expect them to. Notice that
when doing multiplication you need to escape the star character, otherwise
Bash thinks you're trying to create a regular expression! The division operator
(`/`) does not work as you might expect it to since 5 / 2 = 2.5. Bash does
**integer division**, which means that **the result of dividing one number by
another is always rounded down to the nearest integer**. Let's take a look at a
few examples on the command line:


```bash
expr 1 / 3
expr 10 / 3
expr 40 / 21
expr 40 / 20
```

```
## 0
## 3
## 1
## 2
```

The other numerical operator you should be aware of that you might not be
familiar with is the modulus operator (`%`). The modulus operator returns the
**remainder** after integer division. In integer division if A / B = C, and
A % B = D, then B * C + D = A. Let's take a look at some examples on the command
line:


```bash
expr 1 % 3
expr 10 % 3
expr 40 % 21
expr 40 % 20
```

```
## 1
## 1
## 19
## 0
```

Notice that when one number is completely divisible by another number then the
result of the modulus is zero.

arithmetic expressions could be alse denoted with the following format:

```
#!/usr/bin/env bash
# File: math2.sh

echo $((10 + 5)) # => 15
echo $((3-1)) #substracttion 
echo $((1*6)) #multiplication
echo $((8/2)) #division
```
In this case you need to add `echo` command before the arithmetic operation. 


If you want to do more complex math, for example math with fractions and numbers
with decimals then I highly suggest combining `echo` and the **b**ench **c**alculator
program called `bc`. Open up a new file called `bigmath.sh` and type in the
following:

```
#!/usr/bin/env bash
# File: bigmath.sh

echo "22 / 7" | bc -l
echo "4.2 * 9.15" | bc -l
echo "(6.5 / 0.5) + (6 * 2.2)" | bc -l
```

Save `bigmath.sh` and then run this script in your shell:


```bash
bash bigmath.sh
```

```
## 3.14285714285714285714
## 38.430
## 26.2
```

You can pipe any mathematical string to `bc` with the `-l` flag in order to
use decimal numbers in your calculations.

### Summary

- Bash programs are executed in order from the first line in a file until the
last line.
- Anything written after a pound sign (`#`) is a comment and is not executed by
Bash.
- You can do simple arithmetic with the `expr` command or with `echo $(())` expression.
- Perform more complicated arithmetic by piping a string expression into `bc`
using `echo`.

### Exercises

1. Look at the `man` pages for `bc`.
2. Try doing some math in `bc` interactively.

<br />

## Variables

In Bash you can store data in variables. Make sure you follow these rules when you're naming variables:

- Every letter should be lowercase.
- The variable name should start with a letter.
- The name should only contain alphanumeric characters and underscores (`_`).
- Words in the name should be separated by underscores.

If you follow those rules then you can avoid accidentally overwriting data
stored in environmental variables.

You can assign data to a variable using the equals sign (`=`). The data you
store in a variable can either be a string or a number. Let's create a variable
now on the command line:


```bash
number=5
```

The variable name is on the left hand side of the equals sign, and the data
which will be stored in that variable is on the right hand side of the equals
sign. Notice that there are no spaces on either side of the equals sign, this
is not allowed when assigning variables:


```bash
number = 5
```

```
## Error in running command bash
```

In order to print the data in a variable, also called the value of a variable,
we can use `echo`. When you want to retrieve the value of a variable you must
use the dollar sign (`$`) before the name of the variable. Let's try this out:


```bash
echo $number
```

```
## 5
```

You can modify the value of a variable using arithmetic operators by using the
`let` command:


```bash
let number=$number+1
echo $number
```

```
## 6
```

or 


```bash
number=$(($chapter_number+1))
echo $number
```
```
## 6
```


You can also store strings in variables:


```bash
the_empire_state="New York"
echo $the_empire_state
```

```
## New York
```

Occasionally you might want to run a command like you would on the command line
and store the result of that command in a variable. We can do this by wrapping
the command in a dollar sign and parentheses (`$( )`) around a command.
This syntax is called **command substitution**. The command is executed and then
gets replaced by the string that resulted from running the command. For
example if we wanted to store the number of lines in `math.sh`:


```bash
math_lines=$(cat math.sh | wc -l)
echo $math_lines
```

```
## 7
```

Variable names with a dollar sign can also be used inside other strings in
order to insert the value of the variable into the string:


```bash
echo "I went to school in $the_empire_state."
```

```
## I went to school in New York.
```
<br />

## Scritp arguments


In Bash scripts, there is an option to pass arguments to the script when you are executing it. This allows the user to specify more information in the same command used to run the script. These arguments are stored in the script in a few special built-in variables. 

Let's create a new file called `vars.sh` with the following code:

```
#!/usr/bin/env bash
# File: vars.sh

echo "Script arguments: $@"
echo "First arg: $1. Second arg: $2."
echo "Number of arguments: $#"
```

Now let's try running the script a few times in a few different ways in order to undestand what exactly do the arguments:


```bash
bash vars.sh
```

```
## Script arguments:
## First arg: . Second arg: .
## Number of arguments: 0
```


```bash
bash vars.sh red
```

```
## Script arguments: red
## First arg: red. Second arg: .
## Number of arguments: 1
```


```bash
bash vars.sh red blue
```

```
## Script arguments: red blue
## First arg: red. Second arg: blue.
## Number of arguments: 2
```


```bash
bash vars.sh red blue green
```

```
## Script arguments: red blue green
## First arg: red. Second arg: blue.
## Number of arguments: 3
```

Your script can accept arguments just like a command line program! The first
argument to your script is stored in `$1`, the second argument is stored in
`$2`, etc, etc. An array of all of the arguments passed to your script is stored
in `$@`, and we'll discuss how to handle arrays later on in this lesson. The
total number of arguments passed to your script is stored in `$#`. Now that you
know how to pass arguments to your scripts you can start writing your own command line tools!

### Summary

- Variables can be assigned with the equal sign (`=`) operator.
- Strings or numbers can be assigned to variables.
- The value of a variable can be accessed with the dollar sign (`$`) before the
variable name.
- You can use the dollar sign and parentheses syntax (command substitution) to
execute a command and save the output in a variable.
- You can access command line arguments within your own scripts using the dollar
sign followed by the number of the argument.
- `$1..$n` store positional arguments. `$@` stores all the argument into an array. `$#`stores the total number of arguments.

### Exercises

1. Write a Bash program where you assign two numbers to different variables,
and then the program prints the sum of those variables.
2. Write another Bash program where you assign two strings to different
variables, and then the program prints both of those strings. Write a version
where the strings are printed on the same line, and a version where the strings
are printed on different lines.
3. Write a Bash program that prints the number of arguments provided to that
program multiplied by the first argument provided to the program.
4. Write a Bash program that calculate the %GC of a short dna sequence provided as an argument 'ATGTGTAAAGGTGTGTGGTG'. The result have to show decimals

<br />


## User Input

If you're making Bash programs for you or for others to use one way you can get
user input is to specify arguments for users to provide to your program, as we
discussed in the previous section. You could also ask users to type in a string
on the command line by temporarily stopping the execution of your program using
the `read` command. Let's a write a small script where you can see how the read
command works:

```
#!/usr/bin/env bash
# File: letsread.sh

echo "Type in a string and then press Enter:"
read response
echo "You entered: $response"
```

Now let's run this script:


```bash
bash letsread.sh
```

```
## Type in a string and then press Enter:
##
```

Let's type `Hello!` into the console, then press enter:

```
## Type in a string and then press Enter:
## Hello!
## You entered: Hello!
```

The `read` command prompts the user to type in a string, and the string that the
user provides is stored in the variable that is given to the `read` command in
the script.

### Summary

- `read` stores a string that the user provides in a variable.

### Exercises

1. Write a script that asks the user for an adjective, a noun, and a verb, and
then use those words in a sentence (like [Mad Libs](https://en.wikipedia.org/wiki/Mad_Libs)).


## Logic and If/Else

### Conditional Execution

When writing computer programs it is often useful for your program to be able to
make decisions based on inputs like arguments, files, and environmental
variables. Bash provides mechanisms for creating **logical expressions** which
resemble mathematical equations. These logical expressions can be evaluated
until they are either true or false. In fact, `true` and `false` are both simple
Bash commands! Let's try them both out now:


```bash
true
false
```

At first it doesn't look like they do much. In order to see how they work, we're
going to need to look under the hood of Unix a little bit. Whenever you execute
a program on the command line, in general one of two things will happen: either
the command is executed successfully, or there's an error. In terms of errors
there are many ways that a program can go wrong, and Unix can take different
actions depending on what kind of error occurs. For example if I enter the name
of a command that does not exist into the terminal, then I'll see an error:


```bash
this_command_does_not_exist
```

```
## -bash: this_command_does_not_exist: command not found
```

Since that command does not exist, it creates a specific kind of error which is
indicated by the program's **exit status**. The `exit status` of a program is an
integer which indicates whether the program was executed successfully or if
an error occurred.
The exit status of the last program run is stored in the question mark
variable (`$?`). We can take a look at the exit status of the last program with
`echo`:


```bash
echo $?
```

```
## 127
```

This particular `exit status` made an indication to the shell that it should
print an error message to the console. What's the exit status of a program that
runs successfully? Let's take a look:


```bash
echo I will succeed.
echo $?
```

```
## I will succeed.
## 0
```

So the exit status of a successful program is 0. Now let's take a look at the
exit statuses of `true` and `false`:


```bash
true
echo $?
false
echo $?
```

```
## 0 #true
## 1 #false
```

As you can see:`true` has an exit status of 0 and `false` has an exit status of 1 since these programs don't do much else, you could define `true` as a program
that always has an exit status of 0 and `false` as a program that always has an
exit status of 1.

Knowing the exit status of these programs is important when discussing the
**logical operators**: the AND operator (`&&`) and the OR operator (`||`). The
AND and OR operators can be used for conditional execution of programs on the
command line. Conditional execution occurs when the execution of one program
depends on the exit status of another program. For example in the case of the
AND operator, the program on the right hand side of `&&` will only be executed
if the program on the left hand side of `&&` has an exit status of 0. Let's
take a look at some small examples:


```bash
true && echo "Program 1 was executed."
false && echo "Program 2 was executed."
```

```
## Program 1 was executed.
```

Since `false` has an exit status of 1, the program `echo "Program 2 was executed."`
is not executed, so nothing is printed to the console for that command.
Several AND operators can be chained together like so:


```bash
false && true && echo Hello
echo 1 && false && echo 3
echo Athos && echo Porthos && echo Aramis
```

```
## 1
## Athos
## Porthos
## Aramis
```

In a series of programs joined together by AND operators, any programs to the
right of a program that has a **non-zero exit status** is not executed.

The OR operator (`||`) follows a similar set of principles. Commands on the right
hand side of `||` are only executed if the command on the left hand side *fails*
and therefore has an exit status other than 0. Let's take a look at how this
works:


```bash
true || echo "Program 1 was executed."
false || echo "Program 2 was executed."
```

```
## Program 2 was executed.
```

Only `echo "Program 2 was executed."` runs because `false` has a **non-zero exit
status**. You can combine multiple OR operators so that only the first program
with an exit status of 0 is executed:


```bash
false || echo 1 || echo 2
echo 3 || false || echo 4
echo Athos || echo Porthos || echo Aramis
```

```
## 1
## 3
## Athos
```

You can combine AND and OR operators in commands, which are evaluated from left
to right:


```bash
echo Athos || echo Porthos && echo Aramis
echo Gaspar && echo Balthasar || echo Melchior
```

```
## Athos
## Aramis
## Gaspar
## Balthasar
```

By combining AND and OR operators you can precisely control the conditions for
when certain commands should be executed.

<br />

### Conditional Expressions

Enabling your Bash script to make decisions is extremely useful. Conditional
execution allows you to control the circumstances where certain programs are
executed based on whether those programs succeed or fail, but you can also
construct **conditional expressions which are logical statements that are
either equivalent to `true` or `false`. Conditional expressions either compare
two values, or they ask a question about one value. Conditional expressions are
always between double brackets (`[[ ]]`), and they either use logical flags
or logical operators**. For example, there are several logical flags you could
use for comparing two integers. If we wanted to see if one integer was greater
than another we could use `-gt`, the **g**reater **t**han flag. Enter this
simple conditional expression into the command line:


```bash
[[ 4 -gt 3 ]]
```

The logical expression above is asking: Is 4 greater than 3? No result is
printed to the console so let's check the **exit status** of that expression.


```bash
echo $?
```

```
## 0
```

It looks like the exit status of this program is 0, the same exit status as
`true`. This conditional expression is saying that `[[ 4 -gt 3 ]]` is equivalent
to `true`, which of course we know is logically consistent, 4 is in fact greater
than 3! Let's see what happens if we flip the expression around so we're asking
if 3 is greater than 4:


```bash
[[ 3 -gt 4 ]]
```

Again, nothing is printed to the console so we'll look at the exit status:


```bash
echo $?
```

```
## 1
```

Ah-ha! Obviously 3 is not greater than 4, so this false logical expression
resulted in an exit status of 1, which is the same exit status as `false`!
Because they have the same exit status `[[ 3 -gt 4 ]]` and `false` are
essentially equivalent. To quickly test the logical value of a conditional
expression, we can use the AND and OR operators so that an expression will
print "t" if it's true and "f" if its false:


```bash
[[ 4 -gt 3 ]] && echo t || echo f
[[ 3 -gt 4 ]] && echo t || echo f
```

```
## t
## f
```

This is a little trick you can use to quickly look at the resulting value of a
logical expression.

These **binary** logical expressions compare two values, but there are also
**unary** logical expressions that only look at one value. For example, you can
test whether or not a file exists using the `-e` logical flag. Let's take a look
at this flag in action:


```bash
cd ~/Code
[[ -e math.sh ]] && echo t || echo f
```

```
## t
```

As you can see the file `math.sh` exists! Most of the time when you're writing
bash scripts you won't be comparing two raw values or trying to find something
out about one raw value, instead you'll want to create a logical statement
about a value contained in a variable. Variables behave just like raw values in
logical expressions. Let's take a look at a few examples:


```bash
number=7
[[ $number -gt 3 ]] && echo t || echo f
[[ $number -gt 10 ]] && echo t || echo f
[[ -e $number ]] && echo t || echo f
```

```
## t
## f
## f
```

As you can see 7 is greater than 3 though it is not greater than 10, and there
is not file in this directory called `7`. There are several other varieties of
logical flags, and you can find a table of several of these flags below.

| Logical Flag | Meaning | Usage |
|:-------------|:--------|:------|
| -gt | **G**reater **T**han | `[[ $variable -gt 8 ]]` |
| -ge | **G**reater Than or **E**qual To | `[[ $variable -ge 270 ]]` |
| -eq | **Eq**ual | `[[ $variable -eq 10 ]]` |
| -ne | **N**ot **E**qual | `[[ $variable -ne 0 ]]` |
| -le | **L**ess Than or **E**qual To | `[[ $variable -le 9 ]]` |
| -lt | **L**ess **T**han | `[[ $variable -lt 2 ]]` |
| -e | A File **E**xists | `[[ -e $variable ]]` |
| -d | A **D**irectory Exists | `[[ -d $variable ]]` |
| -z | Length of String is **Z**ero | `[[ -z $variable ]]` |
| -n | Length of String is **N**on-Zero | `[[ -n $variable ]]` |

Try using each of these flags on the command line before moving on to the next
section.

In addition to logical flags there are also logical operators. One of the most
useful logical operators is the regex match operator `=~`. The regex match
operator compares a string to a regular expression and if the string is a match
for the regex then the expression is equivalent to `true`, otherwise it's
equivalent to `false`. Let's test this operator a couple different ways:


```bash
[[ rhythms =~ [aeiou] ]] && echo t || echo f
my_name=joaquin
[[ $my_name =~ ^j.+n$ ]] && echo t || echo f
```

```
## f
## t
```

There's also the NOT operator `!`, which inverts the value of any conditional
expression. The NOT operator turns true expressions into false expressions and
vice-versa. Let's take a look at a few examples using the NOT operator:


```bash
[[ 7 -gt 2 ]] && echo t || echo f
[[ ! 7 -gt 2 ]] && echo t || echo f
[[ 6 -ne 3 ]] && echo t || echo f
[[ ! 6 -ne 3 ]] && echo t || echo f
```

```
## t
## f
## t
## f
```

Here's a table of some of the useful logical operators in case you need to
reference how they're used later:

| Logical Operator | Meaning | Usage |
|:-------------|:--------|:------|
| =~ | Matches Regular Expression | `[[ $consonants =~ [aeiou] ]]` |
| = | String Equal To | `[[ $password = "pegasus" ]]` |
| != | String Not Equal To | `[[ $fruit != "banana" ]]` |
| ! | Not | `[[ ! "apple" =~ ^b ]]` |

### If and Else

Conditional expressions are powerful because you can use them to control how a
Bash program that you're writing is executed. One of the fundamental constructs
in Bash programming is the **IF statement**. Code written inside of an `IF`
statement is only executed *if* a certain condition is true, otherwise the code
is skipped. Let's write a small program with an IF statement:

```
#!/usr/bin/env bash
# File: simpleif.sh

echo "Start program"

if [[ $1 -eq 4 ]]
then
  echo "You entered $1"
fi

echo "End program"
```

First this program will print "Start program", then the `IF` statement will check
if the conditional expression `[[ $1 -eq 4 ]]` is true (0 exit status). It will only be true if
you provide `4` as the first argument to the script. If the conditional
expression if true then it will execute the code in between `then` and `fi`,
otherwise it will skip over that code. Finally the program will print
"End program."

Let's try running this Bash program a few different ways. First we'll run this
program with no arguments:


```bash
bash simpleif.sh
```

```
## Start program
## End program
```

Since we didn't provide any arguments to `simpleif.sh` the code within the `IF`
statement was skipped! Now let's try providing an argument to this script:


```bash
bash simpleif.sh 77
```

```
## Start program
## End program
```

We provided the argument `77`, however 77 is not equal to 4, therefore the code
within the IF statement was once again skipped. Finally let's provide `4` as an
argument:


```bash
bash simpleif.sh 4
```

```
## Start program
## You entered 4
## End program
```

It worked! Since the first argument to this script was `4`, and 4 is equal to 4,
the code within the `IF` statement was executed. You can pair `IF` statements with
`ELSE` statements. **An `ELSE` statement only runs if the conditional expression being
evaluated by the IF statement is false**. Let's create a simple program that uses
an `ELSE` statement:

```
#!/usr/bin/env bash
# File: simpleifelse.sh

echo "Start program"

if [[ $1 -eq 4 ]]
then
  echo "Thanks for entering $1"
else
  echo "You entered: $1, not what I was looking for."
fi

echo "End program"
```

Now let's try running this program a few different ways:


```bash
bash simpleifelse.sh 4
```

```
## Start program
## Thanks for entering 4
## End program
```

The conditional expression `[[ $1 -eq 4 ]]` was true so code inside of the `IF`
statement was run and the code in the `ELSE` statement was not run. What do you
think will happened when we make the conditional expression false?


```bash
bash simpleifelse.sh 3
```

```
## Start program
## You entered: 3, not what I was looking for.
## End program
```

The conditional expression `[[ $1 -eq 4 ]]` was false (non-zero exit code) so code inside of the `ELSE`
statement was run and the code in the IF statement was not run.

Between `IF` and `ELSE` statements you can also have `ELIF` statements. These
statements act like `IF` statements except they're only evaluated if preceding
`IF` and `ELIF` statements have all evaluated false conditional expressions. Let's
create a brief program using `ELIF`:

```
#!/usr/bin/env bash
# File: simpleelif.sh

if [[ $1 -eq 4 ]]
then
  echo "$1 is my favorite number"
elif [[ $1 -gt 3 ]]
then
  echo "$1 is a great number"
else
  echo "You entered: $1, not what I was looking for."
fi
```

First let's run the program with `4` as the first argument:


```bash
bash simpleelif.sh 4
```

```
## 4 is my favorite number
```

The condition in the IF statement was true, so only the first `echo` command was
executed. Now let's run the program with `5` as the first argument:


```bash
bash simpleelif.sh 5
```

```
## 5 is a great number
```

The first condition is false since 5 is not equal to 4, but then the next
condition in the `ELIF` statement is true since 5 is greater than 3, so that
echo command is executed and the rest of the statement is skipped. Try to guess
what will happen if we use `2` as an argument:


```bash
bash simpleelif.sh 2
```

```
## You entered: 2, not what I was looking for.
```

Since 2 is neither equal to 4 nor greater than 3, the code in the `ELSE` statement
is executed.

You should also know that you can combine conditional execution, conditional
expressions, and **IF/ELIF/ELSE** statements. The conditional execution operators
AND (`&&`) and OR (`||`) can be used in an IF or ELIF statement. Let's look at
an example using these operators in an `IF` statement:

```
#!/usr/bin/env bash
# File: condexif.sh

if [[ $1 -gt 3 ]] && [[ $1 -lt 7 ]]
then
  echo "$1 is between 3 and 7"
elif [[ $1 =~ "Jeff" ]] || [[ $1 =~ "Roger" ]] || [[ $1 =~ "Brian" ]]
then
  echo "$1 works in the Data Science Lab"
else
  echo "You entered: $1, not what I was looking for."
fi
```
Now let's test this script with a few different arguments:


```bash
bash condexif.sh 2
## You entered: 2, not what I was looking for.

bash condexif.sh 4
## 4 is between 3 and 7

bash condexif.sh 6
## 6 is between 3 and 7

bash condexif.sh Jeff
## Jeff works in the Data Science Lab

bash condexif.sh Brian
## Brian works in the Data Science Lab

bash condexif.sh Sean
## You entered: Sean, not what I was looking for.
```



The conditional execution operators work just like they would on the command
line. If the entire conditional expression evaluates to the equivalent of `true`
then the code within the IF statement is executed, otherwise it is skipped.

Finally we should note that **IF/ELIF/ELSE** statements can be nested inside of
other `IF` statements. Here's a small example of a program with nested statements:

```
#!/usr/bin/env bash
# File: nested.sh

if [[ $1 -gt 3 ]] && [[ $1 -lt 7 ]]
then
  if [[ $1 -eq 4 ]]
  then
  	echo "four"
  elif [[ $1 -eq 5 ]]
  then
  	echo "five"
  else
  	echo "six"
  fi
else
  echo "You entered: $1, not what I was looking for."
fi
```

Now let's run it a few times:


```bash
bash nested.sh 2
## You entered: 2, not what I was looking for.

bash nested.sh 4
## four

bash nested.sh 6
## four
```

In order to get to the inner IF statement, the conditions for the outer IF
statement must be met first (the first argument for the script must be between
3 and 7). As you can see combining variables, arguments, conditional
expressions, and IF statements allow you to write more powerful Bash programs.


### Summary

- All Bash programs have an exit status. `true` has an exit status of 0 and
`false` has an exit status of 1.
- Conditional execution uses two operators: AND (`&&`) and OR (`||`) which you
can use to control what command get executed based on their exit status.
- Conditional expressions are always in double brackets (`[[ ]]`). They have
an exit status of 0 if they contain a true assertion or 1 if they contain
a false assertion.
- IF statements evaluate conditional expressions. If an expression is true then
the code within an IF statement is executed, otherwise it is skipped.
- ELIF and ELSE statements also help control the flow of a Bash program, and IF
statements can be nested within other IF statements.

### Exercises

1. Write a Bash script that takes a string as an argument and prints
"how proper" if the string starts with a capital letter.
2. Write a Bash script that takes one argument and prints "even" if the first
argument is an even number or "odd" if the first argument is an odd number.
3. Write a Bash script that takes two arguments. If both arguments are numbers,
print their sum, otherwise just print both arguments.



