## Basics

#### type
Display information about command type.

For each NAME, indicate how it would be interpreted if used as a command name.

#### Get help
- Get built-in commands
	- help COMMAND
- Get external commands
	- help
		- COMMAND --help or COMMAND --h
	- manual -- man
		- man COMMAND
	- info page
		- info COMMAND

#### Everything after you type in a command in shell
- The shell gets the command, scan all the paths in $path variable trying to find one executable program for that command

- All shell programs have its own executable program under a certain system directory, below commands can show your where it is
	- which
	- whereis

- The paths for Shell to search for an executable programs sit in an environment variable  
	- $PATH
	-  All paths in this file are ordered from left right, meaning:
		- if shell found one program in one of the paths, then it would run it directly without searching the rest of paths come after it

- hash
	- The shell would only search executable program for once, the search result will be store in memory as key-value pairs for current shell window. Use 'hash' to check it.
	- [-d] Delete one k-v pair
	- [-r] Delete all k-v pairs

#### History
Commands history for current shell

- The shell will load history file under '~/.bash_history' when user log in shell
- All commands after login will be stored only in memory, but will be appended in '~/.bash_history' file when user logs out
- [-a]: append history lines from this session to the history file
- [-d]: offset delete the history entry at offset OFFSET
- [-c]:	clear the history list by deleting all of the entries
- !#: execute that command
- !string: last command starts with this string
- !! re-execute last command

#### cat
- Concatenate FILE(s), or standard input, to standard output.
- -E: display $ at end of each line
- -n: number all output lines

#### tac
- concatenate and print files in reverse

#### man
- man1 User command
- man2 System call (系统调用)
- man3 C library call (C 库函数调用)
- man4 Device and special files (设备文件和特殊文件)
- man5 Configuration file formate (配置文件格式)
- man6 Game
- man7 杂项
- man8 Management (管理类)
- man configuration file: /etc/man.config
- SYNOPSIS
	- []: options
	- <>: required
	- a|b: or
- Sometime, one command may have multiple implementations, use
	- man # COMMAND
- Operations on man page
	- space, ^V, ^f, ^F: scroll 'one page' downward
	- b, ^B: scroll 'one page' upward
	- d, ^D: scroll 'half page' downward
	- u, ^U: scroll 'half page' upward
	- RETURN, ^N, e, ^E, j, ^E: scroll 'one line' downward
	- y, ^Y, ^P, k, ^K: scroll 'one line' downward
	- q: quit
	- #: jump to # line
	- 1G: back to the top
	- G: go to bottom
	- /KEYWORD:
		- search KEYWORD from current position to the bottom of the file, case insensitive
			- n: next one
			- N: last one
	- ?KEYWORD:
		- search KEYWORD from current position to the top of the file, case insensitive
			- n: next one
			- N: last one
		

#### date
- date [OPTION]...[+FORMAT]: 显示
- date [MMDDhhmm[[CC]YY][.ss]]: 设置
- cal: calendar

#### cd
- cd -: can switch between last and current directory
- $PWD: current directory
- $OLDPWD: last directory

#### ls
- -a: list all including . and ..
- -A: list all except . and ..
- -l: use a long listing format
	- drwxr-xr-x  1 apache apache  864 Oct 30 22:20 insights
		- -rw-r--r--: 
			- first position: file type
				- '-' regular file
				- 'd' directory file
				- 'l' link file
				- 'b' block file (可随机访问)
				- 'c' charator file (有序访问)
				- 'p' pipeline file
				- 's' socket file
			- following 9 digits
				- access permission
			- digit after first 10 digits
				- times for this file being hard-linked
			- after digit
				- first apache: file owner
				- second apache: file group
			- 864: size of the file
			- Oct 30 22:20: last time it was modified
- -h: with -l, print sizes in human readable format (e.g., 1K 234M 2G)
- -d: list directory entries instead of contents, and do not dereference symbolic links
- -r: reverse order while sorting
- -R: list subdirectories recursively
- -t: sort by modification time

#### stat
- display file or file system status (meta info)

#### file
- determine file type

#### which
- shows the full path of (shell) commands

#### whereis
- locate the binary, source, and manual page files for a command

#### whatis
- search the whatis database for complete words. (Command that lists summaries and related man pages based on search term)

#### apropos

- Command that searches man page for appearances of keyword provided, invoked by entering: apropos Same results can be obtained by: man -k
- Arrow keys and vi key bindings can be used to navigate the man pages, Pressing the 'q' key will exit the man page.

#### shutoff
- halt, poweroff, shutdown, init 0

#### reboot
- reboot, shutdown, init 6

#### user login
- who
- whoami
- w

####命令行展开
  - /tmp{bin,sbin,usr/{bin,sbin}}

####上一次命令执行状态结果
- $?

#### 文件查看类
- cat, tac
- more, less, tail, head

#### 文件时间戳管理
- touch

#### metadata, data
- 查看文件状态
	- stat
		- 三个时间戳
			- access time (读取文件内容)
			- modify time (改变文件内容)
			- change time (元数据发生改变)