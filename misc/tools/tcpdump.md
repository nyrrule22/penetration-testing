# tcpdump

## Promiscuous Mode

Make sure promiscuous mode is set.

VM Settings --> Network --> Advanced --> Promiscuous mode.

## Examples

```bash
tcpdump -D  # List the interfaces
tcpdump -i eth0  # Specify the interface to listen on e.g. eth0
tcpdump -i any  # Listen on all interfaces
tcpdump -i eth0 icmp  # Listen for ICMP packets on a specific interface
tcpdump -c 10 -i any  # Provide count of packets to capture
tcpdump -c 10 -i any -n  # Surpress hostname resolution
tcpdump -c 10 -i any -nn  # Surpress hostname and port name resolution
tcpdump -i eth0 -c 10 -s 0 -X  # Display the whole packet
tcpdump -c 10 host <IP>  # Look at traffic between us and another server
tcpdump -c 10 udp  # Look at only the UDP packets on the network
tcpdump net <IP>/24  # Look at traffic to and from a subnet
tcpdump -c 10 port 22  # Look at traffic just to one port
tcpdump -c 10 dst portrange 1-1023  # Look at traffic to a range of desintation ports
tcpdump -c 10 ip6 -w ip6.cap  # Write out as a pcap file
```
