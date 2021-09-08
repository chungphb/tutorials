# Shell Scripting

## Basic Unix / Linux

### Unix Architecture

* Kernel
* Shell
  * Examples: C Shell, Bourne Shell, Korn Shell, etc.
* Commands and Utilities
  * Examples: `cp`, `mv`, `cat`, `grep`, etc.
* Files and Directories

### System commands

#### Bootup

#### Login

#### Change Password

```bash
$ passwd
```

#### Who am I?

```bash
# Checks your information
$ whoami

# Checks who is logged in to the computer at the same time
$ users
$ who
$ w
```

#### Logout

```bash
$ logout
```

#### Shutdown

```bash
# Brings the system down immediately
$ halt

# Powers off the system to synchronize and clean up the system prior to shutting down
$ init 0

# Shuts down the system by powering off
$ poweroff

# Shuts down the system
$ shutdown

# Reboots the system by shutting it down completely and then restarting it
$ init 6

# Reboots the system
$ reboot
```

### File Management

#### List Files

```bash
# Lists files (and directories)
$ ls

# Lists files (and directories) with more information
#
# Result:
# 1st column | File type and permission
# 2nd column | Numver of memory blocks taken by the file
# 3rd column | The owner of the file
# 4th column | The group of the owner
# 5th column | The file size (in bytes)
# 6rd column | The date and the time when this file was created or modified for the last time
# 7th column | The file name
#
# File types:
# - | Regular file
# b | Block special file
# c | Character special file
# d | Directory
# l | Symbolic link file
# p | Named pipe (Interprocess communication)
# s | Socket (Interprocess communication)
$ ls -l
total 1
drwxrwxr-x. 2 chungphb chungphb 6 Sep  7 10:53 sample
```

##### Metacharacters

```bash
# Metacharacters:
# * | Matches 0 or more charaters
# ? | Matches one single character
$ ls sample*.txt
sample0.txt sample_file.txt sample
```

##### Hidden Files

```bash
$ ls -a
.			.gitconfig
..			.idea
.cache		.java
.ccache		.local
.cmake		.m2
Desktop		workspace
```

#### Create / Edit Files

```bash
# Step 1: Use vi command creating / editing the file
# Step 2: Press the key [i] to enter the edit mode
# Step 3: Edit
# Step 4: Press the key [Esc] to exit the edit mode
# Step 5: Press together two keys [Shift] and [ZZ] ([Z] then [Z]) to exit
$ vi sample.txt
```

#### Display Content of a File

```bash
$ cat sample.txt
Hello world!

# Display the line numbers
$ cat -b sample.txt
1 Hello world!

# Count number of lines, words and characters contained in a file
#
# Result:
# 1st column | The number of lines
# 2nd column | The number of words
# 3rd column | The number of bytes
# 4th column | The file name

$ wc sample.txt
1 2 13 sample.txt
```

#### Copy Files

```bash
$ cp sample.txt sample_copy.txt
```

#### Rename Files

```bash
$ mv sample.txt new_sample.txt
```

#### Delete Files

```bash
$ rm new_sample.txt
```

### Directories

#### Home Directory

```bash
# Goes to our home directory
$ cd ~

# Goes to other user's home directory
$ cd ~chungphb

# Goes to the last directory
$ cd -
```

#### Current Working Directory

```bash
$ pwd
```

#### List Directories

```bash
$ ls /usr/local
```

#### Create Directories

```bash
# Creates a directory
$ mkdir sample

# Creates all necessary directories
$ mkdir -p /tmp/chungphb/sample
```

#### Remove Directories

```bash
$ rmdir sample
```

#### Change Directories

```bash
$ cd /tmp/chungphb/
```

#### Rename Directories

```bash
$ mv sample new_sample
```

#### The directories . and ..

###  File Permission / Access Modes

#### File Access Modes

* Read
* Write
* Execute

#### Directory Access Modes

* Read
* Write
* Execute

#### Change Permissions

##### Use `chmod` in Symbolic Mode

```bash
# Chmod operators:
# + | Adds the designated permission(s)
# - | Removes the designated permission(s)
# = | Sets the designated permission(s)
$ chmod o+wx sample.txt
$ chmod u-x sample.txt
$ chmod g=rx sample.txt

# Combines all commands
# $ chmod o+wx,u-x,g=rx sample.txt
```

##### Use `chmod` with Absolute Permissions

```bash
# Permissions:
# 0 | ---
# 1 | --x
# 2 | -w-
# 3 | -wx
# 4 | r--
# 5 | r-x
# 6 | rw-
# 7 | rwx
$ chmode 755 sample.txt
```

#### Change ownership

```bash
$ chown chungphb sample.txt
```

#### Change group ownership

```bash
$ chgrp vht sample.txt
```

#### SUID and SGID File Permission

* Additional permissions are given to programs via a mechanism known as the ***Set User ID (SUID)*** and ***Set Group ID (SGID)*** bits.
* When a user executes a program that has the SUID bit enabled, he/she/they inherits the permissions of that program's owner.
* When a user executes a program that has the SGID bit enabled, he/she/they inherits the permissions of that program's group owner.
* The SUID and SGID bits will appear as the letter ***s*** at the bits where the owners' execute permission normally resides.
* A capital letter ***S*** in the execute position indicates that the execute bit is not set.

```bash
# Sets the SUID and SGID bits
$ chmod ug+s sample.txt
```

### Environment

#### Initialization

* Sets up the environment for the shell before displaying a prompt for users to use.
* Usually a two-steps process that involves the shell reading the following two files: `/etc/profile` and `.profile`.

#### The `.profile` File

* The file `/etc/profile` is maintained by the system administrator while the file `.profile` is under our control.
* The basic set of information:
  * The terminal type
  * A list of directories in which to locate the commands
  * A list of variables affecting the look and feel of the terminal

##### Set the Terminal Type

* Usually, the terminal type is automatically configured.
* If the terminal is set incorrectly, most users can reset their terminal to the lowest common denominator.

```bash
$ TERM=vt100
```

##### Set the PATH

* The PATH variable specifies the locations in which the shell should look for commands.
* The entries are separated by the colon character (***:***).

```bash
$ PATH=/bin:/usr/bin
```

##### PS1 and PS2 Variables

* ***PS1:*** Decides the characters that the shell displays as the command prompt.

```bash
# To change the command prompt from $ to =>
$ PS1="=> "
=> cd sample
=> ls -l

# To show the working directory
# Escape sequences:
# \t | Current time
# \d | Current date
# \n | New line
# \s | Current shell environment
# \W | Working directory
# \w | Full path of the working directory
# \u | Current user's username
# \h | Hostname of the current machine
# \# | Command number of the current command
# \$ | If we are logged in as root, end the prompt with the # character; otherwise, use the $ sign
=> PS1="[\u@\h \w]\$ "
[chungphb@chungphb-VD ~/shell]$ cd sample
[chungphb@chungphb-VD ~/shell]$ ls -l
```

* ***PS2:*** Decides the characters that the shell displays as the secondary command prompt (when the issued command is incomplete).

```bash
# To change the secondary command prompt from > to %
$ PS2="% "
$ echo "Hello
% world"
```

  #### Environment Variables

<table>
    <thead>
        <tr>
            <th>Variable</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><code>ISPLAY</code></td>
            <td>The identifier for the display</td>
        </tr>
        <tr>
            <td><code>HOME</code></td>
            <td>The home directory of the current user</td>
        </tr>
        <tr>
            <td><code>LANG</code></td>
            <td>The default system locale</td>
        </tr>
        <tr>
            <td><code>LD_LIBRARY_PATH</code></td>
            <td>A colonseperated list of directories that the dynamic linker should search for shared objects</td>
        </tr>
        <tr>
            <td><code>PATH</code></td>
            <td>The search path for commands</td>
        </tr>
        <tr>
            <td><code>PWD</code></td>
            <td>The current working directory</td>
        </tr>
        <tr>
            <td><code>RANDOM</code></td>
            <td>A random integer between 0 and 32,767</td>
        </tr>
        <tr>
            <td><code>TERM</code></td>
            <td>The display type</td>
        </tr>
        <tr>
            <td><code>TZ</code></td>
            <td>Time zone</td>
        </tr>
        <tr>
            <td><code>UID</code></td>
            <td>The numeric user ID of the current user</td>
        </tr>
    </tbody>
</table>


### Pipes and Filters

#### The `grep` Command

* Searches a file or files for lines that have a certain pattern.
* Reads the standard output when a filename is not given.
* Can be linked up into a pipe.

```bash
# Options:
# -v | Prints all lines that do not match the pattern
# -n | Prints matched lines with their line number
# -l | Prints only the name of the files with matching lines
# -c | Prints only the number of matched lines
# -i | Allows matching with both uppercase and lowercase 
$ ls -l | grep "sample"
```

#### The `sort` Command

* Arranges lines of text alphabetically or numerically.
* Can be linked up into a pipe.

```bash
# Options:
# -n | Sorts numerically
# -r | Reverses the order
# -f | Ignores case
$ ls -l | grep "sample" | sort
```

#### The `pg` and `more` Commands

```bash
# Shortens the ouput horizontally
$ ls -l | grep "sample" | sort | pg

# Shortens the ouput vertically
$ ls -l | grep "sample" | sort | more
```

### Processes

#### Start Processes

##### Foreground Processes

* By default, every process that we start runs in the foreground.
* They get input from the keyboard and sends output to the screen.
* While a program is running in the foreground, no other commands can be run because the prompt would not be available.

##### Background Processes

* A background process runs without being connected to your keyboard. If the background process requires any keyboard input, it waits.
* The simplest way to start a background process is to add an ampersand (***&***) at the end of the command.

```bash
$ ls sample*.txt &
[1] 76322
sample0.txt sample_copy.txt sample_file.txt sample.txt
[1]+ Done		ls --color=auto sample*.txt
```

#### List Running Processes

```bash
# Lists all running processes
#
# Result:
# 1st column | UID  | User ID
# 2nd column | TTY  | Terminal type
# 3rd column | TIME | CPU time taken by the process
# 4th column | CMD  | The command that started this process
$ ps
  PID TTY          TIME CMD
81206 pts/0    00:00:01 bash
83546 pts/0    00:00:00 ps 

# Lists all running processes with more information#
# Result:
# 1st column | UID   | User ID
# 2nd column | PID   | Process ID
# 3rd column | PPID  | Parent process ID
# 4th column | C     | CPPU utilization
# 5th column | STIME | Process start time
# 6th column | TTY   | Terminal type
# 7th column | TIME  | CPU time taken by the process
# 8th column | CMD   | The command that started this process
# Note: Process ID is different from Job ID
$ ps -f
UID         PID   PPID  C STIME TTY          TIME CMD
chungphb  81206  81186  0 Sep07 pts/0    00:00:01 /bin/bash
chungphb  83546  81206  0 11:35 pts/0    00:00:00 ps -f

# Other options:
# -a | Shows information about all users
# -x | Shows information about processes without terminals
# -u | Shows additional information
# -e | Displays extended information
$ ps -aux
```

#### Stop Processes

```bash
# Kills a process
$ kill 80000

# Kills a process if it ignores the regular kill command
$ kill -9 80000
```

#### Parent and Child Processes

* Sometimes the parent process is killed before its child.
  * In this case, the ID of the "parent of all processes", the ***init*** process, becomes the new PPID (parent process ID).
  * These processes are called orphan processes.
* When a process is killed, a `ps` listing may still show the process with a ***Z*** state.
  * This is a zombie or defunct process.
  * These processes are different from the orphan processes.
  * They have completed execution but still find an entry in the process table.

#### Daemon Processes

* Daemons are system-related background processes. They run in the background, usually waiting for something to happen that they are capable of working with.
* Daemons has no controlling terminal. Therefore, they all have a ***?*** for their **TTY**.

#### The `top` command

```bash
# Shows information about physical and virtual memory, CPU usage, load averages, etc. of the processes
$ top
```

### Communication

#### The `ping` Utility

```bash
# Sends an echo request to a host available on the network
$ ping google.com
```

#### The `ftp` Utility

```bash
# Supports uploading and downloading files from one computer to another computer
#
# Commands:
# put filename   | Uploads file from the local machine to the remote machine
# get filename   | Downloads file from the remote machine to the local machine
# mput filelist  | Uploads a list of files from the local machine to the remote machine
# mget filelist  | Downloads a list of files from the local machine to the remote machine
# prompt on/off  | Turns the prompt on/off
# dir            | Lists all the files available in the current directory
# cd dirname     | Changes directory to dirname on the remote machine
# lcd dirname    | Changes directory to dirname on the local machine
# quit           | Quits the current login
$ ftp chungphb.com
ftp> dir
ftp> get sample.txt
ftp> quit
```

#### The `telnet` Utility

```bash
# Allows a computer user at one site to make a connection, login and then conduct work on a computer at another site
$ telnet chungphb.com
```

####  The `finger` Utility

```bash
# Displays information about users on a given host
$ finger
```

### The `vi` Editor Tutorial

#### Start `vi`

<table>
    <thead>
        <tr>
            <th>Command</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><code>vi filename</code></td>
            <td>Creates a new file if it does not exist, otherwise opens an existing file</td>
        </tr>
        <tr>
            <td><code>vi -R filename</code></td>
            <td>Opens an existing file in the read-only mode</td>
        </tr>
        <tr>
            <td><code>view filename</code></td>
            <td>Opens an existing file in the read-only mode</td>
        </tr>
    </tbody>
</table>


#### Operation Modes

##### Command mode

Enables performing administrative tasks such as saving the files, executing the commands, moving the cursor, cutting (yanking) and pasting the lines or words, as well as finding and replacing.

##### Insert mode

Enables inserting text into the file.

##### Conversions

* `vi` always starts in the ***command mode***.
* To enter the ***insert mode***, type ***i***.
* To exit the ***insert mode***, press the ***Esc*** key.

#### Quit `vi`

* To ***quit*** out of `vi`, use the `:q` command.
* To ***quit*** out of `vi` without saving the modified file, use the `:q!` command.
* To ***save the contents*** of the editor, use the `:w` command.
* The simplest way to ***save changes and exit vi*** is with the `ZZ` command.

#### Move in `vi`

* To move within a file without affecting your text, we must be in the command mode.
* Commands:

<table>
    <thead>
        <tr>
            <th>Command</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><code>k</code></td>
            <td>Moves the cursor up one line</td>
        </tr>
        <tr>
            <td><code>j</code></td>
            <td>Moves the cursor down one line</td>
        </tr>
        <tr>
            <td><code>h</code></td>
            <td>Moves the cursor to the left one character position</td>
        </tr>
        <tr>
            <td><code>l</code></td>
            <td>Moves the cursor to the right one character position</td>
        </tr>
    </tbody>
</table>


#### Commands

##### Edit Files

<table>
    <thead>
        <tr>
            <th>Command</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><code>i</code></td>
            <td>Inserts text before the current cursor location</td>
        </tr>
        <tr>
            <td><code>l</code></td>
            <td>Inserts text at the beginning of the current line</td>
        </tr>
        <tr>
            <td><code>a</code></td>
            <td>Inserts text after the current cursor location</td>
        </tr>
        <tr>
            <td><code>A</code></td>
            <td>Inserts text at the end of the current line</td>
        </tr>
        <tr>
            <td><code>o</code></td>
            <td>Creates a new line for text entry below the cursor location</td>
        </tr>
        <tr>
            <td><code>O</code></td>
            <td>Creates a new line for text entry above the cursor location</td>
        </tr>
    </tbody>
</table>

##### Delete characters

<table>
    <thead>
        <tr>
            <th>Command</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><code>x</code></td>
            <td>Deletes the character under the cursor location</td>
        </tr>
        <tr>
            <td><code>X</code></td>
            <td>Deletes the character before the cursor location</td>
        </tr>
        <tr>
            <td><code>dw</code></td>
            <td>Deletes from the current cursor location to the next word</td>
        </tr>
        <tr>
            <td><code>d^</code></td>
            <td>Deletes from the current cursor position to the beginning of the line</td>
        </tr>
        <tr>
            <td><code>d$</code></td>
            <td>Deletes from the current cursor position to the end of the line</td>
        </tr>
        <tr>
            <td><code>D</code></td>
            <td>Deletes from the cursor position to the end of the current line</td>
        </tr>
        <tr>
            <td><code>dd</code></td>
            <td>Deletes the line the cursor is on</td>
        </tr>
    </tbody>
</table>


##### Change commands

<table>
    <thead>
        <tr>
            <th>Command</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><code>cc</code></td>
            <td>Removes the contents of the line (<code>vi</code> remains in the insert mode)</td>
        </tr>
        <tr>
            <td><code>cw</code></td>
            <td>Changes the word the cursor is on from the cursor to the lowercase w end of the word.(?)</td>
        </tr>
        <tr>
            <td><code>r</code></td>
            <td>Replaces the character under the cursor (<code>vi</code> leaves the insert mode)</td>
        </tr>
        <tr>
            <td><code>R</code></td>
            <td>Overwrites multiple characters beginning with the character currently under the cursor</td>
        </tr>
        <tr>
            <td><code>s</code></td>
            <td>Replaces the current character with the character you type (<code>vi</code> remains in the insert mode)</td>
        </tr>
        <tr>
            <td><code>S</code></td>
            <td>Deletes the line the cursor is on and replaces it with the new text (<code>vi</code> remains in the insert mode)</td>
        </tr>
    </tbody>
</table>


##### Copy and Paste Commands

<table>
    <thead>
        <tr>
            <th>Command</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><code>yy</code></td>
            <td>Copies the current line</td>
        </tr>
        <tr>
            <td><code>yw</code></td>
            <td>Copies the current word from the character the lowercase w cursor is on, until the end of the word (?)</td>
        </tr>
        <tr>
            <td><code>p</code></td>
            <td>Pastes the copied text after the cursor</td>
        </tr>
        <tr>
            <td><code>P</code></td>
            <td>Pastes the copied text before the cursor</td>
        </tr>
    </tbody>
</table>


#### Advanced Commands

##### Search Words and Characters

* There are two kinds of searches in `vi`:  ***string*** and ***character***.

* For string searches:

  * The `/` command searches downwards in the file.

  * The `?` command searches upwards in the file.

  * The `n` and `N` commands repeat the previous search command in the same or the opposite direction, respectively.
* Some characters have special meanings. These characters must be preceded by a backslash (`\`) to be included as part of the search expression.

<table>
    <thead>
        <tr>
            <th>Character</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><code>^</code></td>
            <td>The beginning of a line</td>
        </tr>
        <tr>
            <td><code>.</code></td>
            <td>Matches a single character</td>
        </tr>
        <tr>
            <td><code>*</code></td>
            <td>Matches zero or more of the previous character</td>
        </tr>
        <tr>
            <td><code>$</code></td>
            <td>The end of a line</td>
        </tr>
        <tr>
            <td><code>[</code></td>
            <td>Starts a set of matching or non-matching expressions</td>
        </tr>
        <tr>
            <td><code>&lt</code></td>
            <td>This is put in an expression escaped with the backslash to find the ending or the beginning of a word</td>
        </tr>
        <tr>
            <td><code>&gt</code></td>
            <td>Helps see the <code>&lt</code> character</td>
        </tr>
    </tbody>
</table>


* For character searches:
  * The `f` and `F` commands search for the character after the cursor on the current line.
    * `f` searches forwards.
    * `F` searches backwards.
    * The cursor moves to the position of the found character
  * The `t` and `T` commands also search for a character on the current line.
    * `t` searches forwards; the cursor moves to the position just before the found character.
    * `T` searches backwards; the cursor moves to the position just after the found character.

##### Set Commands

<table>
    <thead>
        <tr>
            <th>Command</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><code>:set ic</code></td>
            <td>Ignores the case when searching</td>
        </tr>
        <tr>
            <td><code>:set ai</code></td>
            <td>Sets autoindent</td>
        </tr>
        <tr>
            <td><code>:set noai</code></td>
            <td>Unsets autoindent</td>
        </tr>
        <tr>
            <td><code>:set nu</code></td>
            <td>Displays lines with line numbers</td>
        </tr>
        <tr>
            <td><code>:set sw</code></td>
            <td>Sets the width of a software tabstop</td>
        </tr>
        <tr>
            <td><code>:set ws</code></td>
            <td>If wrapscan is set, and the word is not found at the bottom of the file, it will try searching for it at the beginning</td>
        </tr>
        <tr>
            <td><code>:set wm</code></td>
            <td>If this option has a value greater than zero, the editor will automatically "word wrap"</td>
        </tr>
        <tr>
            <td><code>:set ro</code></td>
            <td>Changes file type to "read only"</td>
        </tr>
        <tr>
            <td><code>:set term</code></td>
            <td>Prints terminal type</td>
        </tr>
        <tr>
            <td><code>:set bf</code></td>
            <td>Discards control characters from input</td>
        </tr>
    </tbody>
</table>


##### Run Commands

* To run commands from within the editor, goes to the command mode and type `:!` command. For example: `:! ls`.
* To return to the `vi` session, presses any key.

##### Replace Text

```bash
:s/search/replace/g # The g stands for globally
```

## Shell Programming

### Shell Scripting

#### Shell Types

* In Unix,  there are two major types of shells:
  * ***Bourne shell:*** The `$` character is the default prompt.
    * Bourne shell (sh)
    * Korn shell (ksh)
    * Bourne Again shell (bash)
    * POSIX shell (sh)
  * ***C shell:*** The `%` character is the default prompt
    * C shell (csh)
    * TENEX/TOPS C shell (tcsh)
* Bourne shell was the first shell to appear on Unix systems.
* Bourne shell is usually installed as `/bin/sh` on most versions of Unix.

#### Shell Scripts

* To alert the system that a shell script is being started, uses the ***shebang*** construct.

* Example:

```bash
#!/bin/bash
ls -l
```

* To make the script executable:

```bash
$ chmod +x sample_script.sh
```

* To execute the script:

```bash
$ ./sample_script.sh
```

### Variables

#### Name

* Can contain only letters, numbers  or the underscore character.
* In UPPERCASE by convention.

```bash
VAR_1
VAR_2
_LXT
```

#### Types

* Local Variables
  * Present within the current instance of the shell.
  * Not available to programs started by the shell.
  * Set at the command prompt.
* Environment Variables
  * Available to any child process of the shell.
  * Needed by some programs in order to function correctly.
* Shell Variables
  * Special variables set by the shell that and required by the shell in order to function correctly.
  * Can be environment variables or local variables.

#### Define Variables

* Syntax: `variable_name=variable_value`
* Example: `CITY="Hanoi"`

#### Access Variables

* To access a  variable, prefix its name with `$`.
* Example:

```bash
#!/bin/bash

CITY="Hanoi"
echo $CITY
```

#### Read-only Variables

* To mark variables as read-only, uses the `readonly` command.
* Example:

```bash
#!/bin/bash

CITY="Hanoi"
readonly CITY
CITY="New York" # Failed
```

#### Unset Variables

* To unset or delete a variable from the list of variables tracked by the shell, uses the `unset` command.
* Example:

```bash
#!/bin/bash

CITY="Hanoi"
unset CITY
echo $CITY # Prints nothing
```

### Special variables

<table>
    <thead>
        <tr>
            <th>Variable</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><code>$0</code></td>
            <td>The current script's name</td>
        </tr>
        <tr>
            <td><code>$n (n = 1, 2, etc.)</code></td>
            <td>These variables correspond to the arguments with which a script was invoked.</td>
        </tr>
        <tr>
            <td><code>$#</code></td>
            <td>The number of arguments supplied to a script</td>
        </tr>
        <tr>
            <td><code>$*</code></td>
            <td>All the arguments (double quoted)</td>
        </tr>
        <tr>
            <td><code>$@</td>
            <td>All the arguments (double quoted individually)</td>
        </tr>
        <tr>
            <td><code>$?</code></td>
            <td>The exit status of the last executed command</td>
        </tr>
        <tr>
            <td><code>$$</code></td>
            <td>The process ID of the current shell</td>
        </tr>
        <tr>
            <td><code>$!</code></td>
            <td>The process ID of the last background command</td>
        </tr>
    </tbody>
</table>

```bash
#!/bin/bash

echo "Script name: $0"
echo "The 1st argument: $1"
echo "The 2nd argument: $2"
echo "Quoted values: $*"
echo "Quoted values: $@"
echo "Number of argument: $#"

# ./sample_script.sh Taylor Swift
```

### Arrays

####  Define Arrays

* Syntax: `array_name=(value_1 ... value_n)`
* Example:

```bash
#!/bin/bash

SONGS=("All Too Well" "august", "Blank Space", "Enchanted", "Style", "tis the damn season")
```

#### Access Arrays

* To access an item: `${array_name[index]}`
* To access all items: `${array_name[*]}`, or `${array_name[@]}`
* Example:

```bash
#!/bin/bash

SONGS=("All Too Well" "august", "Blank Space", "Enchanted", "Style", "tis the damn season")
echo "The best song of Taylor Swift: ${SONGS[0]}"
echo "The second best song of Taylor Swift: ${SONGS[1]}"
echo "The best songs of Taylor Swift: ${SONGS[*]}"
```

### File Test Operators

<table>
    <thead>
        <tr>
            <th>Operator</th>
            <th>Description</th>
            <th>Example</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><code>-b &ltfilename&gt</code></td>
            <td>Checks if a file is a block special file</td>
            <td><code>[ -b $file ]</code></td>
        </tr>
        <tr>
            <td><code>-c &ltfilename&gt</code></td>
            <td>Checks if a file is a character special file</td>
            <td><code>[ -c $file ]</code></td>
        </tr>
        <tr>
            <td><code>-d &ltfilename&gt</code></td>
            <td>Checks if a file is a directory</td>
            <td><code>[ -d $file </code>]</td>
        </tr>
        <tr>
            <td><code>-f &ltfilename&gt</code></td>
            <td>Checks if a file is an ordinary file as opposed to a directory or special file</td>
            <td><code>[ -f $file ]</code></td>
        </tr>
        <tr>
            <td><code>-g &ltfilename&gt</code></td>
            <td>Checks if a file has its Set Group ID (SGID) bit set</td>
            <td><code>[ -g $file ]</code></td>
        </tr>
        <tr>
            <td><code>-k &ltfilename&gt</code></td>
            <td>Checks if a file has its sticky bit set</td>
            <td><code>[ -k $file ]</code></td>
        </tr>
        <tr>
            <td><code><code>-p &ltfilename&gt</code></td>
            <td>Checks if a file is a named pipe</td>
            <td><code>[ -p $file ]</code></td>
        </tr>
        <tr>
            <td><code>-t &ltfilename&gt</code></td>
            <td>Checks if a file descriptor is open and associated with a terminal</td>
            <td><code>[ -t $file ]</code></td>
        </tr>
        <tr>
            <td><code>-u &ltfilename&gt</code></td>
            <td>Checks if file has its Set User ID (SUID) bit set</td>
            <td><code>[ -u $file ]</code></td>
        </tr>
        <tr>
            <td><code>-r &ltfilename&gt</code></td>
            <td>Checks if a file is readable</td>
            <td><code>[ -r $file ]</code></td>
        </tr>
        <tr>
            <td><code>-w &ltfilename&gt</code></td>
            <td>Checks if a file is writable</td>
            <td><code>[ -w $file ]</code></td>
        </tr>
        <tr>
            <td><code>-x &ltfilename&gt</code></td>
            <td>Checks if a file is executable</td>
            <td><code>[ -x $file ]</code></td>
        </tr>
        <tr>
            <td><code>-s &ltfilename&gt</code></td>
            <td>Checks if a file has size greater than 0</td>
            <td><code>[ -s $file ]</code></td>
        </tr>
        <tr>
            <td><code>-e &ltfilename&gt</code></td>
            <td>Checks if a file exists</td>
            <td><code>[ -e $file ]</code></td>
        </tr>
    </tbody>
</table>

```bash
~!/bin/bash
FILE="/home/chungphb/sample/sample.txt"

# Checks if a file exists
if [ -d $FILE ]
then
	echo "File exists"
else
	echo "File not exists"

# Checks if a file is a directory
if [ -d $FILE ]
then
	echo "File is a directory"
else
	echo "File is not a directory"

# Checks if a file is readable
if [ -r $FILE ]
then
	echo "File is readable"
else
	echo "File is not readable"
```

### Decision Making

#### `if...else` statements

##### `if...fi ` statement

* Syntax:

```bash
if [ expression ] 
then 
   Statement(s) to be executed if expression is true 
fi
```

* Example:

```bash
#!/bin/bash

VAR_1=4
VAR_2=25

if [ $VAR_1 == $VAR_2 ]
then
	echo "$VAR_1 is equal to $VAR_2"
fi

if [ $VAR_1 != $VAR_2 ]
then
	echo "$VAR_1 is not equal to $VAR_2"
fi
```

##### `if...else...fi` statement

* Syntax:

```bash
if [ expression ]
then
   Statement(s) to be executed if expression is true
else
   Statement(s) to be executed if expression is not true
fi
```

* Example:

```bash
#!/bin/bash

VAR_1=4
VAR_2=25

if [ $VAR_1 == $VAR_2 ]
then
	echo "$VAR_1 is equal to $VAR_2"
else
	echo "$VAR_1 is not equal to $VAR_2"
fi
```

##### `if...elif...else...fi` statement

* Syntax:

```bash
if [ expression 1 ]
then
   Statement(s) to be executed if expression 1 is true
elif [ expression 2 ]
then
   Statement(s) to be executed if expression 2 is true
elif [ expression 3 ]
then
   Statement(s) to be executed if expression 3 is true
else
   Statement(s) to be executed if no expression is true
fi
```

* Example:

```bash
#!/bin/bash

VAR_1=4
VAR_2=25

if [ $VAR_1 == $VAR_2 ]
then
	echo "$VAR_1 is equal to $VAR_2"
elif [ $VAR_1 -gt $VAR_2 ]
	echo "$VAR_1 is greater than $VAR_2"
elif [ $VAR_1 -lt $VAR_2 ]
	echo "$VAR_1 is less than $VAR_2"
else
	echo "Something went wrong"
fi
```

#### `case...esac` statement

* Syntax:

```bash
case word in
	pattern1)
		Statement(s) to be executed if pattern1 matcheS
		;;
	pattern2)
		Statement(s) to be executed if pattern2 matches
		;;
	pattern3)
		Statement(s) to be executed if pattern3 matches
		;;
	*)
		Statement(s) to be executed by default
		;;
esac
```

* Example:

```bash
#!/bin/bash

STAGES_OF_GRIEF="Acceptance"

case "$STAGES_OF_GRIEF" in
	"Denial")
		echo "Watch HOANG HON"
		;;
	"Anger")
		echo "Watch VO NAT"
		;;
	"Bargaining")
		echo "Watch THI DU"
		;;
	"Depression")
		echo "Watch HA NOI"
		;;
	"Acceptance")
		echo "Watch MUA HE"
		;;
	*)
		echo "Subscribe"
		;;
esac
```

```bash
#!/bin/bash

OPTION="${1}"

case ${OPTION} in
	-f)
        FILE="${2}"
        echo "File name is $FILE"
        ;;
	-d)
        DIR="{2}"
        echo "Dir name is $DIR"
        ;;
	*)
        echo "`basename ${0}`: usage: [-f file] | [-d directory]"
        exit 1
        ;;
esac
```

### Loops

#### The `while` loop

* Syntax:

```bash
while condition
do
   Statement(s) to be executed if condition is true
done
```

* Example:

```bash
#!/bin/bash

VAR=4

while [ $VAR -gt 0 ]
do
	echo $VAR
	VAR=`expr $VAR - 1`
done
```

#### The `for` loop

* Syntax:

```bash
for var in value_1 value_2 ... value_N
do
   Statement(s) to be executed for every value
done
```

* Example:

```bash
#!/bin/bash

for VAR in 4 12 13 25
do
	echo $VAR
done
```

```bash
#!/bin/bash

for FILE in $HOME/sample/sample*
do
	echo $FILE
done
```

#### The `util` loop

* Syntax:

```bash
until condition
do
   Statement(s) to be executed until condition is true
done
```

* Example:

```bash
#!/bin/bash

VAR=4

until [ $VAR -lt 0 ]
do
	echo $VAR
	VAR=`expr $VAR - 1`
done
```

#### The `select` loop

* Usage: Provides an easy way to create a numbered menu from which users can select options.
* Syntax:

```bash
select var in value_1 value_2 ... value_N
do
   Statement(s) to be executed for every value
done
```

* Example:

```bash
#!/bin/bash

select ALBUM in Red 1989 folklore evermore Fearless reputation
do
	case $ALBUM in
		Red|Fearless)
			echo "Country"
			;;
		1989|reputation)
			echo "Pop"
			;;
		folklore|evermore)
			echo "Folk"
			;;
		*)
			echo "Invalid option"
			;;
	esac
done
```

#### The `break` statement

* Syntax:

```bash
# Exits the loop
break

# Exits the nested loop
break N
```

* Example:

```bash
#!/bin/bash

VAR=4

while [ $VAR -gt 0 ]
do
	echo $VAR
	if [ $VAR -eq 2 ]
	then
		break
	fi
	VAR=`expr $VAR - 1`
done
```

#### The `continue` statement

* Syntax:

```bash
# Skips the current iteration of the loop
continue

# Skips the current iteration of the loop
continue N
```

* Example:

```bash
#!/bin/bash

VAR=4

while [ $VAR -gt 0 ]
do
	VAR=`expr $VAR - 1`
	if [ $VAR -gt 2 ]
	then
		continue
	fi
	echo $VAR
done
```

### Substitution

#### What is Substitution?

* Happened when the shell encounters an expression that contains one or more special characters.
* Options:
  * Uses `-e` option to enable the interpretation of backslash escapes.
  * Uses `-E` option to disable the interpretation of the backslash escapes.
  * Uses `-n` option to disable the insertion of a new line.
* Escapes:

<table>
    <thead>
        <tr>
            <th>Escape</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><code>\\</code></td>
            <td>Backlash</td>
        </tr>
        <tr>
            <td><code>\a</code></td>
            <td>Alert</td>
        </tr>
        <tr>
            <td><code>\b</code></td>
            <td>Backspace</td>
        </tr>
        <tr>
            <td><code>\c</code></td>
            <td>Suppress trailing newline</td>
        </tr>
        <tr>
            <td><code>\f</code></td>
            <td>Form feed</td>
        </tr>
        <tr>
            <td><code>\n</code></td>
            <td>New line</td>
        </tr>
        <tr>
            <td><code>\r</code></td>
            <td>Carriage return</td>
        </tr>
        <tr>
            <td><code>\t</code></td>
            <td>Horizontal tab</td>
        </tr>
        <tr>
            <td><code>\v</code></td>
            <td>Vertica tab</td>
        </tr>
    </tbody>
</table>

* Example:

```bash
#!/bin/bash

VAR=4
echo -e "Value of a is $VAR\n"
```

#### Command Substitution

* The mechanism by which the shell performs a given set of commands and then substitutes their output in the place of the commands.
* Syntax: ``  `command` ``
* Example:

```bash
#!/bin/bash

DATE=`date`
echo "Date: $DATE"
```

#### Variable Substitution

* Enables the shell programmer to manipulate the value of a variable based on its state.
* Forms:

<table>
    <thead>
        <tr>
            <th>Form</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><code>${var}</code></td>
            <td>Substitutes the value of <code>var</code>.</td>
        </tr>
        <tr>
            <td><code>${var:-word}</td>
            <td>
                If <code>var</code> is null or unset, word is substituted for <code>var</code>.<br/>
                The value of <code>var</code> does not change.
            </td>
        </tr>
        <tr>
            <td><code>${var:=word}</td>
            <td>If <code>var</code> is null or unset, <code>var</code> is set to the value of word.</td>
        </tr>
        <tr>
            <td><code>${var:?message}</td>
            <td>If <code>var</code> is null or unset, <code>message</code> is printed to standard error.</td>
        </tr>
        <tr>
            <td>${var:+word}</td>
            <td>
                If <code>var</code> is set, word is substituted for <code>var</code>.</br>
        		The value of <code>var</code> does not change.
            </td>
        </tr>
    </tbody>
</table>

### Quoting Mechanism

#### The Metacharacters

* The shell provides various metacharacters with special meaning, including: `* ? [ ] ' " \ $ ; & ( ) | ^ < > new-line space tab`.
* Forms:

<table>
    <thead>
        <tr>
            <th>Quoting</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Single quote</td>
            <td>All special characters between these quotes lose their special meaning.</td>
        </tr>
        <tr>
            <td>Double quote</td>
            <td>Most special characters between these quotes lose their special meaning except for: <code>$ ` \$ \` \" \'"' \\</code>.</td>
        </tr>
        <tr>
            <td>Backslash</td>
            <td>Any special character immediately following the backslash loses its special meaning.</td>
        </tr>
        <tr>
            <td>Back quote</td>
            <td>Anything in between back quotes would be treated as a command and would be executed.</td>
        </tr>
    </tbody>
</table>

* Example:

```bash
#!/bin/bash

# Uses backlash 
echo tis\;the\;damn\;season
```

#### The Single Quotes

```bash
#!/bin/bash

# Uses single quotes
echo '<-$400.**>; (update?) [y|n]'
```

#### The Double Quotes

```bash
#!/bin/bash

# Uses double quotes
VAR=PRADA
echo "$VAR owes <-\$1500.**>; [ as of (`date +%m/%d`) ]
```

#### The Back Quotes

```bash
#!/bin/bash

# Uses back quotes
DATE=`date`
echo "Today: $DATE"
```

### I/O Redirection

#### Output Redirection

* The output from a command can be easily diverted to a file instead of the standard output.
* Syntax:
  * `$ command > filename`
    * Writes the output into a file. 
    * If the file already contains some data, that data will be lost.
  * `$ command >> filename`
    * Appends the output into an existing file.
* Example: 

```bash
$ who > users
$ echo chungphb >> users
```

#### Input Redirection

* The commands that normally take their input from the standard input can have their input redirected from a file.
* Syntax: `$ command < filename`
* Example:

```bash
$ wc -l < users
```

#### Here Document

* Used to redirect input into an interactive shell script or program.
* Syntax:

```bash
$ command << delimiter
document
delimeter
```

* Example:

```bash
$ wc -l << EOF
	I just want you to know
	This is me trying
	EOF
```

#### Discard the Output

* To discard the output, redirecs it to the file `/dev/null`:

```bash
$ command > /dev/null
```

* To discard both the output of a command and its error output, uses standard redirection to redirect ***STDERR*** to ***STDOUT***:

```bash
$ command > /dev/null 2>&1 # 0 = STDIN, 1 = STDOUT, 2 = STDERR
```

### Redirection Commands

<table>
    <thead>
        <tr>
            <th>Command</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><code>pgm &gt file</td>
            <td>Output of program <code>pgm</code> is redirected to a file</td>
        </tr>
        <tr>
            <td><code>pgm &lt file</td>
            <td>Program <code>pgm</code> reads its input from a file</td>
        </tr>
        <tr>
            <td><code>pgm &gt&gt file</td>
            <td>Output of <code>pgm</code> is appended to a file</td>
        </tr>
        <tr>
            <td><code>n &gt file</code></td>
            <td>Output from stream with descriptor <code>n</code> redirected to file</td>
        </tr>
        <tr>
            <td><code>n &gt&gt file</code></td>
            <td>Output from stream with descriptor <code>n</code> appended to file</td>
        </tr>
        <tr>
            <td><code>n &gt& m</code></td>
            <td>Merges output from stream <code>n</code> with stream <code>m</code></td>
        </tr>
        <tr>
            <td><code>n &lt& m</code></td>
            <td>Merges input from stream <code>n</code> with stream <code>m</code></td>
        </tr>
        <tr>
            <td><code>&lt&lt tag</code></td>
            <td>Standard input comes from here through next tag at the start of line</td>
        </tr>
        <tr>
            <td>|</td>
            <td>Takes output from one program or process and sends it to another</td>
        </tr>
    </tbody>
</table>

### Functions

#### Create Functions

* Syntax:

```bash
function_name () {
	list of commands
}
```

* Example:

```bash
#!/bin/bash

# Defines a function
Today () {
	DATE=`date`
	echo "Today: $DATE"
}

# Passes parameters to a function
Greet () {
	echo "Hello $1"
}

# Returns from a function
Add () {
	RES=`expr $1 + $2`
	return $RES
}

# Invokes
Today
Greet ChungPHB
Add 1 2
RET=$?
echo "Return value is $RET"
```

#### Unset Functions

* Executing a file in a shell causes functions defined inside of it to be read and defined to the shell as well.

* To remove the definition of a function from the shell, uses the unset command with the `-f` option.

```bash
$ unset -f function_name
```

## Advanced Unix / Linux
