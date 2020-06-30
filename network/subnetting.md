# Subnetting

## Network Traffic

### Traffic methods

* Unicast
* Multicast
* Broadcast

### OSI

Open System Interconnection Model

![OSI](../.gitbook/assets/screen-shot-2020-06-26-at-15.54.46.png)

### TCP/IP

![](../.gitbook/assets/screen-shot-2020-06-26-at-15.56.18.png)

![](../.gitbook/assets/screen-shot-2020-06-26-at-15.57.20.png)

![](../.gitbook/assets/screen-shot-2020-06-26-at-15.58.43.png)

![](../.gitbook/assets/screen-shot-2020-06-26-at-16.00.54.png)

## Network Segmentation

### Why we need segmentation

In any local network like a small corporate network, there could be few things that affect network.

### Bad actor

It could compromise the network resources it has access to. Like a host in the sales department trying to access application server through local network.

### Limiting

In a single-scoped network, bandwidth competition could lead to quality of service issues for important services and hosts.

### Scaling

Needs to easily scale out.

### Benefits

* Improve network performance and speed
* Reduce network congestion
* Enhance security
* Controlled Expansion
* Maintenance and Administration

## Addressing

### IPv4

![](../.gitbook/assets/screen-shot-2020-06-26-at-16.13.57.png)

### IPv6

![](../.gitbook/assets/screen-shot-2020-06-26-at-16.17.29.png)

![](../.gitbook/assets/screen-shot-2020-06-26-at-16.18.44.png)

### MAC

Hardware address on NIC

![](../.gitbook/assets/screen-shot-2020-06-26-at-16.20.20.png)

### ARP

![](../.gitbook/assets/screen-shot-2020-06-26-at-16.23.33.png)

## Network Masks

![](../.gitbook/assets/screen-shot-2020-06-26-at-17.00.50.png)

### Number of Hosts

![](../.gitbook/assets/screen-shot-2020-06-29-at-18.11.53.png)

### Range of Network Address

![](../.gitbook/assets/screen-shot-2020-06-29-at-18.12.11.png)

### Number of Subnets

![](../.gitbook/assets/screen-shot-2020-06-29-at-18.12.26.png)

## Classful Networking

## CIDR

Classless Inter-Domain Routing

* Does not use classes at all for networking assignment or sizing
* The entire unicast range can be segmented into any sized network
* Subnet masks not limited to 255.255.255.0, 255.255.0.0 or 255.0.0.0

### Calculating Network Range

![](../.gitbook/assets/screen-shot-2020-06-29-at-18.49.13.png)

