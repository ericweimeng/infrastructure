# Network Trouble Shooting

## Network Environment Interface Tools

* ipconfig \(obsolete\)
* ip addr show
  * lo -&gt; loopback
* ip route show
* routel
* nmcli
  * nmcli d show -&gt; show devices
* ip n -&gt; show neighbors

## Locating the Network Information

### Introduction

Being able to locate the IP address, netmask, gateway, DNS nameserver, and domain is critical to understanding Linux networking. In this hands-on lab, we're going to populate a text file with this information.

### Logging In

Use the credentials provided in the hands-on lab overview page to log into the lab server with SSH.

### Getting Comfortable

Once we're in, we've got to get a few pieces of information added to a `Network.txt` file in our home directory. We're going to use an `nmcli` command each time. We may want to have two terminals open, one to edit the text file \(with whichever text editor we want\) and the other to run commands and get output.

First, let's see if NetworkManager is even running:

```text
[cloud_user@host]$ systemctl status NetworkManager
```

That will show us that is it. Now let's get a list of active connections.

```text
[cloud_user@host]$ nmcli c show
```

We'll see a line for `eth0`. That's the one we're going to be querying.

### Get the Information

This is actually pretty easy. Let's run this one command:

```text
[cloud_user@host]$ nmcli d show eth0
```

### Conclusion

The information we need is in this output. The IP address and netmask \(IPV4.ADDRESS\[1\]\), gateway address \(IPV4.GATEWAY\), DNS server \(IPV4.DNS1\), and DNS domain \(IPV4.DOMAIN\[1\]\) are all there. To get a passing grade on the lab, just get those values from the command output and get them into the proper lines of the `Network.txt` file. Then we're done. Congratulations!

## Setting Static IP

### The Scenario

In preparation for moving hardware to colocation, we're are being tasked with assigning hosts their dynamic IPs as static addresses.

### Configure Static IPs

Check our current networking situation

```text
[cloud_user@host]$ nmcli c
```

This will show us an interface, `eth0`. Let's dig a little further with:

```text
[cloud_user@host]$ nmcli d show eth0
```

With that command, we've got all the details we need to run the actual configuration command. Substitute IP addresses with the correct ones from the last command:

```text
[cloud_user@host]$ nmcli con mod System\ eth0 ipv4.method manual ipv4.addresses 10.0.1.10/24 ipv4.gateway 10.0.1.1 ipv4.dns 10.0.0.2 ipv4.dns-search ec2.internal
```

### Verify That the Address Is Now Set Manually, Not Dynamically

We'll run another command to verify. Look at the `ipv4.method` in the output from this:

```text
[cloud_user@host]$ nmcli con show System\ eth0 | grep ipv4
```

### Restart Networking

Finally, we've got to restart our network and make sure the changes stick:

```text
[cloud_user@host]$ systemctl restart network
```

{% hint style="info" %}
[https://www.digitalocean.com/community/tutorials/how-to-use-systemctl-to-manage-systemd-services-and-units](https://www.digitalocean.com/community/tutorials/how-to-use-systemctl-to-manage-systemd-services-and-units) for more systemd
{% endhint %}

### Conclusion

If we maintained connectivity, and another `nmcli con show System\ eth0 | grep ipv4` shows the IP address we set, then we're done. Congratulations!

## Multiple IPs on the Same Interface

### The Scenario

In preparation for migrating additional services to a new host, we must provision two more IP addresses on it. These IP addresses are `10.0.1.15` and `10.0.1.20`, on the same subnet as the existing IP.

### Get logged in

Use the credentials and server IP in the hands-on lab overview page to log into our lab server. Once we're in, we can get moving.

### Add the Additional IP Address Configuration

First, let's see how things are currently sitting, and get our IP address at the moment:

```text
[cloud_user@host]$ nmcli c
[cloud_user@host]$ nmcli d show eth0
```

The command we can use to add the additional addresses is:

```text
[cloud_user@host]$ sudo nmcli c mod System\ eth0 ipv4.addresses 10.0.1.15/24,10.0.1.20/24
```

To check where things stand now, run:

```text
[cloud_user@host]$ nmcli c show System\ eth0 | grep ipv4
```

### Restart Networking

Once we're confident in the configuration, we can restart networking using:

```text
[cloud_user@host]$ sudo systemctl restart network
```

Now we can run:

```text
[cloud_user@host]$ nmcli
```

This should show that we have the three IP addresses.

### Conclusion

That was it. We just needed to add a couple of IP addresses to our interface, and we're done. Congratulations!

## Creating an Interface Team

### The Scenario

Our supervisor has asked us to prepare a new application server. The primary interface `eth0` has an IP of `10.0.1.11` and will be used for internal management. It should not be modified.

But we need to configured the following other two interfaces, `eth1` and `eth2` to be part of a team:

* The interface team connection name will be `Team0`
* The interface team interface name is `team0`
* `round robin` is the interface team mode we'll use
* It will have a static IP range `10.0.1.15/24`
* We'll use the gateway from DHCP

### Get logged in

Use the credentials and server IP in the hands-on lab overview page to log into our lab server. Once we're in, we can get moving.

### Install teamd

We'll need to make sure the `teamd` package is installed to use teaming. We'll also grab the `bash-completion` package, so that we can avoid having to actually type out all of our commands:

```text
[cloud_user@host]$ sudo yum -y install teamd bash-completion
```

Now we'll run a quick `source /etc/profile` and then we can get moving on the network.

### Configure the Team Interface

There are three interfaces, and we can see them by running `nmcli d`. We're going to keep our `eth0`, but we'll delete the other two, then recreate them:

```text
[cloud_user@host]$ sudo nmcli con delete Wired\ connection\ 1
[cloud_user@host]$ sudo nmcli con delete Wired\ connection\ 2
```

Create the Team Connection:

```text
[cloud_user@host]$ sudo nmcli con add type team con-name Team0 ifname team0
```

Modify the Team Connection to include the static IP and gateway:

```text
[cloud_user@host]$ sudo nmcli con mod Team0 ipv4.address 10.0.1.15/24 ipv4.gateway 10.0.1.1 ipv4.method manual
```

### Add Slave Interfaces

These will bring up the two interfaces in the team:

```text
[cloud_user@host]$ sudo nmcli con add type team-slave ifname eth1 con-name Slave1 master team0
[cloud_user@host]$ sudo nmcli con add type team-slave ifname eth2 con-name Slave2 master team0
```

Now we can run `nmcli c` to see all of our interfaces.

### Verify the Team State

To verify that the team and interfaces are set properly:

```text
[cloud_user@host]$ nmcli c show Team0 | grep ipv4
```

This will start up the team, then the interfaces:

```text
[cloud_user@host]$ sudo nmcli con up Team0
[cloud_user@host]$ sudo nmcli con up Slave1
[cloud_user@host]$ sudo nmcli con up Slave2
```

We can run a quick `nmcli` to check again, and make sure everything came up the way it was supposed to. We'll see our interfaces listed with a few details.

To get details on the team itself, let's run:

```text
[cloud_user@host]$ sudo teamdctl team0 state
```

### Verify Connectivity

Everything looks good, but is it actually working? We'll try this:

```text
[cloud_user@host]$ ping 10.0.1.15
```

If we get a reply, we're good to go.

### Conclusion

We set up a team, with a couple of interfaces, and adhered to all of the specifications our supervisor gave us. Congratulations!

