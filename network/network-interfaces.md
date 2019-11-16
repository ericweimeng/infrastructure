# Network Interfaces

## Static vs DHCP

### Differences

#### Static

* Dedicated, unchanging IP address assigned to the device
* Upon setting it, you must also specify the subnet, gateway, and DNS host information
* Not dependent on DHCP for anything 
* Typically used for important network devices and hosts that require absolute connectivity

#### DHCP

* Dynamic address assigned from a pool of IP addresses within the DHCP scope
* DHCO will normally provides subnet, gateway, and DNS information upon assigning an IP address
* Dependent on DHCP upon lease expiration
* Typically used for hosts and clients requiring transient connections
* DHCP is used for managing the IP addresses of a large number of hosts

### Considerations

* Using DHCP creates three potential points of failure in network connectivities: the DHCP host itself, a rogue DHCP host and DNS
* The DHCP host becomes an issue if it is misconfigured - especially if you are dependent upon reservations for static-like IP assignment
* The potential exists for a rogue DHCP host to present network configurations that could route traffic to a different router for nefarious purposes
* If host IPs are changing, you'll need to rely on FQDNs for host to host communication, placing a dependency on DNS for FQDN to IP resolution
* DHCP makes managing thousands of host IPs in virtual or automated infrastructure on a secure network much easier





