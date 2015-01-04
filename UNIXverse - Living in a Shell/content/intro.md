+++
date = "2015-01-04T15:59:56+11:00"
title = "Introduction"
+++

Introduction
============

Hello and welcome to the Beginner's Guide of the UNIXverse! Today we will cover
useful features available in shell - through the website content, talks and
exercises (and hopefully, by the time we finished this, [solutions](/UNIXverse/solutions).

But first of all, what is shell? Many of you may think that the terminal you
use (xterm, terminator, etc.) is a shell - you are partially right - it is an
interactive shell. A shell is basically an interface that allows you to
interact with a system's interface. The reason terminals you are used to are
interactive is because you get to...well...interact with it (you give it input,
they give output/errors, you give it more inputs [or rage quit].

One important thing to note about shells is...well, they are a "shell". All
commands within a shell are self-contained - this will be an important concept
in [shell scripting](/UNIXverse/scripting).

Cryptic Symbols
===============

When we are interacting with the terminal (or you are viewing commands online),
you commonly see symbols like <code>$</code>, <code>~</code> and <code>%</code>
(and most probably a bunch of text) before commands. This is merely to indicate
that the command is entered by the user, and is not the output of a program.
Below is a demonstration of what you may see.

<pre class='brush: bash;'>
# this is a comment
$ echo "you may see it like this"
you make see it like this
Oliver $ echo "or like this, with Oliver in front"
or like this, with Oliver in front
Oliver % echo "or this"
or this
</pre>

<div class='alert alert-info'>
A common newbie mistake when using shell is to include the
preceding text before commands when copying something from
the shell. Please don't do this, as it only leads to
confusing errors and CHAOS! Like that '$' just after
Oliver, that's not a part of the echo command, so it
shouldn't be copied.
</div>

Basic Commands
==============

These are some of the most basic and core commands available in basically every
variation of Linux/Unix ever.

<table class="table table-hover">
  <th>Command</th>
  <th>Description</th>

  <tr>
    <td>
      <code>cd</code><br />
      Stands for: Change Directory
    </td>
    <td>
      A directory is the formal name for a folder. The first thing that you
      need to know about <code>cd</code> is that UNIX sorts directories in a
      hierarchy, which means that all files and directories have a 'parent'
      directory (the directory above the current one), and all directories can
      have 'children', which are either files or directories.

      <code>cd</code> is the tool used to change from one directory to another.
      Think about this like the folders in Windows Explorer - double-clicking
      on a folder opens it and shows its contents (its children).

      Examples:
      <ul>
        <li>
          <code>cd ~</code> - takes you to your home directory
        </li>
        <li>
          <code>cd folder</code> - if there is a directory called "folder" in
          the current directory, moves into that directory.
        </li>
        <li>
          <code>cd ..</code> - goes into the parent folder.
        </li>
        <li>
          <code>cd /</code>, which goes to the root directory. The root
          directory is the only directory that doesn't have a parent. Every
          directory and file in your file system is therefore a child of the
          root directory, or a grandchild, or ...
        </li>
      </ul>
    </td>
  </tr>

  <tr>
    <td>
      <code>mkdir</code><br />
      Stands for: Make Directory
    </td>
    <td>
      Makes a new directory as a child of the current directory.<br />

      e.g. <code>mkdir beans</code><br />
    </td>
  </tr>

  <tr>
    <td>
      <code>ls</code><br />
      Stands for: List
    </td>
    <td>
      Lists all files in the current directory. All of these commands have
      options. While <code>ls</code> provides just file and directory names,
      <code>ls -la</code> produces a vertical list with the details of all
      files, including hidden files. This can be incredibly useful!
    </td>
  </tr>

  <tr>
    <td>
      <code>rm</code><br />
      Stands for: Remove
    </td>
    <td>
      Deletes files or directories. May require additional options to delete
      folders and protected files.<br />

      <code>rm -r</code> <em>recursively</em> deletes a folder, so it deletes
      all contents of the folder first, and then the folder itself.<br />

      <code>rm -f</code> <em>force</em> the deletion of something.

      <code>rm</code> can take multiple arguments, so you can delete more than
      one thing at a time.<br /> For example: <code>rm file1 file2</code> will
      delete the files named "file1" and "file2".
    </td>
  </tr>

  <tr>
    <td>
      <code>echo</code>
    </td>
    <td>
      <code>echo</code> echoes (repeats) whatever arguments you give it to
      standard output (the screen), followed by a new line.<br /> Use:<br />

      <code>echo "Hello World!"</code>
    </td>
  </tr>
</table>

<div class="alert alert-info">
  <i class="fa fa-lightbulb-o"></i>
  If your arguments contains spaces, or special characters (like
  <code>*</code>, <code>?</code>), you can use quote marks to escape them, e.g.
  <code>cd "lol* special folder"</code> (although avoid using spaces in names).
</div>

Shell Shortcuts
===============

Here are a few (useful) shortcuts in the shell.

Reuse a previous command
------------------------

Hitting the 'up' key will automatically place a previously executed command in
your next command to execute. This works up until you reach the earliest point
of history that the computer records.

Going down, of course, does the opposite if you need it.

Tab Completion
--------------

Essentially completes a command you have half-typed out so you don't have to
waste precious milliseconds writing the rest of it. It is literally magical.

Mess around with the command above by making directories and navigating through
them so you get used to it all. Make a folder with a really long name and then
tab-complete the name when you <code>cd</code> into it. An example might be:

<pre class='brush: bash;'>
mkdir antidisestablishmentarianism
cd anti<tab> <enter>
</pre>

Try it. Your life is now measurably better.

Keyboard Shortcuts
------------------

There are a number of Keyboard Shortcuts that are handy in a terminal,
including:
<table class="table table-hover">
  <th>Shortcut</th>
  <th>Description</th>

  <tr>
    <td>
      <code>Alt+Backspace</code> or <code>CTRL+W</code>
    </td>
    <td>
      Deletes the previous word you are on
    </td>
  </tr>
  <tr>
    <td>
      <code>CTRL+A</code>
    </td>
    <td>
      Go to beginning of line
    </td>
  </tr>
  <tr>
    <td>
      <code>CTRL+E</code>
    </td>
    <td>
      Goes to end of line
    </td>
  </tr>
  <tr>
    <td>
      <code>CTRL+L</code>
    </td>
    <td>
      Clears the screen - similar to <code>clear</code>
    </td>
  </tr>

  <tr>
    <td>
      <code>CTRL+U</code>
    </td>
    <td>
      Clear up anything before the cursor
    </td>
  </tr>

  <tr>
    <td>
      <code>CTRL+K</code>
    </td>
    <td>
      Clears anything after the cursor
    </td>
  </tr>

  <tr>
    <td>
      <code>CTRL+R</code>
    </td>
    <td>
      Type in a command and it'll go through your command history. Sometimes
      this is a better option than holding down the Up key to get to a previous
      instruction.
    </td>
  </tr>

  <tr>
    <td>
      <code>CTRL+RIGHT</code> or <code>ALT+F</code>
    </td>
    <td>
      Move forward one word
    </td>
  </tr>

  <tr>
    <td>
      <code>CTRL+LEFT</code> or <code>ALT+B</code>
    </td>

    <td>
      Move backwards one word
    </td>
  </tr>
</table>

Standard Streams
----------------

This workshop sometimes relies on you knowing what the standard streams are --
standard input (<code>stdin</code>) and standard error (<code>stderr</code>).
They are further described below:

### Standard Input

This is where a program reads information from -- input is provided by the
user, or by [redirection](#redirection). It is also known as stdin. C reads
from stdin using <code>scanf</code>, and Python uses <code>raw_input()</code>.

### Standard Output

This is where data is usually printed out from a program. This information is
usually displayed in your terminal, unless it is redirected. This is also known
as <code>stdout</code>. To print to stdout, C uses <code>printf</code> and
Python uses <code>print</code>.

### Standard Error

This is where errors are usually printed out from a program. It is also known
as <code>stderr</code>. C prints to stderr using <code>fprintf(STDERR,
"");</code>, and Python prints to stderr using <code>sys.stderr.write</code>.

Redirection
-----------

You may sometimes want to change where the standard streams go or come from --
for example, putting all messages printed to stderr in a file.

## &lt;

Redirects the contents of a file into stdin, for a program to read. Example:
<code>./program < information.txt</code> pushes the contents of information.txt
into stdin, <code>program</code> to read.

## &gt;
Redirects stdout from a program to a file. Example:
<code>./program > output.txt</code> puts the output of my program into
output.txt.

You can use <code>>></code> to append to the end of a file, as > rewrites the
entire file.

Note you can also combine redirectors: <code>./program < in.txt >
out.txt</code>.

<div class="alert alert-info">
  <i class="fa fa-lightbulb-o"></i> Note you can use <code>&gt;
  /dev/null</code>, the "data sink", which is the equivalent of throwing output
  down the drain. This is useful for redirecting information that you don't
  want to see without having to put that output into a file.
</div>

## 2&gt;

This does the same redirection, but for standard error. For example, <code>./program
2> errors.txt</code> logs errors from the program into error.txt.

You can redirect multiple output streams at once, but this can be messy and
complicated. See this for more information.

## |

This is a pipe, which takes the standard output of the left hand side of the
pipe and "pipes" that output to standard input of the command on the right hand
side of the pipe. This is very important in the next section, [UNIX
Filters](/UNIXverse/filters), and is the basis of all commands. For example,
<code>./program | ./program2</code> puts all output of program 1 into program
2.

Note all redirectors can be chained, for example, <code>./program < lol.txt |
./program2 | ./program3 > out.txt</code>. A chain of pipes is known as a
pipeline.

Reading Files
=============

cat
---

Takes unlimited arguments and outputs the contents of the files to standard
output. This way, the files are concatenated together! For example, <code>cat
a.txt b.txt</code> prints a.txt, then b.txt to standard output. This command
can be useful for viewing a short file on screen temporarily.

If you do not give it an argument, it'll wait for standard input from the user,
followed by the end of file character (<code>CTRL+D</code>) and prints that out.

more and less
-------------

More and less are both basic text viewers for terminal. Less is better than
more, but both are functional. You can read and navigate a file by entering the
command <code>less a.txt</code>. Less paginates output (displays it one page at
a time), which is a very welcome feature when dealing with long files - you
definitely do not what to simply <code>cat</code> a long file.

Text Editors
------------

There are many available on UNIX (some of which you have to install), including
<code>gedit</code>, <code>emacs</code>, and <code>vim</code>. What you use
today is up to you!

Lesson Complete!
================

Congratulations, you have just completed the basics of shell. Let's move on
while we're on a roll!
