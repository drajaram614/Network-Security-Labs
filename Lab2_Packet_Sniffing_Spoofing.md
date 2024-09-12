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

### Submission

- **ICMP packet captures:**
    - Screenshot: ` Task 1.1B ICMP`
    ![ Task 1.1B ICMP.png](images/Task%201.1B%20ICMP.png)

## Q1.4: Task 1.1B(b): Capture TCP Packets for Port 23

For this task, you will capture TCP packets that come from a specific IP address and are destined for port 23 (used by Telnet).

### Code:
```python
def print_pkt(pkt):
    pkt.show()

pkt = sniff(filter= 'BLANK' , prn=print_pkt)
```
### Options:

`tcp port 23`

`host 10.9.0.3 and port 23`

`tcp and host 10.9.0.1 and port 23`

`icmp and host 10.9.0.2 and port 23`

### Answer:

`tcp and host 10.9.0.1 and port 23`

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

## Q1.5 Task 1.1B(b): Capture Specific TCP/Port 23 (Screenshot)

### Instructions:
Take a screenshot demonstrating that TCP packets destined for port 23 were captured. Ensure the screenshot includes the entire VM along with the date and time.

### Submission

  - Display TCP packet captures for port 23

  - Include date and time in the VM interface

- **TCP packet captures:**
    - Screenshot: `Q5 Task 1.1B Capture specific TCP/port 23`
    ![Q1.5 Task 1.1B TCP.png](images/Q1.5%20Task%201.1B%20TCP.png)

## Q1.6 Task 1.1B(c): Particular Subnet

### Instructions:
Capture packets coming from or going to the subnet `10.9.0.0/24`. Fill in the blank in the code provided, and then take a screenshot demonstrating the traffic generated.

### Code:

```python
def print_pkt(pkt):
    pkt.show()

pkt = sniff(filter= 'BLANK' , prn=print_pkt)
```
#### Options:


`net 10.9.0.0/24`

`net 10.9.0.0`

`net 10.9.0.0/26`

`net 128.238.0.0/24`

#### Answer:
The correct filter option to capture traffic is:

`net 10.9.0.0/24`

## Q1.7 Task 1.1B(c): Particular Subnet(Screenshot)

### Submission

Show packets from the 10.9.0.0/24 subnet

Include date and time in the VM interface

- **Subnet 10.9.0.0/24 packet capture:**
    - Screenshot: `Q1.7_Task_1.1B_TCP`
    ![Q1.7_Task_1.1B_TCP.png](images/Q1.7%20Task%201.1B%20TCP.png)

## Q2 Task 1.2: Spoofing ICMP Packets

### Instructions:

You will spoof an ICMP echo request packet with a source IP address of 8.8.8.8 from the first VM and send it to the second VM. Use Wireshark on the second VM to show that it replies back with ICMP echo replies.

#### Code:
```python
from scapy.all import *

# Create an IP packet
a = IP()  # ➀
a.dst = '10.0.2.3'  # ➁

# Create an ICMP packet
b = ICMP()  # ➂

# Combine IP and ICMP packets
p = a/b  # ➃

# Send the packet
send(p)  # ➄
```
#### Explanation:

`a = IP()`: Initializes an IP packet.
`a.dst = '10.0.2.3'`: Sets the destination IP address.
`b = ICMP()`: Initializes an ICMP packet.
`p = a/b`: Combines the IP and ICMP packets.
`send(p)`: Sends the packet to the destination.

#### Verification:

Use Wireshark on the second VM to capture network traffic and verify that the echo replies are received in response to your spoofed ICMP packet.
Ensure you capture the entire communication to show the request and reply.


