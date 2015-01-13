+++
date = "2015-01-12T14:55:14+11:00"
title = "UNIX Filters"
+++

As the name suggest, UNIX Filters are used to filter things. As with a filter
in the real world, you have something that goes through the filter and comes
out as a different thing.

The idea of a filter is the basis of the UNIX filters that we will now discuss.
Filters are small programs that process data, modifies the data, and outputs
the new information.

So in our case we have:

* Commands and data (input) which we type into the shell,
* The filter of choice, which we send information to via stdin,
* Data (output) which is sent back to us by the filter, via stdout.

![Filters in the shell](http://www.cse.unsw.edu.au/~cs2041/14s2/lec/filters/Pic/old/filter1.png)

Common Filters and Tools
========================

`cat`
----------------

`cat` outputs its input.  If it is given a list of files instead of just one,
`cat` concatenates the files together and outputs the result.

`cat`'s useful command line options include:

* `-n` Adds line numbers to the front of every line, starting from 1
* `-s` Squelches consecutive blank lines into a single blank line.
* `-v` Display special control characters such as new lines and tabs.

<pre class='brush: bash;'>
&gt; cat file1          &gt; cat file2
line4                line0
line5                line1
line6                line2
                     line3

&gt; cat file1 file2    &gt; cat file2 file1
line4                line0
line5                line1
line6                line2
line0                line3
line1                line4
line2                line5
line3                line6
</pre>

`wc`
=====

`wc` stands for "word count". It is a tool to count the number of lines, words,
and letters of input. `wc` is very useful for counting things, when combined
with other filters. The format of its output is on the right. The numbers
represent lines, words and characters respectively, followed by the file name
(if available).

`wc`'s useful command line options include:

* `-c` Display the number of characters only
* `-w` Display the number of words only
* `-l` Display the number of lines only

Format of `wc`'s output:

<pre class='brush: bash;'>
&gt; wc file1           &gt; wc file2
 3  3 18 file1        4  4 24 file2

&gt; wc file1 file2
 3  3 18 file1
 4  4 24 file2
 7  7 42 total
</pre>

`head`/`tail`
=============

`head`/`tail` are filters that output the first/last 10 lines of input,
respectively. The number of lines can be changed with the command line option
below.

Its useful command line options include:

* <code>-<em>x</em></code> where _x_ is a number, head/tail will display the
   first or last _x_ lines.

<pre class="brush:bash">
&gt; head -1 file1      &gt; head -2 file2
line4                line0
                     line1

&gt; tail -2 file1      &gt; tail -1 file2
line5                line3
line6
</pre>

`sort`
======

`sort` sorts its input a particular order. By default, sort sorts
lexicographically on a line-by-line basis. There are also some more advanced
features such as being able to sort on a certain column of input, instead of
the beginning of each line.

Its useful command line options include:

* `-r` Reverse sort (descending order)
* `-n` Sort numerically instead of lexicographically
* `-d` Dictionary sort (ignores non-letters and non-digits)
* `-t'c'` Use the character _c_ as the column separator
* `-k'n'` Sort the input using column _n_

<pre class="brush:bash">
&gt; cat file1
3
2
5
4

&gt; sort file1     &gt; sort -r file1
2                5
3                4
4                3
5                2
</pre>

`uniq`
======

`uniq` removes _adjacent_ copies of identical lines so that there is only one
left. If identical lines are distributed across your input, a combination of
`sort` and `uniq` will need to be used to remove duplicates.

`uniq`'s useful command line options include:

* `-c` Count the number of times each line is repeated.
* `-d` Print one copy of every duplicated line.
* `-u` Print every unique line (not duplicated)

<pre class="brush:bash">
&gt; cat file1
kitten
dog
mouse
mouse

&gt; uniq file1        &gt; uniq -c file1
kitten              1 kitten
dog                 1 dog
mouse               2 mouse

&gt; uniq -d file1     &gt; uniq -u file1
mouse               kitten
                    dog
</pre>

`cut`
=====

`cut` outputs certain parts of the input lines depending on the command line
options specified. It is very useful for manipulating columnated data.

Its useful command line options include:

* `-c(x|x-x|x,x,....,x)` here _x_ can be any integer and it represents a
   character position in a line of input data. This is easiest to understand by
   actually using `cut`.
* `-f(x|x-x|x,x,....,x)` here _x_ is the same as the above, but it represents
   columns in input data which are separated by tabs (unless another column
   delimiter is specified using the following option)
* `-d'c'` Sets the character _c_ as the column separator

<pre class="brush:bash">
&gt; cat file1
hello world

&gt; cut -c2 file1     &gt; cut -c1-5 file1
e                   hello

&gt; cut -c1,3-5,8 file1
hlloo
</pre>

`diff`
======

`diff` compares files against each other and outputs information about the
differences of the files. `diff` specifies if a line appears in file1 but not
in file2 and visa versa. This is very useful for checking the a program's
output against its expected output.

Its useful command line options include:

* `-i`  ignore case
* `-B` ignore blank lines

<pre class="brush:bash">
&gt; cat file1      &gt; cat file2
line0            line0
line1            line2
line2            line1

&gt; diff file1 file2
2d1
&lt; line1
3a3
&gt; line1
</pre>

`tr`
====

`tr`, or transliterate, converts a set of characters to another set of
characters. Usage: `tr 'sourceChars' 'destinationChars'`. The examples will best explain how tr works. tr has some useful shortcuts. For example, 'a-z' is a shorthand for writing 'abcdefghijklmnopqrstuvwxyz'.

Its useful command line options include:

* `-c` Map every character _not_ provided in the source chars
* `-d` Delete every character in the source chars from the input

<pre class="brush:bash">
&gt; cat file1
compclub

&gt; tr 'a-z' 'A-Z' &lt; file1      &gt; cat file1 | tr 'c' 'f'
COMPCLUB                      fompflub
</pre>

Exercises
=========

Sort Practice
-------------

Consider the columnated (space-delimited) data file below containing
information about marks for a single subject.

    2111321 37 FL
    2166258 67 CR
    2168678 84 DN
    2186565 77 DN
    2190546 78 DN
    2210109 50 PS
    2223455 95 HD
    2266365 55 PS

Using `sort` and its command line options output the above data in the
following formats:

1. In order of student number
2. In ascending order of mark
3. In descending order of mark

Some filter questions
---------------------

1. How could you use head and tail in a pipeline to display lines 25 through 75
   of a file?
2. Using the above data set find a filter that will only display the student
   number and their grade, not their raw mark. But the catch is you need to sort
   the data by their raw mark before displaying it.

Here's one last question employing `uinq`.

    2111321 37 FL
    2111321 37 FL
    2166258 67 CR
    2168678 84 DN
    2266365 55 PS
    2186565 77 DN
    2190546 78 DN
    2266365 55 PS
    2210109 50 PS
    2168678 84 DN
    2223455 95 HD
    2266365 55 PS

Here is the marks data from above after Brad Hall tried to edit it. He got very
confused and ended up duplicating a lot of different student data.

1. First try `uniq` on the data and see the results. There are still duplicates.
2. Now figure out ho you can appropriately use filters to remove all of the duplicates.
3. Now give Brad Hall a jelly baby so he doesn't get confused again!

<div class='text-center'>
  <a href='/UNIXverse/scripting' class='btn btn-lg btn-success' type='button'>
    Scripting<i class='fa fa-arrow-right'></i>
  </a>
</div>
