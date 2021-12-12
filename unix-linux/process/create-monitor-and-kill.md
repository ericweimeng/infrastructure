---
description: Process commands
---

# Create, Monitor and Kill



```
ps

ps -eH | less // -H hierachy
ps -e --forest
ps -efH // -f format

top
press k key with pid to kill a process in top table

uptime // view how long the system has been up, how many users are logged in, and CPU load average

free // used and available memory and swap space
free -m // megabytes
free -g //gigbytes

pgrep // find process information based on process name
pgrep -a httpd // -a list all info
pgrep -u [user] // -u user

kill -l
pkill // Send a signal(usually SIGTERM) to a process based on process name
pkill -x [process name]// only match this process name
killall -s [process name] // -s signal

Watch // Runs a command at specified intervals. Used to monitor a command's output.
watch -n 5 date

screen // A ternimal window manager that allows you to run commands in an isolated session.

tmux // a modern terminal window manager (like screen) with extra features.

man proc
man signal
```
