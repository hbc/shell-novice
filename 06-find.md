---
layout: page
title: The Unix Shell
subtitle: Finding Things
minutes: 15
---
> ## Learning Objectives
>
> *   Use `grep` to select lines from text files that match simple patterns.
> *   Explain what is meant by "text" and "binary" files, and why many common tools don't handle the latter well.

You can guess someone's age by how they talk about search:
young people use "Google" as a verb,
while crusty old Unix programmers use "grep".
The word is a contraction of "global/regular expression/print",
a common sequence of operations in early Unix text editors.
It is also the name of a very useful command-line program.

`grep` finds and prints lines in files that match a pattern.
For our examples,
we will use a file that contains three haikus taken from a
1998 competition in *Salon* magazine. For this set of examples 
we're going to be working in the writing subdirectory:

~~~ {.input}
$ cd
$ cd writing
$ cat haiku.txt
~~~
~~~ {.output}
The Tao that is seen
Is not the true Tao, until
You bring fresh toner.

With searching comes loss
and the presence of absence:
"My Thesis" not found.

Yesterday it worked
Today it is not working
Software is like that.
~~~

> ## Forever, or Five Years {.callout}
>
> We haven't linked to the original haikus because they don't appear to be on *Salon*'s site any longer.
> As [Jeff Rothenberg said](http://www.clir.org/pubs/archives/ensuring.pdf),
> "Digital information lasts forever --- or five years, whichever comes first."

Let's find lines that contain the word "not":

~~~ {.input}
$ grep not haiku.txt
~~~
~~~ {.output}
Is not the true Tao, until
"My Thesis" not found
Today it is not working
~~~

Here, `not` is the pattern we're searching for.
It's pretty simple:
every alphanumeric character matches against itself.
After the pattern comes the name or names of the files we're searching in.
The output is the three lines in the file that contain the letters "not".

Let's try a different pattern: "day".

~~~ {.input}
$ grep day haiku.txt
~~~
~~~ {.output}
Yesterday it worked
Today it is not working
~~~

This time,
two lines that include the letters "day" are outputted.
However, these letters are contained within larger words.
To restrict matches to lines containing the word "day" on its own,
we can give `grep` with the `-w` flag.
This will limit matches to word boundaries.

~~~ {.input}
$ grep -w day haiku.txt
~~~

In this case, there aren't any, so `grep`'s output is empty.

Another useful option is `-n`, which numbers the lines that match:

~~~ {.input}
$ grep -n it haiku.txt
~~~
~~~ {.output}
5:With searching comes loss
9:Yesterday it worked
10:Today it is not working
~~~

Here, we can see that lines 5, 9, and 10 contain the letters "it".

We can combine flags as we do with other Unix commands.
For example,
since `-i` makes matching case-insensitive and `-v` inverts the match,
using them both only prints lines that *don't* match the pattern
in any mix of upper and lower case:

~~~ {.input}
$ grep -i -v the haiku.txt
~~~
~~~ {.output}
You bring fresh toner.

With searching comes loss

Yesterday it worked
Today it is not working
Software is like that.
~~~

`grep` has lots of other options.
To find out what they are, we can type `man grep`.
`man` is the Unix "manual" command:
it prints a description of a command and its options,
and (if you're lucky) provides a few examples of how to use it.

To navigate through the `man` pages,
you may use the up and down arrow keys to move line-by-line,
or try the "b" and spacebar keys to skip up and down by full page.
Quit the `man` pages by typing "q".

~~~ {.input}
$ man grep
~~~
~~~ {.output}
GREP(1)                                                                                              GREP(1)

NAME
grep, egrep, fgrep - print lines matching a pattern

SYNOPSIS
grep [OPTIONS] PATTERN [FILE...]
grep [OPTIONS] [-e PATTERN | -f FILE] [FILE...]

DESCRIPTION
grep  searches the named input FILEs (or standard input if no files are named, or if a single hyphen-
minus (-) is given as file name) for lines containing a match to the given PATTERN.  By default, grep
prints the matching lines.
...        ...        ...

OPTIONS
Generic Program Information
--help Print  a  usage  message  briefly summarizing these command-line options and the bug-reporting
address, then exit.

-V, --version
Print the version number of grep to the standard output stream.  This version number should be
included in all bug reports (see below).

Matcher Selection
-E, --extended-regexp
Interpret  PATTERN  as  an  extended regular expression (ERE, see below).  (-E is specified by
POSIX.)

-F, --fixed-strings
Interpret PATTERN as a list of fixed strings, separated by newlines, any of  which  is  to  be
matched.  (-F is specified by POSIX.)
...        ...        ...
~~~

> ## Wildcards {.callout}
>
> `grep`'s real power doesn't come from its options, though; it comes from
> the fact that patterns can include wildcards. (The technical name for
> these is **regular expressions**, which
> is what the "re" in "grep" stands for.) Regular expressions are both complex
> and powerful; if you want to do complex searches, please look at the lesson
> on [our website](http://software-carpentry.org). As a taster, we can
> find lines that have an 'o' in the second position like this:
>
>     $ grep -E '^.o' haiku.txt
>     You bring fresh toner.
>     Today it is not working
>     Software is like that.
>
> We use the `-E` flag and put the pattern in quotes to prevent the shell
> from trying to interpret it. (If the pattern contained a '\*', for
> example, the shell would try to expand it before running `grep`.) The
> '\^' in the pattern anchors the match to the start of the line. The '.'
> matches a single character (just like '?' in the shell), while the 'o'
> matches an actual 'o'.

> ## Binary Files {.callout}
>
> We have focused exclusively on finding things in text files. What if
> your data is stored as images, in databases, or in some other format?
> One option would be to extend tools like `grep` to handle those formats.
> This hasn't happened, and probably won't, because there are too many
> formats to support.
>
> The second option is to convert the data to text, or extract the
> text-ish bits from the data. This is probably the most common approach,
> since it only requires people to build one tool per data format (to
> extract information). On the one hand, it makes simple things easy to
> do. On the negative side, complex things are usually impossible. For
> example, it's easy enough to write a program that will extract X and Y
> dimensions from image files for `grep` to play with, but how would you
> write something to find values in a spreadsheet whose cells contained
> formulas?
>
> The third choice is to recognize that the shell and text processing have
> their limits, and to use a programming language such as Python instead.
> When the time comes to do this, don't be too hard on the shell: many
> modern programming languages, Python included, have borrowed a lot of
> ideas from it, and imitation is also the sincerest form of praise.

> ## Conclusion
> 
> The Unix shell is older than most of the people who use it. It has
> survived so long because it is one of the most productive programming
> environments ever created --- maybe even *the* most productive. Its syntax
> may be cryptic, but people who have mastered it can experiment with
> different commands interactively, then use what they have learned to
> automate their work. Graphical user interfaces may be better at the
> first, but the shell is still unbeaten at the second. And as Alfred
> North Whitehead wrote in 1911, "Civilization advances by extending the
> number of important operations which we can perform without thinking
> about them."

> ## Using grep {.challenge}
>
> ~~~
> The Tao that is seen
> Is not the true Tao, until
> You bring fresh toner.
>
> With searching comes loss
> and the presence of absence:
> "My Thesis" not found.
>
> Yesterday it worked
> Today it is not working
> Software is like that.
> ~~~
>
> From the above text, contained in the file `haiku.txt`, which command would result in the 
> following output:
>
> ~~~
> and the presence of absence
> ~~~
>
> 1. `grep of haiku.txt`
> 2. `grep -E of haiku.txt`
> 3. `grep -w of haiku.txt`
> 4. `grep -i of haiku.txt`

> ## Little Women {.challenge}
>
> You and your friend, having just finished reading *Little Women* by
> Louisa May Alcott, are in an argument.  Of the four sisters in the
> book, Jo, Meg, Beth, and Amy, your friend thinks that Jo was the
> most mentioned.  You, however, are certain it was Amy.  Luckily, you
> have a file `LittleWomen.txt` containing the full text of the novel.
> Using a`for` loop, how would you tabulate the number of times each
> of the four sisters is mentioned?  Hint: one solution might employ
> the commands `grep` and `wc` and a `|`, while another might utilize
> `grep` options.
