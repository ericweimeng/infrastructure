# Kernel

### Hardware

```
uname -> display info about currrent running kernel
uname -m
uname -rm
uname -a

lsmod -> list modules

modinfo -> dsiplay module info

modprobe -> load/unload module at runtime
modprobe -r <module_name>

/dev -> info about all connected hardwares
udev -> device manager for linux kernel

D-Bus
Sends data message between apps
```

### Boot the system

```
dmesg -> view kernel ring buffer

journalctl -k -> systemd utility to view the kernelring buffer
```
