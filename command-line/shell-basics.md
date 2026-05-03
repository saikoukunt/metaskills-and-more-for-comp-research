# Shell Basics

## What is a shell and why should I use it?

A _shell_ is a textual interface for using a computer. It lets you navigate the filesystem and run programs. You should use it because

- It's usually faster than clicking around
- It lets you easily perform highly specific operations by flexibly chaining commands together
- A lot of useful open-source software is only usable through the shell
- If you need to access a data storage node or computing cluster, you probably have no choice

Linux distros usually come with a shell called bash, and macOS comes with zsh. If you are on a Windows computer, using the Windows Subsystem for Linux is highly recommended over batch or PowerShell so that you are not functionally illiterate. Other shells also exist with various quality of life features (I use [fish](https://fishshell.com/)), but the basics covered here will apply broadly. You can launch your shell by searching for "Terminal" in your OS' applications.

## Navigating

When you launch the terminal, you'll see a prompt that looks like this:

```shell-session
saikoukuntla@Sais-Macbook ~ % 
```

The prompt shows you your username and the name of your machine, followed by the current directory (`~`, shorthand for the home directory). After the symbol, usually a `$` or `%`, you can run a _command_ by typing its name and the command will print any output to the terminal window. For example, the `pwd` command will output the absolute path of the current directory:

```shell-session
saikoukuntla@Sais-Macbook ~ % pwd
/Users/saikoukuntla
```

You can also give _arguments_ to commands:

```shell-session
saikoukuntla@Sais-Macbook ~ % echo hello
hello
```

The command above ran the `echo` program with the argument `hello`. Like its name suggests, `echo` just prints each of its arguments. When you type in a command, the shell _parses_ it by splitting on whitespace, interpreting the first word as the name of a command you want to run, and passing the remaining words to the program as arguments.

### Getting help with `man`

Arguably the most important command to know for beginners is `man`, short for manual. Use it anytime you're in doubt! If you pass in the name of a program to `man`, it will open the manual for that program, which explains what the program is and what arguments you can supply. For example, the command `man date` will open the following
man page:

```shell-session
DATE(1)                     General Commands Manual                    DATE(1)

NAME
     date – display or set date and time

SYNOPSIS
     date [-nRu] [-z output_zone] [-I[FMT]] [-r filename] [-r seconds]
          [-v[+|-]val[y|m|w|d|H|M|S]] [+output_fmt]
     date [-jnRu] [-z output_zone] [-I[FMT]] [-v[+|-]val[y|m|w|d|H|M|S]]
          [[[mm]dd]HH]MM[[cc]yy][.SS] [+output_fmt]
     date [-jnRu] [-z output_zone] [-I[FMT]] [-v[+|-]val[y|m|w|d|H|M|S]]
          -f input_fmt new_date [+output_fmt]

DESCRIPTION
     When invoked without arguments, the date utility displays the current
     date and time.  Otherwise, depending on the options specified, date will
     set the date and time or print it in a user-defined way.

     The date utility displays the date and time read from the kernel clock.
     When used to set the date and time, both the kernel clock and the
     hardware clock are updated.

     Only the superuser may set the date, and if the system securelevel (see
:
```

Arguments preceded by `--` or `-` are called _options_ or _flags_, and modify the program's behavior in some way -- by changing its output, or allowing you to pass in special arguments after them.

By default, `man` opens manual pages in `less`, a text reader which can be navigated with the keyboard. For example, you can scroll up and down with the arrow keys, press `g` to return to the top of the manual or `q` to return to the shell, and `/[pattern]` to search for a specific regex pattern. You can jump to the next instance of the pattern by pressing `n`. `less` has many more shortcuts than this, which you can see by pressing `h` from inside `less` or by running `man less`.

You can also get a short version of the `man` for many programs by running them with the `--help` option. It might also be helpful to install `tldr`, a command-line utility that outputs shortened,
usage-forward help pages for programs.

### Changing directories with `cd`

To change your working directory, i.e., move around the file system, you use the `cd` (change directory) command. You just pass in the path to your desired new working directory, and the shell prompt will change to show that you have moved:

```shell-session
saikoukuntla@Sais-Macbook ~ % cd /bin 
saikoukuntla@Sais-Macbook /bin % 
```

Changing your working directory is useful because it lets you reference programs and files with _relative_ _paths_. So far, we've only seen _absolute_ _paths_, which require you to start at the _root_ directory of the filesystem and specify the full set of directories to navigate to your file. Relative paths instead let you use `..` as shorthand for the parent directory of the current working directory, and `.` as the current directory itself. For example, you can navigate to the parent directory with `cd ..`, two directories up with `cd ../..`, and so on.

## What can I run in the shell?

### The `$PATH` environment variable

If many of the things we've been running so far are programs (`cd` is technically not a program), how does the shell know where to find them? When you type program's name into the shell, it consults a _environment_ _variable_ called `$PATH`. Environment variables are user-modifable values that the shell and programs can use. `$PATH` contains a list of directories, separated by `:`, that the shell should look for programs in:

```shell-session
saikoukuntla@Sais-Macbook ~ % echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

The shell runs the first instance of the program name that it finds with the arguments you supply. You can use the `which` command to see which file is being executed when you run a program.

```shell-session
saikoukuntla@Sais-Macbook ~ % which echo
/bin/echo 
```

We can see that the `echo` program is actually located in the `/bin` folder. You can also add directories to the `$PATH` so that you can run new programs from the shell with just their names (instead of their absolute or relative paths)! Some CLI programs will remind you to do this when you install them.

### Unix utilities: `ls`, `cat`, `grep`, `sed`, `awk`

We can use the `ls` program, which lists the files in the given directory, to see what other programs are in the `/bin` folder:

```shell-session
saikoukuntla@Sais-Macbook ~ % ls /bin
[ dd  launchctl pwd  tcsh bash  df  link 
realpath test cat  echo  ln  rm  unlink
chmod  ed  ls  rmdir  wait4path cp  expr  
mkdir  sh  zsh csh  hostname mv  sleep
dash  kill  pax  stty date  ksh  ps  sync
```

This will usually print many programs -- you might be wondering how those got there, since you didn't install them! Programs in the `/bin` directory are pre-installed on Unix and Unix-like operating systems, as part of the requirements of the Portable Operating System Interface (POSIX) standard. This means you can expect many programs and commands to exist no matter what (Unix-like) machine you're using (see [here](https://en.wikipedia.org/wiki/List_of_POSIX_commands) for a full list).

For now, we'll focus on the most important ones. Here are some simple ones for printing files:

- `cat file` prints the contents of `file`.
- `sort file` prints the lines of `file` in sorted order.
- `uniq file` print the lines of `file`, ignoring consecutive duplicate lines,
- `head file` or `tail file`, which print the first and last few lines of `file`.

`grep pattern file`, which finds lines that match `pattern` in `file`, can be used as a very powerful search tool; `pattern` can be a _regular expression_ (regex), which lets you specify templates to match instead of exact strings. You can also give `grep` a directory to search all the files in it at once, and pass the `-r` flag to search all nested directories recursively.

There are some other tools that are a bit more complicated but very useful.

#### `sed`

`sed` is a programmable file editor can change many lines in a file at once. A common use case is:

```shell-session
saikoukuntla@Sais-Macbook ~ % sed -i `s/pattern/replacement/g` file
```

Here, the `-i` flag specifies edits should be inline (as opposed to leaving the file unchanged and just printing a version with edits). `s/` tells `sed` you want to perform a substitution and `pattern` is regex that specifies what strings you want to replace. `/g` indicates that you want to replace all occurences of `pattern` in each line, not just the first. `replacement` can also incorporate special regex to refer back to the parts of the pattern that were matched in each instance.

#### `find`

`find` lets you find files that match certain criteria. For example:

```shell-session
saikoukuntla@Sais-Macbook ~ % find ~/Downloads -type f -name "*.zip* -mtime + 30
```

finds zip files in the `Downloads` folder that are older than 30 days. A more complicated example:

```shell-session
saikoukuntla@Sais-Macbook ~ % find ~ -type f -size +100M -exec ls -lh {}\;
```

The first part of the command finds files larger than 100 MB in your home directory. The `-exec` flag to `find` lets you supply a command, terminated with `\;` and using `{}` as a signifier of the filename, that `find` should execute for each file it finds. In this case, it will list each file. You can use `-exec` to do some fairly complicated things:

```shell-session
saikoukuntla@Sais-Macbook ~ % find . -name "*.py" -exec grep -l "TODO" {};
```

The `-l` flag to `grep` tells it to print the name of a file if it finds the input string inside. `find` runs `grep -l "TODO" file` for each Python file in the current directory, so that the full command prints the names of Python files that have TODO items in them.

#### `awk`

`awk` lets you parse structured files, e.g. CSV or JSON files, and extract certain parts of each entry:

```shell-session
saikoukuntla@Sais-Macbook ~ % awk `{print $2}` file
```

will split each line into tokens `file` by whitespace, and print the second token from each line. You can use the `-F ,` option to separate split each line by commas (or any other character instead). `awk` has many more advanced features -- filtering rows, computing sums using the entries, printing formatted summaries of files, and more.

### The shell language

You might have noticed from above that the `-exec` flag from `find` let you flexibly combine it with other programs to perform very specific operations. If you were hoping that you could do that with arbitrary programs, you're in luck! Shells actually implement a fullprogramming language that lets you chain programs together, gives programs text files as input or output, and execute programs conditionally. These are great

Pipes (`|`) let you automatically pass the output of one program into the inputs of another. For example:

```shell-session
saikoukuntla@Sais-Macbook ~ % grep -l TODO *.py | ls -l
```

will print long information about each Python file in the current directory with the string `TODO` in it. No special `-exec` flag required! Note that pipes consume the standard output of a program, so only the last program in the pipe will print to the terminal.

You can also have programs read and write files instead of the command-line: appending `>file` to a command will write its output to `file`, creating `file` if it doesn't exist and overwriting its content if it does. If you want to append to `file` instead, you can use `>>file`. Like pipes, these file redirects will also consume the standard output. If you want to see what gets saved to the file, you can use `tee file`, which both saves its input to `file` _and_ prints the input to standard output. You can also give programs inputs from files using `<file`.

You can specific conditionals like `if command1; then command2; command3; fi`, which will execute `command1`, and if it doesn't throw an error, also run `command2` and `command3`. You can also specify an else branch. The common way to use `if` is with the `test` command, which is a program that evaluates conditionals. For example, you can do `test -f file` or `[ -f file ]`to check if a fle exists or `["$var" = "string"]` to branch on the value of an environment variable. Here, `[` is just an alias for the test program that makes shell programs easier to write.

Bash also has while loops: `while command 1; do command2l command3; done` and for loops: `for varname in a b c d; do command; done`. For for loops specifically, you often will want to use _command substitution_, like `for i in $(seq 1 10); do command1; done`. This executes the command `seq 1 10`, which prints the numbers 1 through 10, and replaces the $() with the command's output, making the loop iterate from 1 to 10.

As you write more complex and useful shell scripts, you'll want to write them into a `.sh` file instead:

```bash
#!/bin/bash
set -euo pipefail

# Start CPU stress in background
stress --cpu 8 &
STRESS_PID=$!

# Setup log file
LOGFILE="test_runs_$(date +%s).log"
echo "Logging to $LOGFILE"

# Run tests until one fails
RUN=1
while cargo test my_test > "$LOGFILE" 2>&1; do
    echo "Run $RUN passed"
    ((RUN++))
done

# Cleanup and report
kill $STRESS_PID
echo "Test failed on run $RUN"
echo "Last 20 lines of output:"
tail -n 20 "$LOGFILE"
echo "Full log: $LOGFILE"
```

You should look up the things that you don't understand in this script, but I'll explain the first line. This is called a _shebang_. When the shell sees `#!/path` at the beginning of a file, it will pass the inputs of the file to the program. So executing the above script from any shell would still run it in bash.

## Sources

This guide borrows heavily from the [Missing Semester course](https://missing.csail.mit.edu/2026/course-shell/), with some extra information added and a few shifts to the order.
