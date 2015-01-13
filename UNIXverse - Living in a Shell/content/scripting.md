+++
date = "2015-01-13T14:14:36+11:00"
title = "Shell Scripting"
+++

Shell as an Interpreter
=======================

All of our interactions with the shell so far have been using a _command_ (or
_program_) and passing command line arguments to it. This usually looks
something like:

<pre class='brush: bash;'>
Command Arg1 Arg2 Arg3 < input_file > output_file
</pre>

We extended that functionality using our friendly little pipes, `|`, to direct
one programs output into another's input. With this we were able to perform
some complicated operations. Now we're going to extend our capabilities even
further with **Shell Scripting**.

Shell Scripts
=============

Since this is a (basic) programming language, we'll start with the classic and
over done:

<pre class='brush: bash;'>
#!/bin/sh

echo Hello, World
</pre>

Looking at this script yo ucan see what it will most likely do when run. It
will execute the command `echo` with input text *Hello, World*.

Now you'll probably be wondering what in the world does the first line of
script mean or do? Very basically it tells the thing trying to run the script
which *interpreter* shouldd be used to execute the code. In this case it is
saying "use `sh`", which is one of the main shell interpreters. There's no
need to know much more about it at this point beyond the facts that:

* You should include it as teh first line of every shell script you write.
* Pretty much any command you can run in the command line will work with the
   `sh` interpreter.

Now we have the ability to write some basic shell scripts BUT we need a way to
execute (run) them. Just before the next section create a file called *hello*
and save the Hello, World program into it.

Running Shell Scripts
=====================

So before it was mentioned that `sh` is an interpreter that can read and
execute shell scripts. One way we could run our script then would be to treat
the script just like normal input from a file that we are going to give to a
command, like every other filter we've been using so far. Like so:

<pre class='brush: bash;'>
sh hello
</pre>

This tells `sh` to take in the contents of *hello* and try to execute it. Now
this sounds very similar to what that first line of our script is supposed to
do. If we run our scripts using the above method we don't even need that first
line but there is a better way to execute our scripts!

What we're going to do is turn our standard file that just contains some text
into an executable, like an _.exe_ on windows. To do this we need to change
the _properties_ of the file so that UNIX sees it as an executable file. To do
this we use a program we haven't seen yet called `chmod`. To fully explain
`chmod` and UNIX file permissions would take a while so I'll very quickly give
you the three main permissions a file/folder can hold and how we add the
executable permission to our scripts.

`chmod`
-------

Usage:

<pre class='brush: bash;'>
chmod permission file
</pre>

* The three basic permissions you can give to a file are **read**, **write**,
   and **execute**
* The _permission_ section is where you put what type of permission you want
   to add or take away.

Our usage will pretty much only consist of adding the executable permission to
our scripts. To do this, we use the following command:

<pre class='brush: bash;'>
chmod +x file
</pre>

Inputs and variables
====================

Now we'll look at a script that does something crazy and amazing.

<pre class='brush: bash;'>
#!/bin/sh

echo -n 'Enter your name: '
read name
echo Hello, $name
</pre>

This is where shell scripting starts to look a bit different from normal
commands that you would use in a command prompt. The first `echo` line does as
you would expect, just prints out text (`-n` means it won't print a newline at
the end of the line).

Though the next line has `read` and then `name`, `name` is our very creative
name for a variable.

This is where your prior coding experience should help. You probably know what
a variable is (if you don't, just ask a helper!) and you know that they store
things. So if we want to read in someon's name to our program we need to store
it so we can then use it later, that's waht our variable `name` does. You'll
pick up the syntax for using variables by writing scripts, but the gist is
that when you first put in the variable you just put its name, such as `name`.
From then on, when you want to refer to the value stored in that variable (for
example, in `echo`), you add a `$` before the variable name.

Here's an example script that demonstrates variable usage in Bash:

<div class="alert alert-info">
  Note: spaces matter, do not put spaces around the = symbol or your variables
  won't work.
</div>
<pre class='brush: bash;'>
read x    # read a value into variable x
y=John    # assign a value to the variable y
echo $x   # display the value of variable x
z="$y $x" # assign two copes of y to variable z
echo $z   # display the value of variable z
</pre>

A couple more useful notes on shell variables
---------------------------------------------

* There's no need to declare shell variables. You simply use them.
* All variables have the type `string`
* Note that `x=1` is equivalent to `x="1"`

<pre class='brush: bash;'>
$ x=5
$ y="6"
$ z=abc
$ echo $(( $x + $y ))
11
$ echo $(( $x + $z ))
5
</pre>

More examples:
<pre class='brush: bash;'>
$ x=1
$ y=fred
$ echo $x$y
1fred
$ echo $xy        # the aim is to display "1y"

$ echo "$x"y
1y
$ echo ${x}y
1y
$ echo ${j-10}    # give value of j or 10 if no value
10
$ echo ${j=33}    # set j to 33 if no value (and give $j)
33
$ echo ${x:?No Value}   # display "No Value" if $x not set
1
$ echo ${xx:?No Value}  # display "No Value" if $xx not set
-bash: xx: No Value
</pre>

Built-in shell variables
------------------------

* `$0` - The name of the thing being run (command / script)
* `$1` - The first command-line argument
* `$2` - The second command-line argument
* `$3` - The third command-line argument
* `$#` - Count of command-line arguments
* `$*` - All of the command-line arguments (together)
* `$@` - All of the command-line arguments (separately)
* `$?` - Exit status of the most recent command
* `$$` - Process ID of this shell

Quoting in all its varieties
============================

Quoting can be used for three purposes in the shell:

* To group a sequence of words into a single "word" (concatenation)
* To control the kinds of transformations that are performed.
* To capture the output of commands (back-quotes)

The three different types of quotes result in different things happening:

* Single quote - `'` - stops any variables inside the quotes from being treated
   as variables; `$name` would be printed, rather than its value.
* Double quote - `"` - allows variables inside the quotes to be substituted for
   their contents.
* Back quote - <code>\`</code> - will execute the text in the quotes as commands
    and return the output.

Back quotes are very useful and it is good to know how they're treated by the shell:

1. Performs variable-substitution (as for double-quotes)
2. Executes the resulting command and arguments
3. Captures the standard output from the command.
4. Converts it to a single string
5. Uses this string as the value of the expression.

`expr`
======

This is just a quick little bit on a very useful command called `expr`. `expr`
allows use to perform mathematical operations in the shell and then return the
result as a string.

Using our handy back quotes we just learned about above, we can assign a
variable to be some value defined by a mathematical operation of two other
variables. Like so:

<pre class='brush: bash;'>
$ a=3
$ b=30
$ sum=`expr $a + $b`
$ echo $sum
33
</pre>

`if`, `while`, `for`
====================

`test`
------

Before we can go into flow control, which involves conditions and tests to
decide **how** to control the flow of the program we must tackle one feature
of shell, the `test` command.

The `test` command performs a test or combination of tests and:

* Returns a zero exit status if the test succeeds
* Returns a non-zero exit status if the test fails

`test` has a variety of useful testing options:

* String comparison - `=`, `!=`
* Numeric comparison - `-eq`, `-ne`, `-lt`
* Checks on files - `-f`, `-x`, `-r`
* Boolean operators - `-a`, `-o`, `!`

Here are some examples of how `test` can be used:

<pre class='brush: bash;'>
test "$msg" = "Hello"
    # does the variable msg have the value "Hello"?

test "$x" -gt "$y"
    # does x contain a numeric value larger than y?

msg="hello there"
test $msg = Hello
    # Error: expands to "test hello there = Hello"?

test "$x" -ge 10 -a "$x" -le 20
    # is the value of x in range 10..20?

test -r xyz -a -d xyz
    # is the file xyz a readable directory?
[ -r xyz -a -d xyz ]
    # alternative syntax; requires closing ]
</pre>

`if`
----

Now, onto some flow control! Most languages have some form of flow control, so
hopefully you're familiar with _if statements_ and _looping_. `if`s are formed
by a test/condition and some code to execute based on the result of the test.
Loops are a test (like the test in an `if` statement), but the condition
decides when to enter and exit the loop, so the code can be executed 0 times,
or infinitely many times, or anywhere in between.

The easiest way to get you used to loops and ifs is to have a look at examples
and how their syntax works.

<pre class='brush: bash;'>
if tests
then
  commands
elif tests
then
  commands
else
  commands
fi
</pre>

Important things to note:

* `test` is where you put your conditions
* Make sure each keyword (`then`, `if`, etc.) is on a new line.
* `elif` is just this language's `else if`, `elsif`, etc.
* `fi` this ends the `if` statement, like a closing `}` in C or going back one
   level of indentation in Python.

`while` and `for`
-----------------

Again, here's the basic syntax for `while` and `for` loops:

### `while`

<pre class='brush: bash;'>
while tests
do
  commands
done
</pre>

### `for`

<pre class='brush: bash;'>
for var in wordList
do
  commands  # generally involving var
done
</pre>

`for` is the slightly more complicated loop as it loops through a list, each
iteration loading the next item from the list into the variable specified
where `var` is. Examples will reveal how to go about using each type of loop:

Examples
========

<pre class='brush: bash;'>
# Check the system status every ten minutes

while true
do
   uptime
   sleep 600
done
</pre>

<pre class='brush: bash;'>
# Process files in $PWD, asking for confirmation

for file in *
do
   echo -n "Process $file? "
   read answer
   if test "$answer" = "y"
      process &lt; $file &gt;&gt; results
   fi
done
</pre>

<pre class='brush: bash;'>
# Interactively prompt the user to process files

echo -n "Next file: "
while read filename
do
   process &lt; "$filename" &gt;&gt; results
   echo -n "Next file: "
done
</pre>

<pre class='brush: bash;'>
# Compute sum of a list of numbers from command line

sum=0
for n in "$@"    # remember $@ is all of the command arguments
do
   sum=`expr $sum + "$n"`
done
</pre>

Exercises
=========

Digits
------

Write a script **digits.sh** that reads from standard input and writes to
standard output mapping all digit characters whose values are less than 5 into
the character `<` and all digit characters whose values are larger than 5 into
the character `>`. The digit character '5' should be left unchanged.

<center>
  <table class="table table-bordered table-striped">
    <tbody>
      <tr>
        <th>Sample Input Data</th>
        <th>Corresponding Output</th>
      </tr>
      <tr>
        <td><pre>1 234 5 678 9</pre></td>
        <td><pre>&lt; &lt;&lt;&lt; 5 &gt;&gt;&gt; &gt;</pre></td>
      </tr>
      <tr>
        <td>
          <pre>
I can think of 100's
of other things I'd rather
be doing than these 3 questions</pre>
        </td>
        <td>
          <pre>
I can think of &lt;&lt;&lt;'s
of other things I'd rather
be doing than these &lt; questions</pre>
        </td>
      </tr>
      <tr>
        <td>
          <pre>
A line with lots of numbers:
123456789123456789123456789
A line with all zeroes
000000000000000000000000000
A line with blanks at the end
1 2 3</pre>
        </td>
        <td>
          <pre>
A line with lots of numbers:
&lt;&lt;&lt;&lt;5&gt;&gt;&gt;&gt;&lt;&lt;&lt;&lt;5&gt;&gt;&gt;&gt;&lt;&lt;&lt;&lt;5&gt;&gt;&gt;&gt;
A line with all zeroes
&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;
A line with blanks at the end
&lt; &lt; &lt;</pre>
        </td>
      </tr>
      <tr>
        <td>
          <pre>
Input with absolutely 0 digits in it
Well ... apart from that one ...</pre>
        </td>
        <td>
          <pre>
Input with absolutely &lt; digits in it
Well ... apart from that one ...</pre>
       </td>
     </tr>
     <tr>
       <td>
         <pre>1 2 4 8 16 32 64 128 256 512 1024 2048 4096 8192 16384 32768 65536</pre>
       </td>
       <td>
         <pre>
&lt; &lt; &lt; &gt; &lt;&gt; &lt;&lt; &gt;&lt; &lt;&lt;&gt; &lt;5&gt; 5&lt;&lt; &lt;&lt;&lt;&lt; &lt;&lt;&lt;&gt; &lt;&lt;&gt;&gt; &gt;&lt;&gt;&lt; &lt;&gt;&lt;&gt;&lt; &lt;&lt;&gt;&gt;&gt; &gt;55&lt;&gt;</pre>
       </td>
     </tr>
   </tbody>
 </table>
</center>

echon
-----

Write a shell script a program echon.sh which given exactly two arguments, an
integer _n_ and a string, prints the string _n_ times. For example:

<pre class='brush: bash;'>
% ./echon.sh 5 hello
hello
hello
hello
hello
hello
% ./echon.sh 0 nothing
% ./echon.sh 1 goodbye
goodbye
%
</pre>

Your script should print an error message if it is not given exactly 2
arguments. For example:

<pre class='brush: bash;'>
% ./echon.sh 
Usage: ./echon.sh
% ./echon.sh 1 2 3
Usage: ./echon.sh
</pre>

Also get your script to print an error message if its first argument isn't a
non-negative integer. For example:

<pre class='brush: bash;'>
% ./echon.sh hello world
./ex2.sh: argument 1 must be a non-negative integer
% ./echon.sh -42 lines
./echon.sh: argument 1 must be a non-negative integer
</pre>

<div class="alert alert-info">
  Hint: you'll need to use the shell if, while and exit statements, shell
  arithmetic and the test command.
</div>

File sizes
----------

Write a shell script file_sizes.sh which prints the names of the files in the
current directory, splitting them into three categories: _small_,
_medium-sized_ and _large_. A file is considered _small_ if it contains less
than 10 lines, _medium-sized_ if contains less than 100 lines, otherwise it is
considered _large_.

Your script should always print exactly three lines of output. Files should be
listed in alphabetic order on each line. Your shell-script should match
character-for-character the output shown in the example below. Notice the
creation of a separate directory for testing and the use of the script from the
last question to produce test files. You could also produce test files manually
using an editor.

<pre class='brush: bash;'>
% mkdir test
% cd test
% ../echon.sh 5 text &gt;a
% ../echon.sh 505 text &gt;bbb
% ../echon.sh 17 text &gt;cc
% ../echon.sh 10 text &gt;d
% ../echon.sh 1000 text &gt;e
% ../echon.sh 0 text &gt;empty
% ls -l
total 24
-rw-r--r-- 1 andrewt andrewt   25 Mar 24 10:37 a
-rw-r--r-- 1 andrewt andrewt 2525 Mar 24 10:37 bbb
-rw-r--r-- 1 andrewt andrewt   85 Mar 24 10:37 cc
-rw-r--r-- 1 andrewt andrewt   50 Mar 24 10:37 d
-rw-r--r-- 1 andrewt andrewt 5000 Mar 24 10:37 e
-rw-r--r-- 1 andrewt andrewt    0 Mar 24 10:37 empty
% ../file_sizes.sh 
Small files: a empty
Medium-sized files: cc d
Large files: bbb e
% rm cc d
% ../echon.sh 10000 . &gt;lots_of_dots
% ls -l
total 36
-rw-r--r-- 1 andrewt andrewt    25 Mar 24 10:37 a
-rw-r--r-- 1 andrewt andrewt  2525 Mar 24 10:37 bbb
-rw-r--r-- 1 andrewt andrewt  5000 Mar 24 10:37 e
-rw-r--r-- 1 andrewt andrewt     0 Mar 24 10:37 empty
-rw-r--r-- 1 andrewt andrewt 20000 Mar 24 10:39 lots_of_dots
% ../file_sizes.sh 
Small files: a empty
Medium-sized files:
Large files: bbb e lots_of_dots
%
</pre>

<div class="alert alert-info">
  Hint: you can use the command `wc` to discover how many lines are in a file.
  You probably want to use the shell's back quotes, its `if` statement, and
  the test command.
</div>

Challenge question
------------------

Write a shell script courses.sh which prints a list of UNSW courses with the
given prefix by extracting them from the UNSW handbook pages. For example:

<pre class='brush: bash;'>
% courses.sh JAPN
JAPN4500 Japanese Studies Honours (Research)
JAPN4550 Combined  Japanese Studies Honours (Research)
JAPN5000 Special Project
JAPN5011 Japanese Teaching Practicum
JAPN5100 Business Japanese A
JAPN5101 Business Japanese B
JAPN5102 Professional Japanese A
JAPN5103 Professional Japanese B
% courses.sh MATH | wc
    124     566    4751
% courses.sh COMP | grep Soft
COMP2041 Software Construction: Techniques and Tools
COMP3111 Software Engineering
COMP3141 Software System Design and Implementation
COMP3431 Robotic Software Architecture
COMP3711 Software Project Management
COMP4001 Object-Oriented Software Development
COMP4161 Advanced Topics in Software Verification
COMP4181 Language-based Software Safety
COMP9041 Software Construction: Techniques and Tools
COMP9116 Software System Development Using the B-Method and B-Toolkit
COMP9117 Architecture of Software Systems
COMP9181 Language-based Software Safety
COMP9431 Robotic Software Architecture
</pre>

Your script must download the handbook web pages and extract the information
from them when it is run.

<div class="alert alert-info">
  This task can be done using the usual tools of grep, sed, sort, and uniq. The
  regular expressions take some thought.
</div>

The UNSW handbook uses separate web pages for undergraduate and postgraduate
courses. These two web pages would need to be downloaded for the above example (JAPN):

http://www.handbook.unsw.edu.au/vbook2011/brCoursesByAtoZ.jsp?StudyLevel=Undergraduate&descr=J
and
http://www.handbook.unsw.edu.au/vbook2012/brCoursesByAtoZ.jsp?StudyLevel=Postgraduate&descr=J

The command `wget` can be used to download a web page and has an option which
allows it to be used in shell pipelines. For example:

<pre class='brush: bash;'>
% wget -q -O- "http://www.handbook.unsw.edu.au/vbook2012/brCoursesByAtoZ.jsp?StudyLevel=Undergraduate&amp;descr=J" | wc
    213     982   14222
</pre>

jpg2png
-------

Write a shell script jpg2png.sh which converts all images in JPEG format in the
current directory to PNG format.

You can assume that JPEG files and only JPEG files have the suffix `.jpg`.

If the conversion is successful the JPEG file should be removed.

Your script should stop with an appropriate error message and exit status if
the PNG file already exists.

<pre class='brush: bash;'>
% wget http://www.cse.unsw.edu.au/~cs2041/lab/sh/images/images.zip
% unzip images.zip
Archive:  images.zip
  inflating: Johannes Vermeer - The Girl With The Pearl Earring.jpg  
  inflating: nautilus.jpg            
  inflating: panic.jpg               
  inflating: penguins.jpg            
  inflating: shell.jpg               
  inflating: stingray.jpg            
  inflating: treefrog.jpg            
% ./jpg2png.sh
% ls
Johannes Vermeer - The Girl With The Pearl Earring.png  panic.png
email_image.sh                                          penguins.png
images.zip                                              shell.png
index.php                                               stingray.png
jpg2png.sh                                              treefrog.png
nautilus.png
% cp -p /home/cs2041/public_html/lab/sh/images/penguins.jpg
% ./jpg2png.sh
penguins.png already exists
</pre>

<pre class='brush: bash;'>
% wget -q -O- "http://www.handbook.unsw.edu.au/vbook2012/brCoursesByAtoZ.jsp?StudyLevel=Undergraduate&amp;descr=J" | wc
    213     982   14222
</pre>

<div class="alert alert-info">
  You may find `sed` and back quotes useful here.
</div>

The tool `convert` will convert between many different image formats. Example:

    % convert penguins.jpg penguins.png

Email image
-----------

Write a shell script email_image.sh which given a list of image files as
arguments displays them one-by-one. After the user has viewed each image the
script should prompt the user for an e-mail address. If the user does enter an
email address, the script should prompt the user for a message to accompany the
image and then send the image to that e-mail address.

<pre class='brush: bash;'>
% ./email_image.sh penguins.png treefrog.png 
Address to e-mail this image to? andrewt@cse.unsw.edu.au
Message to accompany image? Penguins are cool.
penguins.png sent to andrewt@cse.unsw.edu.au
Address to e-mail this image to? andrewt@cse.unsw.edu.au
Message to accompany image? This is a White-lipped Tree Frog
treefrog.png sent to andrewt@cse.unsw.edu.au
</pre>

<div class="alert alert-info">
  The program `display` can be used to view image files
</div>

The program `mutt` can be used to send mail from the command line including
attachments, for example:

<pre class='brush: bash;'>
% echo 'Penguins are cool.'|mutt -s 'penguins!' -a penguins.png -- nobody@nowhere.com
</pre>
