
`ls -l` 

Long listing, showing file and directory and ownership, permissions and sizes.

`ls -lh`

The 'h' shows the size in human readable format, must be used with 'l' switch

`ls -a` 

Show all files and folders, including hidden ones

`ls -R`

List directories recursively, generally used when you are not sure about the location of a file within a directory, and what to generally scout the structure of the directory.

`ls -S`

Sort files by size with the largest at the top

`ls -t`

Sort by last time modified displaying the newest first, most useful with the -l switch

`env`

List all of the environment variable set for the current shell environment

`PATH`

The PATH environment variable contains a list of all of the directories that Bash will look in for applications or scripts to run

`echo`

Print what follows the screen.

`echo $PATH`

Print the contents of the PATH environment variable to the screen.

`echo $VARIABLE`

Prints the value of  VARIABLE to the screen

`set`

Lists out all environment variables in alphabetical order

`VARIABLENAME=value' 

Format for declaring a new variable in Bash

`export VARIABLE`

Exports variable and its value to other shells

`which`

Display the full location of the application that was installed.

Linux and Linux like os are case sensitive.

`Whoami`

Display your currently logged in user

`su`

Substitute user, change to another user account on the system

`exit`

Leave a shell environment that you are logged into

`int 6`

Legacy command for rebooting system

`int 0`

Legacy command for shutting a system down

`halt, poweroff`

shuts down (halts) a system

`shutdown`

Can be used to power off, reboot or stop a pending shutdown request

`top`

Interactively display top running process on a system.

`uname`

Display the name in the system kernel.
-r display the kernel release number.
-v display the kernel build version.
-m display the machine type
-o display the name of the operating system
-a display all information that uname can show

`cd -`

return to last directory

`cd ~ / cd`

Change to the home directory of the current logged in user

.bash_history -> hidden file within the home directory that contains a log of commands entered at the Bash prompt.

HISTFILESIZE -> environment variable that specifies how many lines of history to keep

HISTCONTROL -> environment variable that modifies Bash's history behavior

`history`

Command that prints out commands saved in .bash_history with each command numbered

`!<history number>` 
Re-runs command from .bash_history.

`/etc/bashrc`

System-wide functions and aliases

`/etc/profile`

System-wide environment and startup programs, used during a login shell

`/etc/profile.d/`

location of extra environment setup scripts

**The following files are in the home directory of the user(note that not all distributions will use all of these files)**

`.bash_profile`

Used to set user specific shell environment preferences

`.bash_logout`

Ran when the user logs out of a login shell. not a terminal, does not exist on every system

`.bashrc`

Non-login file that stores user specific functions and aliases.

[Login & Non-login shell](https://unix.stackexchange.com/questions/38175/difference-between-login-shell-and-non-login-shell)

**The Bash Configuration File Order - Login Shell**
 During a login shell, these configuration script files are called in this order:
* /etc/profile
* Then the order branches out, the first file that is found is used, all of the others are ignored even if they exist:
    * ~/.bash_profile
    * ~/.bash_login
    * ~/.profile
    Next, .bashrc is called, followed bu /etc/bashrc

`/etc/profile => ~/.bash_profile => ~/.bashrc => /etc/bashrc`

**Globbing**

\* - Matches zero or more charactors

? - Matches any single charactor

[abc] - Matches any one of the characters in the list, case sensitive

[^abc] - Matches any one characters except those in the list, case sensitive

[0-9] - Matches any range of numbers

**Quoting**

"" - Double quotes, contains string and any variables or commands within them get evaluated or acted on

'' - Single quotes, anything within these gets treated literally, disables any special character functionality

\ - backslash, escape character, disables any special character functionality that immediately follows it

Quotes around spaces or an escape character preceding a space will be treated literally

`locate`

Searches a local database of files and folders looking for items that match the search criteria
like: locate cd

`find`

Searches the file system for files that matches the search criteria. Like: find /path/to/folder -name file
When using the find command to search for part of a file name, use globbing within single quotes: find /path/to/folder -name '*file*'

`whereis`

Locates binary, source and/or manual pages for a command.

`man`

Manual pages command, invoked by entering: man <command>

`whatis`

Command that lists summaries and related man pages based on search term, invoke by entering: whatis <command>
Some results can be obtained by: man -f <command>

`apropos`

Command that searches man page for appearances of keyword provided, invoked by entering: apropos <keyword>
Same results can be obtained by: man -k <keyword>

Arrow keys and vi key bindings can be used to navigate the man pages, Pressing the 'q' key will exit the man page.

`info`

information utility command, invoked by entering: info <command>

Arrow keys can be used to navigate the info pages. placing the cursor over a node link and pressing the 'enter' will take you to the selected node page. Pressing the 'q' key will exit the info utility.

**More local documentation**

Common local documentation directories (varies by distribution):

`/usr/doc/packagename`
`/usr/share/docs/packagename`
`/usr/share/docs/packages/packagename`

Files here are in plain text, and an be regular text files or html files that can be viewed in a web browser.

**Linux File System**

/bin - contains executable program that regular users on system can run. This is where your 'cd', 'ls'... commands located

/boot - contains files that necessary to get the computer booted up. The linux kernel itself also resides here.

/dev - a place where all devices are referenced from. Including hard drive, keyboard, usb, sound cards...

/etc - contains configuration files for most of the system services and system information

/home - user's own folder. for example like you download something from internet, it will be store in this directory.

/lib (32 bit) and /lib64 (64 bit) - for library files that contain code which has been shared by applications
/mnt - connect any hard drive that connect to your system

/opt - optional location for applications stored if they are not be located at bin directory

/proc - contains information about a running linux system, the linux system output information that many applications use is in this directory.

/root - home directory for root user.

/sbin - location where system administrator tools and programs are located, typically, only root user has access to it.

/srv - for server applications, such as web servers

/sys - information about hardware on system

/tmp - store information for temporary data

/usr - own set of directory tree, that closely mirror the root directory, you can find more applications that stored here along with system documentation and other configuration files

/var contains files like log files, printer files and local system email files

`mkdir`

Make a new directory. -p make a parent directory along with a subdirectory. e.p. mkdir -p art/painting/modern

`rmdir`

Remove an empty directory

`touch`

Create an empty file or update a file's timestamp

`cp`

Copy a file or folder
-R copy a folder recursively
-v Verbose, display what the copy command is doing

`mv`

Move or rename a file or folder

`rm`

Remove a file or folder. -r recursively remove a folder and its contents.

**Archive & Compression**

`tar`

-c Create a new archive

-t List what inside an archive

-z Pass the archive through gzip
compression

-j Pass the archive through bzip2 compression

-f File name of the archive to create

-x extract an archive

-v verbose output

`zip`

Create a new compressed file

-r Recursively create a new compressed file of directory and its contents

`unzip`

Extract a zip archive

`gzip`

Create a gzip archive

`gunzip`

Extract a gzip archive

`bzip2`

Create a bzip2 archive

`bunzip2`

Extract a bzip2 archive

**Viewing text**

`less`

View a text file with the ability to scroll through the pages of the file

`head`

View the first ten lines of a file

-n <number> => View the first <number> lines of a file specified

`tail`

View the last ten lines of a file

-n <number> = View the last <number> lines of a file specified

-f => follow the text file as new data is written to it in real time

**Analyzing Text** 

`cut`

Remove text from file and print specified fields to screen

-d Specifies delimiter to use

-f Specifies which field to print

`sort`

Sorts content of file alphabetically based on first character in file

-n Sorts content of file numerically

`wc`

word count, prints number of lines, words and characters in file

-l Print number of lines in file

-w Print number of words in file

`>`

Redirects standard output to new location, if output goes to file replaces contents of file with output from stdout

`>>`

Redirects standard output to new location, appends stdout to file

**Pipe and Regular Expressions**

`grep`

Show the lines in a file that match a given pattern

-i Perform a case-insensitive search

-v Return lines that do not contain the pattern

-r Perform a recursive search

`|` Pipe character, used to send output of one command as input to another command. e.g.: command1 | command2

**Regular Expressions**

^ Search the beginning of a line

$ Search the end of a line

. Stands for a single character

[abc] Search for specified characters

[^abc] Search for other characters except for these

* Match zero or more of the preceding characters or expression

\b(string)\b Left and right word boundary

**Shell Scripting**

#!/bind/bash

The 'Shebang', the first line in a bash script that tells bash what scripting language is being used

#

Begins a comment, a line that is ignored in the script but acts as documentation for someone viewing the script contents

Basic 'if' statement: if[something] then do this thing fi

Basic 'if else' statement: if[something] then do this thing else do this other thing instead fi

Basic for loop setup

For variable in item1 item2 item3 do command to execute on $variable done

**Linux Processes**

`ps`

Lists all processes currently running on a system

-u List the processes for specific user

-e List all processes running on the system

-H List all processes with indented output showing the hierarchy

-f Full format listing, including command arguments

`top`

Show statistics on the processor, RAM, and running processes

**Networking in Linux**

`ifconfig `
`ip addr show`

View your IP address configuration settings

127.0.0.1 Local loopback address

`ip route show`
`route`
`netstat -r`

View your default route (gateway) settings

`host`

Test DNS host name resolution, e.g.: host google.com

`ping`

Test network connectivity. e.g.: ping -c 3 linuxacademy.com

`/etc/hosts`

Local name resolution file for the local host and for small networks.

`/etc/resolv.conf` File that contains the IP address(es) of DNS servers the system should contact