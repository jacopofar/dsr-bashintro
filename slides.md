---
# try also 'default' to start simple
theme: default
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
# background: https://cover.sli.dev
# some information about your slides (markdown enabled)
title: "Bash and the UNIX command line"
info: |
  made for Data Science Retreat
  by Jacopo Farina

  Based on [Sli.dev](https://sli.dev)
# apply unocss classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
# to serve statically without that awful history manipulation
routerMode: hash
# enable Comark Syntax: https://comark.dev/syntax/markdown
comark: true
# duration of the presentation
duration: 240min
---

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  background-clip: text;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}

.slidev-layout h1 + p {
  opacity: 0.9;
}
</style>

# Bash and the UNIX-like command line

Jacopo Farina
@ Data Science Retreat
---

# What is a shell

The "original" way to interact with a computer, still relevant today!

* Usually the fastest way to do things
* Necessary to work on servers
* Whatever you can do from the terminal, can later be automated

```python
from os import system
>>> system('whoami')
jacopo
0
>>> system('uptime')
 15:46:56 up  5:28,  1 user,  load average: 3.36, 3.55, 3.76
```

(for fancier ways to run commands from Python, you can look at the `subprocess` module).

Often in documentation a `$` prefix indicates that a command is for the shell. `#` indicates a shell running as an administrator (or with `sudo`).

---

# Your shell

We call the class `Bash` but it's one of the possible command-line interpreters. `ZSH` is also very common (the default on macOS).
The differences are minimal for most uses and we don't really mind; you can install more and switch.

Also, you will notice that the string on the side in the terminal, called **prompt**, is different across OS and settings. It also does
not matter and can be configured.

On ZSH you may get a nicer configuration by installing `oh-my-zsh` (but don't do it now!).

The system shell, like the Python one, follows the REPL approach.

* Read (what you type after the prompt)
* Evaluate (that is, run the command)
* Print (the output)
* Loop (back to the beginning)

---

# Important Keys

|                                              |                                       |
| -------------------------------------------- | --------------------------------------|
| <kbd>Ctrl + C</kbd>                          | Empties the line / stops the program  |
| <kbd>Ctrl + D</kbd>                          | Quits                                 |
| <kbd>up</kbd>/<kbd>down</kbd>                | Retrieve previous commands            |
| <kbd>page up</kbd>/<kbd>page down</kbd>      | Same but with prefix (depends on settings)                 |
| <kbd>Ctrl + R</kbd>                          | Searches in command history           |
| <kbd>tab</kbd>                               | AUTOCOMPLETION!                       |

The Python shell and others share most of the same keys.

You can get a fancier command search with **atuin** and record your sessions with **asciinema**.

---

# Your bestest friend!
![the tab key](/Tabts.jpg)

Like in an IDE or in Jupyter, the tab key does autocomplete. **Get used to it!**

Not only does it save time which is always nice, but prevents many mistakes. The suggestions are contextual, and apply to **filenames** too. If you are running a command on a file with a long name you avoid typos and avoid wasting time on it. Win-win!

Try it now: run the command `whoami` but don't write all of it, only the beginning.

Depending on your system, pressing tab multiple times will show all possible completions or iterate over them.

---

# Getting help

You can get a quick overview of what a command does by adding `--help` after it (e.g. `whoami --help`).

In most systems you can get a detailed explanation of a command with `man <command name>`, and you are sure the documentation is for your exact version of it.


---

# The filesystem

At any point in time the terminal, like any other process, is "pointing" to a specific folder called working directory.
Commands will have an effect on it.

You can see where you are by typing `pwd` (Print Working Directory).

FYI Python has the same concept:

```python
>>> import os
>>> os.getcwd()
'/home/jacopo/projects/bashintro'
```

if you use `open('aaa.txt')` in Python, that folder is the one it will use to locate `aaa.txt`.

---

# Paths
<style>
  img {max-width: 40%}
</style>
![the FS tree](/fs_tree.png)

All files and folders are arranged into a tree.

A path starting with `/` is *absolute*. Otherwise it is relative to the working directory.

With "`..`" you can indicate the "upper" folder. "`.`" indicates the current folder.

"`/home/andrew/Documents/../../john`" here refers to "`/home/john/`"

---

# Moving around

Use `cd` (Change Directory) to change working directory.

* `cd Documents` goes to the Documents folder (remember: autocompletion! Just write "Doc" and let it complete)
* `cd` alone goes back to your home folder (same as `cd ~`)
* `cd ..` goes up in the tree
* `cd -` goes back to the previous location

---

# Seeing around

`ls` shows the list of what's in this folder.

Lots of flags, we'll see some of them (`l`, `h`, `t`, `a`, `r`, `S`).

As a side note: all flags preceded by a single dash can be combined, two dashes cannot:

`ls -lht` is equivalent to `ls -l -h -t`, each letter has its meaning

With two dashes like `ls --help` this does not apply. This convention is used pretty much everywhere.

With folders, `ls -l` shows the size of the **metadata** not the whole content.

For that there's another command (`du`, we'll see later)

You can filter a bit: `ls *.pdf`

---

# du and df

For total folder size use `du`

```bash
jacopo@flat2:~$ du -sh Documents/
183G    Documents/
```

for disks use `df`:

```bash
jacopo@flat2:~$ df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/dm-0       952G  466G  483G  50% /
...more lines here...
devtmpfs         24G     0   24G   0% /dev
```

---

# Create a directory or a file

`mkdir` makes a directory

It wants to create one at a time, you can create multiple with the `-p` flag (p for "parents").

`touch` creates an empty file or updates the last modification date of existing ones.

---

# Shell expansions


Nice trick:

`mkdir -p {2026..2030}/{01..12}` creates a whole structure and it has proper trailing 0s.

This `{...}` syntax is called "brace expansion" and can be applied to many other commands.

Many others in the [documentation](https://www.gnu.org/software/bash/manual/html_node/Shell-Expansions.html).

---

# Arguments and shell expansions

These expansions are done by the shell, the program does not see them but only the result

```python
import sys
# argv is for argument vector
print(sys.argv)
```

example:


```bash
$ python script.py {1..7}
['script.py', '1', '2', '3', '4', '5', '6', '7']
```


---

# Copy and move

The `cp` command copies, while `mv` moves.

To copy folders use `cp -r` (r=recursive).

Moving is also used to rename (basically "moving to another name").

Moving to an existing path deletes the overwrites (destroy) the destination!

Both accept the `-v` (v=verbose) flag to see what they are doing.

---

# Deleting

`rmdir` can delete a folder, but only if it's empty.

`rm` can delete files and folders. There is no trash bin!

* `rm -r` deletes recursively
* `rm -v` shows what is happening
* `rm -f` does not ask to confirm

---

# find

To get all the files under a directory:

`find .`

can filter by name, date, depth, and pretty much everything.

---

# Finally, files


Download the file (there's a copy button on the border)


```
wget https://gist.githubusercontent.com/jacopofar/804c5694ac12a9d6fde653b5a6e3b983/raw/8ffd027bf5e9b1184695e1e55798f699e6acda74/countries_capitals.tsv
```

or if `wget` is not present:

```
curl https://gist.githubusercontent.com/jacopofar/804c5694ac12a9d6fde653b5a6e3b983/raw/8ffd027bf5e9b1184695e1e55798f699e6acda74/countries_capitals.tsv > countries_capitals.tsv
```

now you can see a new file `countries_capitals.tsv`.

Interesting fact: `wget -r https://somesite.com` can download a whole website by following links.

---

# What did we just download?

`cat` dumps the whole file to the screen.

`wc` counts lines, characters and "words".

`head`/`tail` show the first and last 10 lines.

`less` visualizes it better (Q to quit)

---

# grep

To search into a file:

`grep Italy countries_capitals.tsv`

case insensitive:

`grep -i den countries_capitals.tsv`

inverse search:

`grep -iv a countries_capitals.tsv`


---

# vim / vi

Depending on your system you may have issues with it sooner or later.

`vim` is a command-line editor, in some cases the only one (e.g. on a server) and could be opened for you.
Since it's not intuitive at all, let's see the basics not to get stuck.

* When opening it, you are in **normal (command) mode**. Press <kbd>i</kbd> for insert mode
* In **insert mode**, press <kbd>ESC</kbd> for the command mode
* To save and quit `:wq` (the colon is included!)
* To quit without saving `:q!` (colon and exclamation mark are included!)

it can be quite powerful, but I do NOT suggest learning it now. I usually use it in the lesson to avoid switching window.

Sometimes your system has `nano` instead, much simpler, use <kbd>Ctrl + X</kbd> to exit it.


---

# STDIN, STDOUT, STDERR

Every process has a stream of data in input and two in output.

---

# The UNIX philosophy

UNIX systems had one design philosophy: provide many tools doing one and only one thing, and doing it well, plus ways to combine them.

Use `|` to send the standard output of a process as the input of another.

Use `>` or `>>` to send the output to a file.

Examples:

* how many countries contain the letter f?
* how many files in this folder/subfolders are PDFs?

---

# echo


`echo` is the same as `print` in Python. Useful combined with other commands:

`echo hello >> greetings.txt`
`echo hello again >> greetings.txt`


---
