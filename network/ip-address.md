# IP Address

## IP address

A public [IP address](https://www.iplocation.net/ip-address) is an IP address that can be accessed over the Internet. Like postal address used to deliver a postal mail to your home, a public IP address is the globally unique IP address assigned to a computing device. Your public IP address can be found at [What is my IP Address](https://www.iplocation.net/find-ip-address) page. Private IP address, on the other hand, is used to assign computers within your private space without letting them directly expose to the Internet. For example, if you have multiple computers within your home you may want to use private IP addresses to address each computer within your home. In this scenario, your router gets the public IP address, and each of the computers, tablets and smartphones connected to your router \(via wired or wifi\) gets a private IP address from your router via [DHCP](https://www.iplocation.net/dhcp) protocol.

Internet Assigned Numbers Authority \(IANA\) is the organization responsible for registering IP address ranges to organizations and Internet Service Providers \(ISPs\). To allow organizations to freely assign private IP addresses, the Network Information Center \(InterNIC\) has reserved certain address blocks for private use. The following IP blocks are reserved for private IP addresses.

| Class | Starting IP Address | Ending IP Address | \# of Hosts |
| :--- | :--- | :--- | :--- |
| A | 10.0.0.0 | 10.255.255.255 | 16,777,216 |
| B | 172.16.0.0 | 172.31.255.255 | 1,048,576 |
| C | 192.168.0.0 | 192.168.255.255 | 65,536 |

### What is public IP address?

A public IP address is the address that is assigned to a computing device to allow direct access over the Internet. A web server, email server and any server device directly accessible from the Internet are candidate for a public IP address. A public IP address is globally unique, and can only be assigned to a unique device.

### What is private IP address?

A private IP address is the address space allocated by InterNIC to allow organizations to create their own private network. There are three IP blocks \(1 class A, 1 class B and 1 class C\) reserved for a private use. The computers, tablets and smartphones sitting behind your home, and the personal computers within an organizations are usually assigned private IP addresses. A network printer residing in your home is assigned a private address so that only your family can print to your local printer.

When a computer is assigned a private IP address, the local devices see this computer via it's private IP address. However, the devices residing outside of your local network cannot directly communicate via the private IP address, but uses your router's public IP address to communicate. To allow direct access to a local device which is assigned a private IP address, a Network Address Translator \(NAT\) should be used.

## Subnet mask

An [IP address](https://www.iplocation.net/ip-address) has two components, the network address and the host address. A subnet mask separates the IP address into the network and host addresses \(&lt;network&gt;&lt;host&gt;\). Subnetting further divides the host part of an IP address into a subnet and host address \(&lt;network&gt;&lt;subnet&gt;&lt;host&gt;\) if additional subnetwork is needed. Use the [Subnet Calculator](https://www.iplocation.net/subnet-calculator) to retrieve subnetwork information from IP address and Subnet Mask. It is called a subnet mask because it is used to identify network address of an IP address by performing a bitwise AND operation on the net mask.

A Subnet mask is a 32-bit number that masks an IP address, and divides the IP address into network address and host address. Subnet Mask is made by setting network bits to all "1"s and setting host bits to all "0"s. Within a given network, two host addresses are reserved for special purpose, and cannot be assigned to hosts. The "0" address is assigned a network address and "255" is assigned to a broadcast address, and they cannot be assigned to hosts.

Examples of commonly used netmasks for classed networks are 8-bits \(Class A\), 16-bits \(Class B\) and 24-bits \(Class C\), and classless networks are as follows:

| Class | Address | \# of Hosts | Netmask \(Binary\) | Netmask \(Decimal\) |
| :--- | :--- | :--- | :--- | :--- |
| CIDR | /4 | 240,435,456 | 11110000 00000000 00000000 00000000 | 240.0.0.0 |
| CIDR | /5 | 134,217,728 | 11111000 00000000 00000000 00000000 | 248.0.0.0 |
| CIDR | /6 | 67,108,864 | 11111100 00000000 00000000 00000000 | 252.0.0.0 |
| CIDR | /7 | 33,554,432 | 11111110 00000000 00000000 00000000 | 254.0.0.0 |
| A | /8 | 16,777,216 | 11111111 00000000 00000000 00000000 | 255.0.0.0 |
| CIDR | /9 | 8,388,608 | 11111111 10000000 00000000 00000000 | 255.128.0.0 |
| CIDR | /10 | 4,194,304 | 11111111 11000000 00000000 00000000 | 255.192.0.0 |
| CIDR | /11 | 2,097,152 | 11111111 11100000 00000000 00000000 | 255.224.0.0 |
| CIDR | /12 | 1,048,576 | 11111111 11110000 00000000 00000000 | 255.240.0.0 |
| CIDR | /13 | 524,288 | 11111111 11111000 00000000 00000000 | 255.248.0.0 |
| CIDR | /14 | 262,144 | 11111111 11111100 00000000 00000000 | 255.252.0.0 |
| CIDR | /15 | 131,072 | 11111111 11111110 00000000 00000000 | 255.254.0.0 |
| B | /16 | 65,534 | 11111111 11111111 00000000 00000000 | 255.255.0.0 |
| CIDR | /17 | 32,768 | 11111111 11111111 10000000 00000000 | 255.255.128.0 |
| CIDR | /18 | 16,384 | 11111111 11111111 11000000 00000000 | 255.255.192.0 |
| CIDR | /19 | 8,192 | 11111111 11111111 11100000 00000000 | 255.255.224.0 |
| CIDR | /20 | 4,096 | 11111111 11111111 11110000 00000000 | 255.255.240.0 |
| CIDR | /21 | 2,048 | 11111111 11111111 11111000 00000000 | 255.255.248.0 |
| CIDR | /22 | 1,024 | 11111111 11111111 11111100 00000000 | 255.255.252.0 |
| CIDR | /23 | 512 | 11111111 11111111 11111110 00000000 | 255.255.254.0 |
| C | /24 | 256 | 11111111 11111111 11111111 00000000 | 255.255.255.0 |
| CIDR | /25 | 128 | 11111111 11111111 11111111 10000000 | 255.255.255.128 |
| CIDR | /26 | 64 | 11111111 11111111 11111111 11000000 | 255.255.255.192 |
| CIDR | /27 | 32 | 11111111 11111111 11111111 11100000 | 255.255.255.224 |
| CIDR | /28 | 16 | 11111111 11111111 11111111 11110000 | 255.255.255.240 |
| CIDR | /29 | 8 | 11111111 11111111 11111111 11111000 | 255.255.255.248 |
| CIDR | /30 | 4 | 11111111 11111111 11111111 11111100 | 255.255.255.252 |

Subnetting an IP network is to separate a big network into smaller multiple networks for reorganization and security purposes. All nodes \(hosts\) in a subnetwork see all packets transmitted by any node in a network. Performance of a network is adversely affected under heavy traffic load due to collisions and retransmissions.

Applying a subnet mask to an IP address separates network address from host address. The network bits are represented by the 1's in the mask, and the host bits are represented by 0's. Performing a bitwise logical AND operation on the IP address with the subnet mask produces the network address. For example, applying the Class C subnet mask to our IP address 216.3.128.12 produces the following network address:

```text
IP:   1101 1000 . 0000 0011 . 1000 0000 . 0000 1100  (216.003.128.012)

Mask: 1111 1111 . 1111 1111 . 1111 1111 . 0000 0000  (255.255.255.000)

      ---------------------------------------------

      1101 1000 . 0000 0011 . 1000 0000 . 0000 0000  (216.003.128.000)

```

  
Subnetting Network  


Here is another scenario where subnetting is needed. Pretend that a web host with a Class C network needs to divide the network so that parts of the network can be leased to its customers. Let's assume that a host has a network address of 216.3.128.0 \(as shown in the example above\). Let's say that we're going to divide the network into 2 and dedicate the first half to itself, and the other half to its customers.

```text
   216 .   3 . 128 . (0000 0000)  (1st half assigned to the web host)

   216 .   3 . 128 . (1000 0000)  (2nd half assigned to the customers)

```

The web host will have the subnet mask of 216.3.128.128 \(/25\). Now, we'll further divide the 2nd half into eight block of 16 IP addresses.

```text
   216 .   3 . 128 . (1000 0000)  Customer 1 -- Gets 16 IPs (14 usable)

   216 .   3 . 128 . (1001 0000)  Customer 2 -- Gets 16 IPs (14 usable)

   216 .   3 . 128 . (1010 0000)  Customer 3 -- Gets 16 IPs (14 usable)

   216 .   3 . 128 . (1011 0000)  Customer 4 -- Gets 16 IPs (14 usable)

   216 .   3 . 128 . (1100 0000)  Customer 5 -- Gets 16 IPs (14 usable)

   216 .   3 . 128 . (1101 0000)  Customer 6 -- Gets 16 IPs (14 usable)

   216 .   3 . 128 . (1110 0000)  Customer 7 -- Gets 16 IPs (14 usable)

   216 .   3 . 128 . (1111 0000)  Customer 8 -- Gets 16 IPs (14 usable)

   -----------------------------

   255 . 255 . 255 . (1111 0000)  (Subnet mask of 255.255.255.240)

```

You may use [Subnet Calculator](https://www.iplocation.net/subnet-calculator) to ease your calculation.  
CIDR - Classless Inter Domain Routing  


**C**lassless **I**nter**D**omain **R**outing \(CIDR\) was invented to keep the Internet from running out of IP Addresses. The IPv4, a 32-bit, addresses have a limit of 4,294,967,296 \(232\) unique IP addresses. The classful address scheme \(Class A, B and C\) of allocating IP addresses in 8-bit increments can be very wasteful. With classful addressing scheme, a minimum number of IP addresses allocated to an organization is 256 \(Class C\). Giving 256 IP addresses to an organization only requiring 15 IP addresses is wasteful. Also, an organization requiring more than 256 IP addresses \(let's say 1,000 IP addresses\) is assigned a Class B, which allocates 65,536 IP addresses. Similarly, an organization requiring more than 65,636 \(65,634 usable IPs\) is assigned a Class A network, which allocates 16,777,216 \(16.7 Million\) IP addresses. This type of address allocation is very wasteful.

With CIDR, a network of IP addresses is allocated in 1-bit increments as opposed to 8-bits in classful network. The use of a CIDR notated address can easily represent classful addresses \(Class A = /8, Class B = /16, and Class C = /24\). The number next to the slash \(i.e. /8\) represents the number of bits assigned to the network address. The example shown above can be illustrated with CIDR as follows:

```text
   216.3.128.12, with subnet mask of 255.255.255.128 is written as

   216.3.128.12/25



   Similarly, the 8 customers with the block of 16 IP addresses can be

   written as:



   216.3.128.129/28, 216.3.128.130/28, and etc.

```

With an introduction of CIDR addressing scheme, IP addresses are more efficiently allocated to ISPs and customers; and hence there is less risk of IP addresses running out anytime soon. For detailed specification on CIDR, please review RFC 1519. With introduction of additional gaming, medical, applicance and telecom devices requiring static IP addresses in addition to more than 6.5 billion \(July 2006 est.\) world population, the IPv4 addresses with CIDR addressing scheme will eventually run out. To solve shortage of IPv4 addresses, the IPv6 \(128-bit\) address scheme was introduced in 1993.

