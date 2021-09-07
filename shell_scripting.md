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
            <td>DISPLAY</td>
            <td>The identifier for the display</td>
        </tr>
        <tr>
            <td>HOME</td>
            <td>The home directory of the current user</td>
        </tr>
        <tr>
            <td>LANG</td>
            <td>The default system locale</td>
        </tr>
        <tr>
            <td>LD_LIBRARY_PATH</td>
            <td>A colonseperated list of directories that the dynamic linker should search for shared objects</td>
        </tr>
        <tr>
            <td>PATH</td>
            <td>The search path for commands</td>
        </tr>
        <tr>
            <td>PWD</td>
            <td>The current working directory</td>
        </tr>
        <tr>
            <td>RANDOM</td>
            <td>A random integer between 0 and 32,767</td>
        </tr>
        <tr>
            <td>TERM</td>
            <td>The display type</td>
        </tr>
        <tr>
            <td>TZ</td>
            <td>Time zone</td>
        </tr>
        <tr>
            <td>UID</td>
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
            <td>vi filename</td>
            <td>Creates a new file if it does not exist, otherwise opens an existing file</td>
        </tr>
        <tr>
            <td>vi -R filename</td>
            <td>Opens an existing file in the read-only mode</td>
        </tr>
        <tr>
            <td>view filename</td>
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
            <td>k</td>
            <td>Moves the cursor up one line</td>
        </tr>
        <tr>
            <td>j</td>
            <td>Moves the cursor down one line</td>
        </tr>
        <tr>
            <td>h</td>
            <td>Moves the cursor to the left one character position</td>
        </tr>
        <tr>
            <td>l</td>
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
            <td>i</td>
            <td>Inserts text before the current cursor location</td>
        </tr>
        <tr>
            <td>l</td>
            <td>Inserts text at the beginning of the current line</td>
        </tr>
        <tr>
            <td>a</td>
            <td>Inserts text after the current cursor location</td>
        </tr>
        <tr>
            <td>A</td>
            <td>Inserts text at the end of the current line</td>
        </tr>
        <tr>
            <td>o</td>
            <td>Creates a new line for text entry below the cursor location</td>
        </tr>
        <tr>
            <td>O</td>
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
            <td>x</td>
            <td>Deletes the character under the cursor location</td>
        </tr>
        <tr>
            <td>X</td>
            <td>Deletes the character before the cursor location</td>
        </tr>
        <tr>
            <td>dw</td>
            <td>Deletes from the current cursor location to the next word</td>
        </tr>
        <tr>
            <td>d^</td>
            <td>Deletes from the current cursor position to the beginning of the line</td>
        </tr>
        <tr>
            <td>d$</td>
            <td>Deletes from the current cursor position to the end of the line</td>
        </tr>
        <tr>
            <td>D</td>
            <td>Deletes from the cursor position to the end of the current line</td>
        </tr>
        <tr>
            <td>dd</td>
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
            <td>cc</td>
            <td>Removes the contents of the line (vi remains in the insert mode)</td>
        </tr>
        <tr>
            <td>cw</td>
            <td>Changes the word the cursor is on from the cursor to the lowercase w end of the word.(?)</td>
        </tr>
        <tr>
            <td>r</td>
            <td>Replaces the character under the cursor (vi leaves the insert mode)</td>
        </tr>
        <tr>
            <td>R</td>
            <td>Overwrites multiple characters beginning with the character currently under the cursor</td>
        </tr>
        <tr>
            <td>s</td>
            <td>Replaces the current character with the character you type (vi remains in the insert mode)</td>
        </tr>
        <tr>
            <td>S</td>
            <td>Deletes the line the cursor is on and replaces it with the new text (vi remains in the insert mode)</td>
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
            <td>yy</td>
            <td>Copies the current line</td>
        </tr>
        <tr>
            <td>yw</td>
            <td>Copies the current word from the character the lowercase w cursor is on, until the end of the word (?)</td>
        </tr>
        <tr>
            <td>p</td>
            <td>Pastes the copied text after the cursor</td>
        </tr>
        <tr>
            <td>P</td>
            <td>Pastes the copied text before the cursorr</td>
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
            <td>^</td>
            <td>The beginning of a line</td>
        </tr>
        <tr>
            <td>.</td>
            <td>Matches a single character</td>
        </tr>
        <tr>
            <td>*</td>
            <td>Matches zero or more of the previous character</td>
        </tr>
        <tr>
            <td>$</td>
            <td>The end of a line</td>
        </tr>
        <tr>
            <td>[</td>
            <td>Starts a set of matching or non-matching expressions</td>
        </tr>
        <tr>
            <td>&lt</td>
            <td>This is put in an expression escaped with the backslash to find the ending or the beginning of a word</td>
        </tr>
        <tr>
            <td>&gt</td>
            <td>This helps see the &lt character description above</td>
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
            <td>:set ic</td>
            <td>Ignores the case when searching</td>
        </tr>
        <tr>
            <td>:set ai</td>
            <td>Sets autoindent</td>
        </tr>
        <tr>
            <td>:set noai</td>
            <td>Unsets autoindent</td>
        </tr>
        <tr>
            <td>:set nu</td>
            <td>Displays lines with line numbers</td>
        </tr>
        <tr>
            <td>:set sw</td>
            <td>Sets the width of a software tabstop</td>
        </tr>
        <tr>
            <td>:set ws</td>
            <td>If wrapscan is set, and the word is not found at the bottom of the file, it will try searching for it at the beginning</td>
        </tr>
        <tr>
            <td>:set wm</td>
            <td>If this option has a value greater than zero, the editor will automatically "word wrap"</td>
        </tr>
        <tr>
            <td>:set ro</td>
            <td>Changes file type to "read only"</td>
        </tr>
        <tr>
            <td>:set term</td>
            <td>Prints terminal type</td>
        </tr>
        <tr>
            <td>:set bf</td>
            <td>Discards control characters from input</td>
        </tr>
    </tbody>
</table>

##### Run Commands

* To run commands from within the editor, goes to the command mode and type `:!` command. For example: `:! ls`.
* To return to the `vi` session, presses any key.

##### Replace Text

```bash
:s/search/replace/g		# The g stands for globally
```

## Shell Programming

## Advanced Unix / Linux
