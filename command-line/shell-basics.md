# Shell Basics

[//]: # (TODO: add section about quotes)

## Contents

- [Contents](#contents)
- [What is a shell and why should you use it?](#what-is-a-shell-and-why-should-you-use-it)
- [Navigating](#navigating)
  - [Shell keyboard shortcutrs](#shell-keyboard-shortcutrs)
  - [Changing directories with `cd`](#changing-directories-with-cd)
  - [Getting help with `man`](#getting-help-with-man)
- [What can you do in the shell?](#what-can-you-do-in-the-shell)
  - [The `$PATH` environment variable](#the-path-environment-variable)
  - [Unix utilities: `ls`, `cat`, `grep`, `sed`, `awk`](#unix-utilities-ls-cat-grep-sed-awk)
  - [Manipulating files](#manipulating-files)
  - [File-system permissions](#file-system-permissions)
  - [The shell language](#the-shell-language)
- [Links](#links)

## What is a shell and why should you use it?

A _shell_ is a textual interface for using a computer that lets you navigate the filesystem and run programs. It might look scary at first because all the pretty pictures you're used to have been replaced by text, but the shell has a few important advantages over graphical user interfaces (GUIs):

- It's usually faster than clicking around
- It lets you easily perform highly specific operations by flexibly chaining commands together
- A lot of useful open-source software is only usable through the shell
- If you need to access a data storage node or computing cluster, you probably have no choice

Linux distros usually come with a shell called bash, and macOS comes with zsh. If you are on a Windows computer, using the Windows Subsystem for Linux is highly recommended over batch or PowerShell so that you are not functionally illiterate :P. Other shells also exist with various quality of life features (I use [fish](https://fishshell.com/)). Unless specified, the topics covered here work identically in any shell. You can launch your shell by searching for "Terminal" in your OS' applications.

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

### Shell keyboard shortcutrs

A few basic keyboard shortcuts before we move onto specific programs:

- The up and down arrow keys let you scroll through the previous prompts one at a time.
- `Ctrl` + `R` lets you search for a previous command.
- `Ctrl` + `C` kills a program and returns you to the shell prompt.
- `Ctrl` + `D` closes the current shell or program when the cursor is on an empty line.
- `Ctrl` + `L` clears the terminal screen.
- `Ctrl` + `A` or `Ctrl` + `E` moves the cursor to the beginning or end of the line respectively.
- `Ctrl` + `U` erases the whole current line.
- In general, pressing `q` from inside a program will quit and return you to the shell.

In most shells, including bash, zsh, and fish, you can press `Tab` after typing the beginning of a program or file to autocomplete. If only one match exists, the shell will automatically fill in the rest of the name. If there are no matching entries, nothing happens. If there's more than one matching entry, the shell will print all matches on the next line of the terminal. You can press `Tab` again to cycle through the options, but if there are many of them, you may want to type some more characters before pressing `Tab` again to autocomplete.

### Changing directories with `cd`

To change your working directory, i.e., move around the file system, you use the `cd` (change directory) command. You just pass in the path to your desired new working directory, and the shell prompt will change to show that you have moved:

```shell-session
saikoukuntla@Sais-Macbook ~ % cd /bin 
saikoukuntla@Sais-Macbook /bin % 
```

Changing your working directory is useful because it lets you reference programs and files with _relative_ _paths_. So far, we've only seen _absolute_ _paths_, which require you to start at the _root_ directory of the filesystem and specify the full set of directories to navigate to your file. Relative paths instead let you use `..` as shorthand for the parent directory of the current working directory, and `.` as the current directory itself. For example, you can navigate to the parent directory with `cd ..`, two directories up with `cd ../..`, and so on. Some other important basics:

- `cd -` returns you to the previous directory.
- `cd ~` takes you to your home directory.
- `cd ~user` takes you to another user's home directory.

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

By default, `man` opens manual pages in `less`, a text reader which can be navigated with the keyboard. For example, you can press `q` to return to the shell, scroll up and down with the arrow keys, press `g` to return to the top, and `/[pattern]` to search for a specific regex pattern. You can jump to the next instance of the pattern by pressing `n`. `less` has many more shortcuts than this, which you can see by pressing `h` from inside `less` or by running `man less`.

You can also get a short version of the `man` for many programs by running them with the `--help` option. It might also be helpful to install `tldr`, a command-line utility that outputs shortened,
usage-forward help pages for programs.

## What can you do in the shell?

### The `$PATH` environment variable

Many of the things we've run so far are programs (`cd` is technically not a program). How does the shell know where to find them? The shell and programs can access information about your machine via named values called _environment_ _variables_. When you type a program's name into the shell, it consults an environment variable called `$PATH`. `$PATH` contains a list of directories, separated by `:`, that the shell should look for programs in:

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

You can see a list of all the environment variables with the `env` command (no arguments).

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

This will usually print many programs -- you might be wondering how those got there, since you didn't install them! You can also use `ls` with the `-a` flag to see all files in the directory (including hidden files, which begin with `.`) and/or with the `-l` flag to see extra file details like the read-write permissions, owner, and last modified time.

Programs in the `/bin` directory are pre-installed on Unix and Unix-like operating systems, as part of the requirements of the Portable Operating System Interface (POSIX) standard. This means you can expect many programs and commands to exist no matter what (Unix-like) machine you're using (see a [full list](https://en.wikipedia.org/wiki/List_of_POSIX_commands)).

For now, we'll focus on the most important ones. Here are some simple ones for printing files:

- `cat file` prints the contents of `file`.
- `sort file` prints the lines of `file` in sorted order.
- `uniq file` prints the lines of `file`, ignoring consecutive duplicate lines.
- `head file` or `tail file`, which print the first and last few lines of `file`.

`grep pattern file`, which prints lines that match `pattern` in `file`, can be used as a very powerful search tool; `pattern` can be a _regular expression_ (regex), which lets you specify templates to match instead of exact strings. You can also give `grep` a directory to search all the files in it at once, and pass the `-r` flag to search all nested directories recursively.

There are some other tools that are a bit more complicated but very useful.

#### `sed`

`sed` is a programmable file editor that can change many lines in a file at once. A common use case is:

```shell-session
saikoukuntla@Sais-Macbook ~ % sed -i 's/pattern/replacement/g' file
```

Here, the `-i` flag specifies edits should be inline (as opposed to leaving the file unchanged and just printing a version with edits). `s/` tells `sed` you want to perform a substitution and `pattern` is regex that specifies what strings you want to replace. `/g` indicates that you want to replace all occurrences of `pattern` in each line, not just the first. `replacement` can also incorporate special regex to refer back to the parts of the pattern that were matched in each instance.

#### `find`

`find` lets you find files that match certain criteria. For example:

```shell-session
saikoukuntla@Sais-Macbook ~ % find ~/Downloads -type f -name "*.zip" -mtime +30
```

finds zip files in the `Downloads` folder that are older than 30 days. A more complicated example:

```shell-session
saikoukuntla@Sais-Macbook ~ % find ~ -type f -size +100M -exec ls -lh {} \;
```

The first part of the command finds files larger than 100 MB in your home directory. The `-exec` flag to `find` lets you supply a command, terminated with `\;` and using `{}` as a signifier of the filename, that `find` should execute for each file it finds. In this case, it will list each file. You can use `-exec` to do some fairly complicated things:

```shell-session
saikoukuntla@Sais-Macbook ~ % find . -name "*.py" -exec grep -l "TODO" {} \;
```

The `-l` flag to `grep` tells it to print the name of a file if it finds the input string inside. `find` runs `grep -l "TODO" file` for each Python file in the current directory, so that the full command prints the names of Python files that have TODO items in them.

#### `awk`

`awk` lets you parse structured files, e.g. CSV or JSON files, and extract certain parts of each entry:

```shell-session
saikoukuntla@Sais-Macbook ~ % awk '{print $2}' file
```

will split each line into tokens `file` by whitespace, and print the second token from each line. You can use the `-F ,` option to split each line by commas (or any other character instead). `awk` has many more advanced features -- filtering rows, computing sums using the entries, printing formatted summaries of files, and more.

### Manipulating files

Many use cases will require you to modify the structure of your filesystem. You can use the following commands to manipulate files:

#### `mkdir`

`mkdir /path/to/folder` creates a new folder(s) at the specified location(s). To pass in multiple folders, you can pass in multiple paths separated by spaces. By default, `mkdir` requires that the intermediate folders (`path/` and `to/` in this case) already exist, but you can pass the `-p` option to have `mkdir` create intermediate directories that don't already exist.

#### `rm`

`rm /path/to/file` removes (deletes) a file. Unlike your file explorer's delete button, this deletes things permanently. You can also pass in a directory instead and use the `-r` flag to remove all its nested contents. The `-i` flag will make `rm` ask for confirmation for each file that it tries to remove.

#### `cp`

`cp /path/to/source_file /path/to/target_file` copies the contents of the source file to the target file.
`cp /path/to/source_file ... /path/to/target_directory` copies the source file(s) to the target directory with the same name.
`cp -r /path/to/source_folder /path/to/target_directory` copies the directory and entire subtree to the target directory.

#### `mv`

`mv /path/to/source_file /path/to/target_file` moves the source file to the target filepath. If the target filename is different from the source, this also renames the file.
`mv /path/to/source_file ... /path/to/target_directory` moves each of the source files to the target directory, preserving their filenames.

#### `touch`

`touch file` sets the modification and access time of the file. This creates the file if it does not already exist. You can choose to set only the modification time or only the access time with the `-m` and `-a` flags respectively.

Note that all these commands have option flags that modify their functionality. I've only mentioned a few for each command here, but you can use `man` to learn more about each of these!

#### Globs

You may sometimes need to use some commands above (or below) on many files. To avoid running a command separately for each file, Unix-like shells support _glob_ patterns, which can match strings of the structure specified in the pattern. When a shell encounters a glob in a command, it replaces the glob with the filesystem entries that match the glob. Specifying globs is simple:

- `?` (not in brackets) matches any non-`/` character once.
- `*` (not in brackets) matches a string of zero or more non-`/` characters.
- `**` matches any number of path segments including 0, though some shells may not support this.
- `/` separates path segments.
- `[...]` or `[!...]` matches any single character within the brackets. The characters in the brackets can be a list `[abc]`, range `[a-c]`, or character class `[[:digit:]]`. If the first character in the brackets is `!`, it will instead match any single character NOT in the brackets.
- Normal characters match only themselves.

By combining these elements, you can apply commands to specific sets of files. For example, if you have seen the light and decided to renounce MATLAB, `rm projects/**/*.m` will rid your `projects` folder of any disgusting MATLAB source files. Some programs may also support regex, which is a richer pattern matching language, as inputs, but the shell may not.

### File-system permissions

To prevent users of shared computers or servers from using the above commands to meddle in other people's files, Unix-like systems have file-system permissions -- the operating system keeps a record of who created each folder or file, and who is allowed to access them. Unix-based file systems specify three classes of users (owner, owner group, and others), and track three permissions that can be granted or denied to each class:

- read (r): for a file, this permission grants the ability to read the file's or directory's content. For a directory, this means access to the names of files and subdirectories, but not necessarily their contents. Access to contents is controlled by the permissions on each file/directory.
- write (w): for a file, this allows modification of the contents. For a directory, this grants the ability to create, delete, or rename entries, but only for users that already have execute access to the directory.
- execute (x): for files that a user can read, this grants the ability to execute a file (i.e., as a program). For directories, this grants the ability to enter (`cd`) inside. If a user also has the write permission for a directory, they can manipulate files within.

#### `chmod`

`chmod mode` modifies the permissions for a file or directory. You must be the owner of a file or a superuser (admin) to modify its permissions. `mode` refers to specific symbols that specify which permissions to modify and how. The mode is usually specified as a set of three numbers, where the first, second, and third numbers correspond to the permissions for the user (owner), group, and others. The decimal value of the number corresponds to a three-bit binary number that indicates the new permissions, in the order `rwx`. Therefore, `7` corresponds to granting all three permissions, and `4` corresponds to granting only read permission. For an example of the full command, `chmod 644` would make a file writable by the owner only, readable by anyone, and executable by no one. There's also a way to specify the mode with letters and operators, though that is much less common.

If you try to operate on a file without the appropriate permissions, you'll get a "permission denied" error. If you have superuser access, you can prepend `sudo` to the command -- this will prompt you for your password and run the command as a superuser. Don't try this on a company or university server if you're not a superuser though! Failed `sudo` attempts will usually be logged.

### The shell language

You might have noticed from above that the `-exec` flag from `find` let you flexibly combine it with other programs to perform very specific operations. If you were hoping that you could do that with arbitrary programs, you're in luck! Shells actually implement a full programming language that lets you chain programs together, gives programs text files as input or output, and execute programs conditionally.

#### Pipes (`|`) and `xargs`

Pipes (`|`) let you automatically pass the standard output of one program into the standard input of another. Say we want to print the first 5 lines of each Python file in the current directory with the string `TODO` in it. We know that `head` can print the first few lines of files that are passed in as arguments, and `grep -l TODO *.py` will print the filenames of the TODO files to the standard output. If we just piped `grep`'s output to `head`, `head` would read from standard input and print the first 5 lines of `grep`'s output — i.e., the first 5 filenames. What we actually want is for those filenames to become arguments to `head`, so it opens each file and prints its first 5 lines. Converting standard input to arguments is a common enough use case that `xargs` exists for it. Combining a pipe and `xargs` lets you chain the standard output of one program to the arguments of another:

```shell-session
saikoukuntla@Sais-Macbook ~ % grep -l TODO *.py | xargs head -n 5
```

No special `-exec` flag required! Note that pipes consume the standard output of a program, so only the last program in the pipe will print to the terminal.

#### Redirects (`>`, `>>`, `<`)

You can also have programs read and write files instead of the command-line: appending `>file` to a command will write its output to `file`, creating `file` if it doesn't exist and overwriting its content if it does. Keep in mind that the output file is opened and erased _before_ the program does its job, so something like `sort file >file` erases the contents of the file instead of saving a sorted version (use `sort -o` instead).

If you want to append to `file` instead, you can use `>>file`. Like pipes, these file redirects will also consume the standard output. If you want to see what gets saved to the file, you can use `tee file`, which both saves its input to `file` _and_ prints the input to standard output.

You can also give programs inputs from files using `<file`.

#### Conditionals and Loops

You can specify conditionals like `if command1; then command2; command3; fi`, which will execute `command1`, and if it doesn't throw an error, also run `command2` and `command3`. You can also specify an else branch. The common way to use `if` is with the `test` command, which is a program that evaluates conditionals. For example, you can do `test -f file` or `[ -f file ]`to check if a file exists or `[ "$var" = "string" ]` to branch on the value of an environment variable. Here, `[` is just an alias for the test program that makes shell programs easier to write. The spaces in these commands are important and easy to omit, but if you remember that `[` is a program, you can deduce that it should have a space after it.

Bash also has while loops: `while command1; do command2; command3; done` and for loops: `for varname in a b c d; do command; done`. For for loops specifically, you often will want to use _command substitution_, like `for i in $(seq 1 10); do command1; done`. This executes the command `seq 1 10`, which prints the numbers 1 through 10, and replaces the $() with the command's output, making the loop iterate from 1 to 10.

As you write more complex and useful shell scripts, you'll want to write them into a `.sh` file instead:

```bash
#!/bin/bash
# Setup log file
LOGFILE="test_runs_$(date +%s).log"
echo "Logging to $LOGFILE"

# Run tests until one fails
RUN=1
while uv run my_test > "$LOGFILE" 2>&1; do
    echo "Run $RUN passed"
    ((RUN++))
done

# Cleanup and report
echo "Test failed on run $RUN"
echo "Last 20 lines of output:"
tail -n 20 "$LOGFILE"
echo "Full log: $LOGFILE"
```

Don't worry if you don't understand everything in here yet! For now I'll focus on the first line. This is called a _shebang_. When the OS sees `#!/path` at the beginning of a file, it will pass the inputs of the file to the program at `path`. So executing the above script from any shell would still run it in bash.

## Links

- This [cheatsheet](../cheatsheets/shell-basics.md) summarizes the concepts, programs, and commands covered in this document.
- This guide borrows from the [Missing Semester course](https://missing.csail.mit.edu/2026/course-shell/) but adds more beginner-friendly details and extra content that I think beginners should know (shortcuts, permissions, file manipulation, etc.).
