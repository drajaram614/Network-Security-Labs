# Packet Sniffing and Spoofing Lab

This guide will walk you through performing tasks related to packet sniffing and spoofing using Python and Scapy. You will capture specific network traffic, including ICMP and TCP packets, using filters to isolate certain types of communication.

## Prerequisites

- Basic understanding of networking, TCP/IP protocols, and packet structure
- Installed Python environment with `scapy` module
- Root or sudo privileges to run sniffing programs
- A virtual machine with network interfaces properly configured (e.g., VirtualBox, VMware, or Docker)

## Q1.1: Task 1.1A: Sniffing Packets

You will write a simple Python script using Scapy to capture and print out network packets.

### Code:

```python
#!/usr/bin/env python3
from scapy.all import *

def print_pkt(pkt):
    pkt.show()

pkt = sniff(iface='br-c93733e9f913', filter='icmp', prn=print_pkt)

```
#### Explanation:

`sniff()`: Captures the network packets.

`iface='br-c93733e9f913'`: Specify the network interface where packets will be captured. Adjust this value to match your environment (e.g., eth0, wlan0, etc.).

`filter='icmp'`: Capture only ICMP packets.

`prn=print_pkt`: Calls the print_pkt function every time a packet is captured, which will print out the packet details.

`show()`: Displays the packet's contents in detail.

#### Make the program executable
`chmod a+x sniffer.py`

##### Run the program with root privileges
`sudo ./sniffer.py`
If you switch to another account (e.g., seed), try running without root to check privileges:

```bash
su seed
$ ./sniffer.py
```
## Q1.2: Task 1.1B(a): Capture Only ICMP Packets
In this task, you will capture only ICMP packets using a filter in Scapy.

### Question:
Complete the blank in the following code to capture ICMP packets only.

```python
def print_pkt(pkt):
    pkt.show()

pkt = sniff(filter= 'BLANK' , prn=print_pkt)
```
### Options:

`ping`
`icmp`
`tcp`

### Answer:
The correct option to capture only ICMP packets is:

`icmp`

## Q1.3: Task 1.1B(a) Screenshot
#### Instructions:
Adjust the iface parameter to match the network interface on your virtual machine.
Run the ICMP capture script and generate some ICMP traffic (e.g., by using the ping command from another terminal or machine).
To generate ICMP traffic:


`ping <destination_ip>`

Take a screenshot showing the ICMP packets being captured in the terminal along with the date and time visible in your virtual machine.

#### ICMP packet captures
Date and time in the taskbar or system clock

The full VM interface for proof of environment

## Q1.4: Task 1.1B(b): Capture TCP Packets for Port 23

For this task, you will capture TCP packets that come from a specific IP address and are destined for port 23 (used by Telnet).

### Code:
```python
def print_pkt(pkt):
    pkt.show()

pkt = sniff(filter='tcp and host 10.9.0.1 and port 23', prn=print_pkt)
```
### Explanation:
`filter='tcp and host 10.9.0.1 and port 23'`: The filter captures only TCP packets coming from the host 10.9.0.1 and destined for port 23.

This will capture Telnet traffic from a specific IP.
`prn=print_pkt`: The function print_pkt is called to print out each captured packet.
#### Testing:
To test your script, generate some Telnet traffic using the following steps:

Run the sniffer script on the attacker machine:

`sudo ./sniffer.py`

#### Generate Telnet traffic:

From another terminal, use the following command to connect via Telnet:

`telnet 10.9.0.1 23`
#### Answer:
The correct filter option is:

`tcp and host 10.9.0.1 and port 23`

This will ensure that only TCP traffic destined for port 23 from the specific host is captured.