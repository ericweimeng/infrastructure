## File System

#### root file systems
- /bin - contains executable program that regular users on system can run. This is where your 'cd', 'ls'... commands located (一般存放os启动即会用到的程序。这个目录不能被关联至独立分区)

- /boot - contains files that necessary to get the computer booted up. The linux kernel itself also resides here.

- /dev - a place where all devices are referenced from. Including hard drive, keyboard, usb, sound cards...

- /etc - contains configuration files for most of the system services and system information (usually text file)

- /home - user's own folder. for example like you download something from internet, it will be store in this directory.

- /lib (32 bit) and /lib64 (64 bit) - for library files that contain code which has been shared by applications (基本共享库， 以及内核模块文件 /lib/modules)

- /media Mount point for removable media

- /mnt - any hard drive that connect to your system (Mount point for a temporarily mounted filesystem)

- /opt - optional location for applications stored if they are not be located at bin directory

- /proc - contains information about a running linux system, the linux system output information that many applications use is in this directory (virtual)

- /root - home directory for root user.

- /sbin - location where system administrator tools and programs are located, typically, only root user has access to it. (一般存放os启动即会用到的程序。这个目录不能被关联至独立分区)

- /srv - for server applications, such as web servers

- /sys - information about hardware on system (virtual)

- /tmp - store information for temporary data

- /usr - universal shared, read-only. own set of directory tree, that closely mirror the root directory, you can find more applications that stored here along with system documentation and other configuration files

- /var contains variable data files like log files, printer files and local system email files

