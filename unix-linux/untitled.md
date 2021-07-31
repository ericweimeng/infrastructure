# Environment Variable

## How to Set Environment Variables in Linux

{% embed url="https://www.serverlab.ca/tutorials/linux/administration-linux/how-to-set-environment-variables-in-linux/" %}

### Overview

In this tutorial, you will learn how to set environment variables in Ubuntu, CentOS, Red Hat, basically any Linux distribution for a single user and globally for all users. You will also learn how to list all environment variables and how to unset \(clear\) existing environment variables.

Environment variables are commonly used within the Bash shell. It is also a common means of configuring services and handling web application secrets.

It is not uncommon for environment specific information, such as endpoints and passwords, for example, to be stored as environment variables on a server. They are also used to set the important directory locations for many popular packages, such as JAVA\_HOME for Java.

### Setting an Environment Variable

To set an environment variable the export command is used. We give the variable a name, which is what is used to access it in shell scripts and configurations and then a value to hold whatever data is needed in the variable.

```text
export NAME=VALUE
```

For example, to set the environment variable for the home directory of a manual OpenJDK 11 installation, we would use something similar to the following.

```text
export JAVA_HOME=/opt/openjdk11
```

To output the value of the environment variable from the shell, we use the echo command and prepend the variable’s name with a dollar \($\) sign.

```text
echo $JAVA_HOME
```

And so long as the variable has a value it will be echoed out. If no value is set then an empty line will be displayed instead.

### Unsetting an Environment Variable

To unset an environment variable, which removes its existence all together, we use the unset command. Simply replace the environment variable with an empty string will not remove it, and in most cases will likely cause problems with scripts or application expecting a valid value.

To following syntax is used to unset an environment variable

```text
unset VARIABLE_NAME
```

For example, to unset the JAVA\_HOME environment variable, we would use the following command.

```text
unset JAVA_HOME
```

### Listing All Set Environment Variables

To list all environment variables, we simply use the set command without any arguments.

```text
set
```

An example of the output would look something similar to the following, which has been truncated for brevity.

```text
BASH=/bin/bash
BASHOPTS=checkwinsize:cmdhist:complete_fullquote:expand_aliases:extglob:extquote:force_fignore:globasciiranges:histappend:interactive_comments:login_shell:progcomp:promptvars:sourcepath
 BASH_ALIASES=()
 BASH_ARGC=([0]="0")
 BASH_ARGV=()
 BASH_CMDS=()
 BASH_COMPLETION_VERSINFO=([0]="2" [1]="8")
 BASH_LINENO=()
 BASH_SOURCE=()
 BASH_VERSINFO=([0]="5" [1]="0" [2]="3" [3]="1" [4]="release" [5]="x86_64-pc-linux-gnu")
 BASH_VERSION='5.0.3(1)-release'
 COLUMNS=208
 DIRSTACK=()
 EUID=1000
 GROUPS=()
 HISTCONTROL=ignoreboth
 HISTFILE=/home/ubuntu/.bash_history
 HISTFILESIZE=2000
 HISTSIZE=1000
 HOME=/home/ubuntu
 HOSTNAME=ubuntu1904
 HOSTTYPE=x86_64
 IFS=$' \t\n'
 LANG=en_US.UTF-8
 LESSCLOSE='/usr/bin/lesspipe %s %s'
 LESSOPEN='| /usr/bin/lesspipe %s'
 LINES=54
```

### Persisting Environment Variables for a User

When an environment variable is set from the shell using the export command, its existence ends when the user’s sessions ends. This is problematic when we need the variable to persist across sessions.

To make an environment persistent for a user’s environment, we export the variable from the user’s profile script.

1. Open the current user’s profile into a text editor  


   ```text
   vi ~/.bash_profile
   ```

2. Add the export command for every environment variable you want to persist.  


   ```text
   export JAVA_HOME=/opt/openjdk11
   ```

3. Save your changes.

Adding the environment variable to a user’s bash profile alone will not export it automatically. However, the variable will be exported the next time the user logs in.

To immediately apply all changes to bash\_profile, use the source command.

```text
source ~/.bash_profile
```

### Export Environment Variable

Export is a built-in shell command for Bash that is used to export an environment variable to allow new child processes to inherit it.

To export a environment variable you run the export command while setting the variable.

```text
export MYVAR="my variable value"
```

We can view a complete list of exported environment variables by running the export command without any arguments.

```text
export
```

```text
SHELL=/bin/zsh
SHLVL=1
SSH_AUTH_SOCK=/private/tmp/com.apple.launchd.1pB5Pry8Id/Listeners
TERM=xterm-256color
TERM_PROGRAM=vscode
TERM_PROGRAM_VERSION=1.48.2
```

To view all exported variables in the current shell you use the `-p` flag with export.

```text
export -p
```

### Setting Permanent Global Environment Variables for All Users

A permanent environment variable that persists after a reboot can be created by adding it to the default profile. This profile is loaded by all users on the system, including service accounts.

All global profile settings are stored under /etc/profile. And while this file can be edited directory, it is actually recommended to store global environment variables in a directory named /etc/profile.d, where you will find a list of files that are used to set environment variables for the entire system.

1. Create a new file under /etc/profile.d to store the global environment variable\(s\). The name of the should be contextual so others may understand its purpose. For demonstrations, we will create a permanent environment variable for HTTP\_PROXY.  


   ```text
   sudo touch /etc/profile.d/http_proxy.sh
   ```

2. Open the default profile into a text editor.  


   ```text
   sudo vi /etc/profile.d/http_proxy.sh
   ```

3. Add new lines to export the environment variables  


   ```text
   export HTTP_PROXY=http://my.proxy:8080
   ```

   ```text
   export HTTPS_PROXY=https://my.proxy:8080
   ```

   ```text
   export NO_PROXY=localhost,::1,.example.com
   ```

4. Save your changes and exit the text editor

