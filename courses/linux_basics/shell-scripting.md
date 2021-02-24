# SHELL SCRIPTING FUNDAMENTALS 

In this course will cover the fundamentals of shell scripting. You can use this knowledge to write bash scripts on your own to automate or schedule tasks that are often required for SRE practices.

### ***Lab Environment Setup***:

For practicing bash scripting given in this you can either use the "Terminal" application directly if you are using Linux or MacOS. 
*If you feel you need some brushing up on "vim" editor to be able to create/edit script files on linux, follow the [vim tutorial here](https://github.com/BINPIPE/learning-resources/blob/master/learning-notes/vim-tutorial.md).*

If using Windows you can use follow the below steps:
- Install docker on your system - https://docs.docker.com/engine/install/
(Note: We will be running all the commands on Red Hat Enterprise Linux (RHEL) 8 system.)
- Launch the Docker container by running the below command:

```
docker run -it --name test registry.access.redhat.com/ubi8/ubi:8.1 bash
```

## **What is a Shell Script?**

A Shell Script is a text file that contains a series of linux commands. These commands are a mix of commands that we type on the command line ourselves at normal times (for example the `mv` or `ls` or `ping`) and one liner command snippets with pipe signs `|` and variables (we will learn about these in a while). 

However, please remember that whatsoever you can run on the command line can be put into a script and when executed the outcome will exactly be the same. And vice-versa anything you put into a script can also be run one by one in the same sequence in a terminal to yield the same results.

In this sense, if you know how to do things at the command line then you already a little bit about shell scripting.

For shell scripts the convention is to give files an extension of .sh (`testscript.sh` for example).


## **How does a Shell Script work?**

In the Linux domain there are programs and processes. In layman terms, a program can be understood as a blob of binary data consisting of a series of instructions for the CPU, that is organised into a package and typically stored on your hard disk. 

When we say we are running a program we aren't really running the program but a replica of it which is named a process. What we do is copy those instructions and resources from the hard disk into memory (or RAM). We also allocate some space in RAM for the method to store variables (to hold temporary working data) and a couple of flags to permit the OS (Operating System) to manage and track the method during its execution.

There might be several processes representing an equivalent program running in memory at an equivalent time. For instance I could have two terminals open and be running the command cp in both of them. During this case there would be two cp processes currently existing on the system. Once they're finished running the system then destroys them and there are not any longer any processes representing the program cp.


> In computing, a shell is a computer program which exposes an operating
> system's services to a human user or other program. In general,
> operating system shells use either a command-line interface or
> graphical user interface, depending on a computer's role and
> particular operation.
> 
> [Bash](https://en.wikipedia.org/wiki/Bash_%28Unix_shell%29) is a Unix shell and command language written by Brian Fox for
> the GNU Project as a free software replacement for the Bourne shell.
> First released in 1989, it has been used as the default login shell
> for most Linux distributions.


When we are at the terminal we've a Bash process running so as to offer us the Bash shell. If we start a script running it doesn't actually run therein process but instead starts a replacement process to run inside. We'll demonstrate this within the next section on variables and it's implications should become clearer. However, for most cases you will not have to bother about this.

## **How do we run Shell Scripts?**

"Running" a Bash script is fairly easy. Another term you may come across is "executing" the script (which means the same thing). Before we can execute a script it must have the execute permission set (for safety reasons this permission is generally not set by default). 

If you forget to grant this permission before running the script you'll just get an error message telling you as such and no harm will be done. To set the permission and make the script executable, you can run the command - 

`chmod +x testscript.sh`

Here are the contents of `testscript.sh`
`cat testscript.sh`
```
#!/bin/bash
# A sample Bash script, by Prasanjit Singh
echo Hello World!
```

*Let's break it down:
Line 1 - Is what's referred to as the "shebang". See below for what this is.
Line 2 - This is a comment. Anything after # is not executed. It is for our reference only.
Line 3 - Is the command echo which will print a message to the screen. You can type this command yourself on the command line and it will behave exactly the same.*

The syntax highlighting is there only to make it easier to read and is not something you need to do in your own files (remember they are just plain text files).


Why the **./**
You've possibly noticed that when we run a normal command (such as `ls`) we just type its name but when running the script above I put a `./` in front of it. When you just type a name on the command line Bash tries to find it in a series of directories stored in a variable called `$PATH`. We can see the current value of this variable using the command echo (you'll learn more about variables in the next section).

`echo $PATH`

    /home/Prasanjit/bin:/usr/local/bin:/usr/bin:/bin

The directories are separated by " **:** "

Bash only looks in those specific directories and doesn't consider sub directories or your current directory. It will look through those directories in order and execute the first instance of the program or script that it finds.

The `$PATH` variable is an individual user variable so each user on a system may set it to suit themselves.

This is done for a few different reasons:

- It allows us to have several different versions of a program installed. We can control which one gets executed based on where it sits in our `$PATH`.
- It allows for convenience. As you saw above, the first directory for myself is a bin directory in my home directory. This allows us to put our own scripts and programs there and then we can use them no matter where we are in the system by just typing their name. 
- We could even create a script with the same name as a program (to act as a wrapper) if we wanted slightly different behaviour.
It increases safety - For example a malicious user could create a script called `ls` which actually deletes everything in your home directory. You wouldn't want to inadvertently run that script. But as long as it's not in your `$PATH` that won't happen. Thank God :)

If a program or script is not in one of the directories in your `$PATH` then you can run it by telling Bash where it should look to find it. You do so by including either an absolute or relative path in front of the program or script name. You'll remember that dot ( **.** ) is actually a reference to your current directory. Assuming this script is in my home directory I could also have run it by using an absolute path.

`/home/Prasanjit/testscript.sh`
Hello World!

The Shebang (**#!**)
*#!/bin/bash*

This is the first line of the script above. The hash exclamation mark ( **#!** ) character sequence is referred to as the *Shebang*. Following it is the path to the interpreter (or program) that should be used to run (or interpret) the rest of the lines in the text file. (For Bash scripts it will be the path to Bash, but there are many other types of scripts and they each have their own interpreter.)

Formatting is important here. The shebang must be on the very first line of the file (line 2 won't do, even if the first line is blank). There must also be no spaces before the # or between the ! and the path to the interpreter.

Whilst you could use a relative path for the interpreter, most of the time you are going to want to use an absolute path. You will probably be running the script from a variety of locations so absolute is the safest (and often shorter than a relative path too in this particular case).

It is possible to leave out the line with the shebang and still run the script but it is unwise. If you are at a terminal and running the Bash shell and you execute a script without a shebang then Bash will assume it is a Bash script. So this will only work assuming the user running the script is running it in a Bash shell and there are a variety of reasons why this may not be the case, which is dangerous.

You can also run Bash, passing the script as an argument.

    bash testscript.sh

Hello World!

Whilst this is safe it also involves unnecessary typing every time you want to run the script.


## **Shell Scripting Essentials**

### VARIABLES

*A variable is a temporary store for a piece of information*. There are two actions we may perform for variables:

 *1. Setting a value for a variable.
 2. Reading the value for a variable.*

Variables may have their value set in a few different ways. The most common are to set the value directly and for its value to be set as the result of processing by a command or program. You will see examples of both below.

To read the variable we then place its name (preceded by a **$** sign) anywhere in the script we would like. Before Bash interprets (or runs) every line of our script it first checks to see if any variable names are present. For every variable it has identified, it replaces the variable name with its value. Then it runs that line of code and begins the process again on the next line.

Here are a few quick points on syntax. They will be elaborated on and demonstrated as we go into more detail below.

- When referring to or reading a variable we place a $ sign before the variable name.
- When setting a variable we leave out the $ sign.
- Some people like to always write variable names in uppercase so they stand out. It's your preference however. They can be all uppercase, all lowercase, or a mixture.
- A variable may be placed anywhere in a script (or on the command line for that matter) and, when run, Bash will replace it with the value of the variable. This is made possible as the substitution is done before the command is run.

**Command line arguments**
Command line arguments are commonly used and easy to work with so they are a good place to start.
When we run a program on the command line you would be familiar with supplying arguments after it to control its behaviour. For instance we could run the command 
`ls -l /etc`. 
**-l** and **/etc** are both command line arguments to the command ls. We can do similar with our bash scripts. To do this we use the variables $1 to represent the first command line argument, $2 to represent the second command line argument and so on. These are automatically set by the system when we run our script so all we need to do is refer to them.

Let's look at an example:

`mycopy.sh`

```
#!/bin/bash
# A simple copy script
cp $1 $2
# Let's verify the copy worked
echo Details for $2
ls -lh $2
```

Let's break it down:
Line 4 - run the command cp with the first command line argument as the source and the second command line argument as the destination.
Line 8 - run the command echo to print a message.
Line 9 - After the copy has completed, run the command ls for the destination just to verify it worked. We have included the options l to show us extra information and h to make the size human readable so we may verify it copied correctly.

`./mycopy.sh /projects/file1.data ./results.data`

Details for `./results.data`
```-rw-r--r-- 18 Prasanjit users 3.4M Sep 27 09:25 results.data```

We'll discuss their usage a little more in the next section (Input).

**Other Special Variables**
There are a few other variables that the system sets for you to use as well.
```
*$0 - The name of the Bash script.
$1 - $9 - The first 9 arguments to the Bash script. (As mentioned above.)
$# - How many arguments were passed to the Bash script.
$@ - All the arguments supplied to the Bash script.
$? - The exit status of the most recently run process.
$$ - The process ID of the current script.
$USER - The username of the user running the script.
$HOSTNAME - The hostname of the machine the script is running on.
$SECONDS - The number of seconds since the script was started.
$RANDOM - Returns a different random number each time is it referred to.
$LINENO - Returns the current line number in the Bash script.*
```
**Setting Our Own Variables**
As well as variables that are preset by the system, we may also set our own variables. This can be useful for keeping track of results of commands and being able to refer to and process them later.

There are a few ways in which variables may be set (such as part of the execution of a command) but the basic form follows this pattern:

`variable=value`

This is one of those areas where formatting is important. Note there is no space on either side of the equals ` = ` sign. We also leave off the `$` sign from the beginning of the variable name when setting it.

Variable names may be uppercase or lowercase or a mixture of both but Bash is a case sensitive environment so whenever you refer to a variable you must be consistent in your use of uppercase and lowercase letters. You should always make sure variable names are descriptive. This makes their purpose easier for you to remember.

Here is a simple example to illustrate their usage.

`simplevariables.sh`
```
#!/bin/bash
# A simple variable example
variable1=Hello
variable2=Popeye
echo $variable1 $variable2
echo
sampledir=/etc
ls $sampledir
```

Let's break it down:
Lines 4 and 6 - set the value of the two variables variable1 and variable2.
Line 8 - run the command echo to check the variables have been set as intended.
Line 9 - run the command echo this time with no arguments. This is a good way to get a blank line on the screen to help space things out.
Line 11 - set another variable, this time as the path to a particular directory.
Line 13 - run the command ls substituting the value of the variable `sampledir` as its first command line argument.

`./simplevariables.sh`

*Hello Popeye*
a2ps.cfg aliases alsa.d ...

It is important to note that in the example above we used the command `echo` simply because it is a convenient way to demonstrate that the variables have actually been set. `echo` is not needed to make use of variables and is only used when you wish to print a specific message to the screen. (Pretty much all commands print output to the screen as default so you don't need to put echo in front of them.)

**Quotes**
In the example above we kept things nice and simple. The variables only had to store a single word. When we want variables to store more complex values however, we need to make use of quotes. This is because under normal circumstances Bash uses a space to determine separate items.

`myvariable=Hello World`
*-bash: World: command not found*

Remember, commands work exactly the same on the command line as they do within a script.

When we enclose our content in quotes we are indicating to Bash that the contents should be considered as a single item. You may use single quotes ( **'** ) or double quotes ( **"** ).

*Single quotes will treat every character literally.*
*Double quotes will allow you to do substitution* (that is include variables within the setting of the value).
```
myvariable='Hello World'
echo $myvariable
```
Hello World
```
newvariable="More $myvariable"
echo $newvariable
```
More Hello World
```
newvariable='More $myvariable'
echo $newvariable
More $myvariable
```

**Command Substitution**
Command substitution allows us to take the output of a command or program (what would normally be printed to the screen) and save it as the value of a variable. To do this we place it within brackets, preceded by a **$** sign.

```
myvariable=$( ls /etc | wc -l )
echo There are $myvariable entries in the directory /etc
```
Command substitution is nice and simple if the output of the command is a single word or line. If the output goes over several lines then the newlines are simply removed and all the output ends up on a single line.

```
ls
bin Documents Desktop ...
Downloads public_html ...
myvariable=$( ls )
echo $myvariable
bin Documents Desktop Downloads public_html ...
```
Let's break it down:
Line 1 - We run the command ls. Normally its output would be over several lines. We have shortened it a bit in the example above just to save space.
Line 4 - When we save the command to the variable `myvariable` all the newlines are stripped out and the output is now all on a single line.

**Exporting Variables**
Remember how in the previous segment we talked about scripts being run in their own process? This introduces a phenomenon known as scope which affects variables amongst other things. The idea is that variables are limited to the process they were created in. Normally this isn't an issue but sometimes, for instance, a script may run another script as one of its commands. If we want the variable to be available to the second script then we need to export the variable.

`script1.sh`
```
#!/bin/bash
# demonstrate variable scope 1.
var1=blah
var2=foo
# Let's verify their current value
echo $0 :: var1 : $var1, var2 : $var2
export var1
```
`./script2.sh`
```
# Let's see what they are now
echo $0 :: var1 : $var1, var2 : $var2
```

`script2.sh`
```
#!/bin/bash
# demonstrate variable scope 2
# Let's verify their current value
echo $0 :: var1 : $var1, var2 : $var2
# Let's change their values
var1=flop
var2=bleh
```
Now lets run it and see what happens.

`./script1.sh`
```
script1.sh :: var1 : blah, var2 : foo
script2.sh :: var1 : blah, var2 :
script1.sh :: var1 : blah, var2 : foo
```
The output above may seem unexpected. What actually happens when we export a variable is that we are telling Bash that every time a new process is created (to run another script or such) then make a copy of the variable and hand it over to the new process. So although the variables will have the same name they exist in separate processes and so are unrelated to each other.

Exporting variables is a one way process. The original process may pass variables over to the new process but anything that process does with the copy of the variables has no impact on the original variables.

Exporting variables is something you probably won't need to worry about for most Bash scripts you'll create. Sometimes you may wish to break a particular task down into several separate scripts however to make it easier to manage or to allow for reusability (which is always good). For instance you could create a script which will make a dated (ie todays date prepended to the filename) copy of all filenames exported on a certain variable. Then you could easily call that script from within other scripts you create whenever you would like to take a snapshot of a set of files.


**Ask the User for Input**
If we would like to ask the user for input then we use a command called `read`. This command takes the input and will save it into a variable.

`read var1`

Let's look at a simple example:

`introduction.sh`
```
#!/bin/bash
# Ask the user for their name
echo Hello, who am I talking to?
read varname
echo It\'s nice to meet you $varname
```
Let's break it down:
Line 4 - Print a message asking the user for input.
Line 6 - Run the command read and save the users response into the variable varname
Line 8 - echo another message just to verify the read command worked. Note: We had to put a backslash ( \ ) in front of the ' so that it was escaped.

`./introduction.sh`

Hello, who am I talking to?
*Prasanjit*
It's nice to meet you *Prasanjit*

Note: Prasanjit above is in italics just to show that it was something we typed in. On your terminal input will show up normally.

### LOOPS


**Basic If Statements**
A basic `if` statement effectively says, if a particular test is true, then perform a given set of actions. If it is not true then don't perform those actions. If follows the format below:
```
if [ <some test> ]
then
<commands>
fi
```
Anything between then and fi (if backwards) will be executed only if the test (between the square brackets) is true.

Let's look at a simple example:

`if_example.sh`

```
#!/bin/bash
# Basic if statement
if [ $1 -gt 100 ]
then
echo Hey that\'s a large number.
pwd
fi
date
```

Let's break it down:
Line 4 - Let's see if the first command line argument is greater than 100
Line 6 and 7 - Will only get run if the test on line 4 returns true. You can have as many commands here as you like.
Line 6 - The backslash ( \ ) in front of the single quote ( ' ) is needed as the single quote has a special meaning for bash and we don't want that special meaning. The backslash escapes the special meaning to make it a normal plain single quote again.
Line 8 - fi signals the end of the if statement. All commands after this will be run as normal.
Line 10 - Because this command is outside the if statement it will be run regardless of the outcome of the if statement.

`./if_example.sh 15`
`Tue 19 Jan 6:51:31 2021`

./if_example.sh 150
`Hey that's a large number.`
/home/Prasanjit/bin
`Tue 19 Jan 6:51:31 2021`


**Nested If statements**
Talking of *indenting*. Here's a perfect example of when it makes life easier for you. You may have as many if statements as necessary inside your script. It is also possible to have an if statement inside of another if statement. 
For example, we may want to analyse a number given on the command line like so:

`nested_if.sh`
```
#!/bin/bash
# Nested if statements
if [ $1 -gt 100 ]
then
echo Hey that\'s a large number.
if (( $1 % 2 == 0 ))
then
echo And is also an even number.
fi
fi
```
Let's break it down:
Line 4 - Perform the following, only if the first command line argument is greater than 100.
Line 8 - This is a light variation on the if statement. If we would like to check an expression then we may use the double brackets just like we did for variables.
Line 10 - Only gets run if both if statements are true.


**If Else**
Sometimes we want to perform a certain set of actions if a statement is true, and another set of actions if it is false. We can accommodate this with the else mechanism.

```
if [ <some test> ]
then
<commands>
else
<other commands>
fi
```

Now we could easily read from a file if it is supplied as a command line argument, else read from STDIN.

`else.sh`
```
#!/bin/bash
# else example
if [ $# -eq 1 ]
then
nl $1
else
nl /dev/stdin
fi
```

**Boolean Operations**
Sometimes we only want to do something if multiple conditions are met. Other times we would like to perform the action if one of several condition is met. We can accommodate these with boolean operators.

`and - &&`
`or - ||`
For instance maybe we only want to perform an operation if the file is readable and has a size greater than zero.

`and.sh`
```
#!/bin/bash
# and example
if [ -r $1 ] && [ -s $1 ]
then
echo This file is useful.
fi
```
Maybe we would like to perform something slightly different if the user is either Julia or Merida.

`or.sh`
```
#!/bin/bash
# or example
if [ $USER == 'Julia' ] || [ $USER == 'Merida' ]
then
ls -alh
else
ls
fi
```

**While Loops**
One of the easiest loops to work with is while loops. They say, while an expression is true, keep executing these lines of code. They have the following format:

```
while [ <some test> ]
do
<commands>
done
```

In the example below we will print the numbers 1 through to 10:

`while_loop.sh`
```
#!/bin/bash
# Basic while loop
counter=1
while [ $counter -le 10 ]
do
echo $counter
((counter++))
done
echo All done
```


**Until Loops**
The until loop is fairly similar to the while loop. The difference is that it will execute the commands within it until the test becomes true.

```
until [ <some test> ]
do
<commands>
done
```

`until_loop.sh`
```
#!/bin/bash
# Basic until loop
counter=1
until [ $counter -gt 10 ]
do
echo $counter
((counter++))
done
echo All done
```

**For Loops**
The for loop is a little bit different to the previous two loops. What it does is say for each of the items in a given list, perform the given set of commands. It has the following syntax.

```
for var in <list>
do
<commands>
done
```
The for loop will take each item in the list (in order, one after the other), assign that item as the value of the variable var, execute the commands between do and done then go back to the top, grab the next item in the list and repeat over.

*The list is defined as a series of strings, separated by spaces*.

Here is a simple example to illustrate:

`for_loop.sh`
```
#!/bin/bash
# Basic for loop
names='Julia Elsa Anna'
for name in $names
do
echo $name
done
echo All done
```
Ranges
We can also process a series of numbers

`for_loop_series.sh`

```
#!/bin/bash
# Basic range in for loop
for value in {1..5}
do
echo $value
done
echo All done
```

It is also possible to specify a value to increase or decrease by each time. You do this by adding another two dots ( .. ) and the value to step by.

`for_loop_stepping.sh`
```
#!/bin/bash
# Basic range with steps for loop
for value in {10..0..2}
do
echo $value
done
echo All done
```

One of the more useful applications of for loops is in the processing of a set of files. To do this we may use wildcards. Let's say we want to convert a series of .html files over to .php files.

`convert_html_to_php.sh`

```
#!/bin/bash
# Make a php copy of any html files
for value in $1/*.html
do
cp $value $1/$( basename -s .html $value ).php
done
```


**Controlling Loops: Break and Continue**
Most of the time your loops are going to through in a smooth and ordely manner. Sometimes however we may need to intervene and alter their running slightly. There are two statements we may issue to do this.

Break
The break statement tells Bash to leave the loop straight away. It may be that there is a normal situation that should cause the loop to end but there are also exceptional situations in which it should end as well. For instance, maybe we are copying files but if the free disk space get's below a certain level we should stop copying.

`copy_files.sh`
```
#!/bin/bash
# Make a backup set of files
for value in $1/*
do
used=$( df $1 | tail -1 | awk '{ print $5 }' | sed 's/%//' )
if [ $used -gt 90 ]
then
echo Low disk space 1>&2
break
fi
cp $value $1/backup/
done
```


### FUNCTIONS

Think of a function as a small script within a script. It's a small chunk of code which you may call multiple times within your script. They are particularly useful if you have certain tasks which need to be performed several times. Instead of writing out the same code over and over you may write it once in a function then call that function every time.

**Functions**
Creating a function is fairly easy. They may be written in two different formats:

```
function_name () {
<commands>
}
```
or
```
function function_name {
<commands>
}
```


Let's look at a simple example:

`function_example.sh`
```
#!/bin/bash
# Basic function
print_something () {
echo Hello I am a function
}
print_something
print_something
```

**Passing Arguments**
It is often the case that we would like the function to process some data for us. We may send data to the function in a similar way to passing command line arguments to a script. We supply the arguments directly after the function name. Within the function they are accessible as $1, $2, etc.

`arguments_example.sh`

```
#!/bin/bash
# Passing arguments to a function
print_something () {
echo Hello $1
}
print_something Saturn
print_something Uranus
```
`./arguments_example.sh`

Hello Saturn
Hello Uranus


**Return Values**
Most other programming languages have the concept of a return value for functions, a means for the function to send data back to the original calling location. Bash functions don't allow us to do this. They do however allow us to set a return status. Similar to how a program or command exits with an exit status which indicates whether it succeeded or not. We use the keyword return to indicate a return status.

`return_status_example.sh`

```
#!/bin/bash
# Setting a return status for a function
print_something () {
echo Hello $1
return 5
}
print_something Saturn
print_something Uranus
echo The previous function has a return value of $?
```


Example Scripts:
- https://github.com/BINPIPE/devops-scripts

Further Reading:
- https://www.linuxtopia.org/online_books/advanced_bash_scripting_guide/
- https://s905060.gitbooks.io/site-reliability-engineer-handbook/content/

