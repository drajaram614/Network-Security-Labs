# Packet Sniffing and Spoofing Lab

## Summary

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

## Q2.1 Task 1.2 - Scapy Code

### Instructions:
Complete the Scapy code to spoof an ICMP packet. Use the provided IP addresses to fill in the blanks and execute the code.

### Code:

```python
from scapy.all import *

# Define IP layer with source and destination addresses
ip = IP(src='BLANK(1)', dst='BLANK(2)')  # BLANK(1) and BLANK(2)
icmp = ICMP()  # Define ICMP layer

# Combine IP and ICMP layers to create the packet
packet = ip/icmp

# Display the packet details
packet.show()

# Send the packet
send(packet)
```
### Questions:
a. Based on the code above, what is the IP address for (1)?

`Answer: 8.8.8.8`

b. Based on the code above, what is the IP address for (2)?

`Answer: 10.9.0.5`

## Q2.2 Task 1.2 - Wireshark Screenshot

### Instructions:

Take a screenshot in Wireshark showing the spoofed ICMP echo request from 8.8.8.8 and that the VM replied back with an echo reply.

### Submission

Show the ICMP echo request from 8.8.8.8

Show the corresponding echo reply

Include the entire VM interface with date and time

- **Spoof Echo Request Wireshark capture:**
    - Screenshot: `Q2.2_Task_1.2_Screenshot`
    ![Q2.2_Task_1.2_Screenshot.png](images/Q2.2%20Task%201.2%20Wireshark.png)


    ## Q3 Task 1.3: Fully-Automated Traceroute

### Instructions:

Using the provided skeleton code, implement an ICMP traceroute using Scapy. Do not use the built-in Scapy traceroute function. Perform a traceroute to `8.8.8.8`. Capture the results using Wireshark and take a screenshot of your program’s output.

### Q3.1 Task 1.3 - Scapy Code

Fill in the blanks in the following code to implement the ICMP traceroute.

```python
from scapy.all import *

ttl = 'BLANK(1)'
while True:
    a = IP(dst='BLANK(2)', ttl=ttl)  
    b = ICMP()
    p = a/b
    pkt = sr1(p, verbose=0)
    if 'BLANK(3)' == 0:
        print("Complete", pkt[IP].src)
        break
    else:
        print("TTL: %d, Source: %s" % (ttl, pkt[IP].src))
    ttl += 1
```
### Answers:
a. Fill in the blank for 1.

`Answer: 1`

b. Fill in the blank for 2.

`Answer: 8.8.8.8`

c. Fill in the blank for 3.

`Answer: pkt[IP].type`

### Explanation:

`ttl = 1`: Initializes the Time-to-Live (TTL) value for the ICMP packets.

`a = IP(dst='8.8.8.8', ttl=ttl)`: Creates an IP packet with the destination address 8.8.8.8 and the current TTL value.

`if pkt[IP].type == 0`: Checks if the packet type is 0, which indicates that the destination has been reached (ICMP Echo Reply).

## Q3.2 Task 1.3 - Write Your Own Traceroute Program

### Instructions:

For this task, you will write your own traceroute program using the skeleton code provided. Ensure that your traceroute program handles cases where routers may not respond, and fully automate the traceroute process.

### Implementation:
Use the following skeleton code to create your traceroute program. Be sure to handle scenarios where routers might not respond (e.g., timeouts).

```python
from scapy.all import *

def traceroute(dst_ip):
    ttl = 1
    while True:
        # Create IP packet with increasing TTL
        packet = IP(dst=dst_ip, ttl=ttl) / ICMP()
        response = sr1(packet, timeout=2, verbose=0)  # Set a timeout for the response
        
        if response is None:
            print("TTL: %d, No Response" % ttl)
        elif response[IP].type == 0:
            print("Complete", response[IP].src)
            break
        else:
            print("TTL: %d, Source: %s" % (ttl, response[IP].src))
        
        ttl += 1
```
# Test the traceroute function
`traceroute("8.8.8.8")`

### Write Your Traceroute Program:

Implement the traceroute program using the provided skeleton code.
Ensure that the program handles cases where a router does not respond within the timeout period.
### Testing:

Run your traceroute program to trace the route to 8.8.8.8.
Verify that your program handles non-responsive routers and completes the trace.

### Documentation:

#### Traceroute Program: 
Submit your Python traceroute program file.
#### Wireshark Screenshot: 
Capture a screenshot showing the Wireshark output of the traceroute to 8.8.8.8.

#### Program Output: 
Include a screenshot of the output from your traceroute program.

#### Ubuntu Built-in Traceroute: 
Take a screenshot of the output from the built-in Ubuntu traceroute command (traceroute -I 8.8.8.8).

#### VM Screenshot: 
Include a screenshot showing the entire VM interface with date and time visible.

- **Trace route program:**
    - Screenshot: `Q3.2 Task 1.3 traceroute`
    ![Q3.2 Task 1.3 traceroute.png](images/Q3.2%20Task%201.3%20traceroute.png)

## Q4 Task 1.4: Sniffing and Then Spoofing

In this task, you will perform both sniffing and spoofing on the same LAN. You need to run the programs on two machines: the VM and the user container. 

### Objective:
- Implement a program that sniffs for ICMP packets and then sends a spoofed response.
- Explain why the program does not work for the IP address `10.9.0.99`, even though it works for other IPs such as `1.2.3.4` and `8.8.8.8`.

### Q4.1 Task 1.4 - Sniffing and Then Spoofing Program

You will use the following skeleton code to create a program that both sniffs and spoofs ICMP packets.

```python
from scapy.all import *

def sniff_and_spoof(packet):
    if ICMP in packet:
        ip = IP('BLANK(1)', dst='BLANK(2)')
        icmp = ICMP(type='BLANK(3)', id='BLANK(4)', seq='BLANK(5)')
        raw_data = 'BLANK(6)'
        new_packet = 'BLANK(7)'
        send(new_packet, verbose=0)

# Sniffing for ICMP packets from a specific source IP address
pkt = sniff(filter='BLANK(8)', prn=sniff_and_spoof)
```

### Fill in the Blanks:
#### Fill in the blank for 1:

`packet[IP].dst`

#### Fill in the blank for 2:

`packet[IP].src`

#### Fill in the blank for 3:

`0`

##### Fill in the blank for 4:

`packet[ICMP].id`

##### Fill in the blank for 5:

`packet[ICMP].seq`

##### Fill in the blank for 6:

`packet[Raw].load`

##### Fill in the blank for 7:

`ip/icmp/raw_data`

##### Fill in the blank for 8:


`icmp and src host 10.9.0.6`

### Explanation:
Why the Program Does Not Work for IP 10.9.0.99: The program might not work for IP 10.9.0.99 due to several possible reasons:

The IP address 10.9.0.99 might not be active or reachable on the network.

There might be network security measures or configurations preventing packets from or to 10.9.0.99.

The IP 10.9.0.99 could be configured to drop or ignore certain types of packets or responses.

Ensure that the network configuration does not block or filter packets for specific IP addresses.


## Q4.2 Task 1.4 - Ping 1.2.3.4

1. **Ping the IP address 1.2.3.4** from the terminal and observe the output.

2. **Run your sniffing and spoofing program** and verify that it correctly handles the ICMP packets.

**Ping 1.2.3.4. Screenshot:**

- **Program Output**: Show the output of your program.

- **Terminal Ping**: Show the result of the `ping` command for `1.2.3.4`.

- `Q4.2 Task 1.4 - Ping 1.2.3.4`

![Q4.2 Task 1.4 - Ping 1.2.3.4](images/Q4.2%20Task%201.4%20-%20Ping%201.2.3.4.png)

## Q4.3 Task 1.4 - Ping 10.9.0.99

1. **Ping the IP address 10.9.0.99** from the terminal and observe the output.

2. **Run your sniffing and spoofing program** and verify if it handles the ICMP packets.

**Screenshot:**

- **Program Output**: Show the output of your program.

- **Terminal Ping**: Show the result of the `ping` command for `10.9.0.99`.

- `Q4.3 Task 1.4 - Ping 10.9.0.99`

![Q4.3 Task 1.4 - Ping 10.9.0.99.png](images/Q4.3%20Task%201.4%20-%20Ping%2010.9.0.99.png)


### Explanation

**Why It Does Not Work:**

The failure to communicate with IP address `10.9.0.99` is likely due to network issues related to the ARP (Address Resolution Protocol). Here’s an explanation:

- **ARP Protocol**: ARP is used to map IP addresses to MAC addresses within a local subnet. When a device needs to communicate with another device on the same network, it uses ARP to find the MAC address associated with the IP address.

- **Unreachable Host**: If the ARP table does not contain an entry for `10.9.0.99`, or if there is no device with that IP on the subnet, the gateway or local router will return a "Destination Host Unreachable" message. This indicates that the network layer cannot find a route to the IP address because it is either not reachable or does not exist on the network.

- **Impact on Sniffing and Spoofing**: Since `10.9.0.99` is not a valid or reachable host, the sniffing and spoofing program cannot intercept ICMP echo requests or send spoofed responses. The ARP failure prevents the packets from being delivered correctly to the destination, thereby rendering the sniffing and spoofing operations ineffective.

**Summary**: The issue arises because `10.9.0.99` is a non-existent or unreachable IP address on the local subnet. The ARP protocol's inability to map this address to a MAC address results in the network reporting the destination as unreachable, which prevents the sniffing and spoofing program from functioning as intended.

Make sure to include the required screenshots and explanations in your submission to complete the task.

## Q4.4 Task 1.4 - Ping 8.8.8.8

1. **Ping the IP address 8.8.8.8** from the terminal and observe the output.

2. **Run your sniffing and spoofing program** and verify that it correctly handles the ICMP packets.

**Expected Output for 8.8.8.8**:
- The output should show the successful ping results to `8.8.8.8`.

**Screenshot:**

- **Program Output**: Show the output of your program.

- **Terminal Ping**: Show the result of the `ping` command for `8.8.8.8`.

- `Q4.4 Task 1.4 - Ping 8.8.8.8`
![Q4.4 Task 1.4 - Ping 8.8.8.8.png](images/Q4.4%20Task%201.4%20-%20Ping%208.8.8.8.png)

**Submission File**

## Q5 Extra Credit - Ping 10.9.0.99 (+5%)

**Objective**: Write a program that allows you to successfully ping `10.9.0.99` by performing ARP cache poisoning (ARP spoofing).

### Instructions:

1. **Perform ARP Cache Poisoning**: Use ARP spoofing to trick the network into thinking that your machine is the destination for `10.9.0.99`. This involves sending fake ARP responses to associate `10.9.0.99` with your machine's MAC address.

2. **Configure Scapy's Sniff Function**: Ensure that you set the appropriate filter argument in Scapy’s `sniff` function to capture the relevant packets.

3. **Ping 10.9.0.99**: After setting up ARP spoofing, ping `10.9.0.99` from the terminal and check if you receive a response.

**Requirements**:

- **Screenshot**:

  - **Program Output**: Show the output of your ARP spoofing and pinging program.

  - **Terminal Ping**: Show the result of the `ping` command for `10.9.0.99`.

  - **Wireshark**: Provide a screenshot of Wireshark showing the ARP spoofing and the replies to the ping request.

- **Extra Credit Program Code**: `Q5 Extra Credit - CODE`
![Q5 Extra Credit-CODE.png](images/Q5%20Extra%20Credit%20-%20CODE.png)

- **Extra Credit Ping**: `Q5 Extra Credit - Ping 10.9.0.99`
![Q5 Extra Credit - Ping 10.9.0.99.png](images/Q5%20Extra%20Credit%20-%20Ping%2010.9.0.99.png)

## What I Learned:

In this lab, I gained hands-on experience with Scapy for network traffic analysis and manipulation. I learned to capture and filter ICMP and TCP packets, spoof ICMP packets, and implement a custom ICMP traceroute program. Combining packet sniffing and spoofing techniques enhanced my understanding of network traffic dynamics and practical cybersecurity skills.:

### Q1.1: Sniffing Packets

I learned to use Scapy to capture and display network packets, focusing on ICMP packets. I wrote a script to monitor network traffic, applied filters to isolate specific packet types, and used the show() function to detail packet contents.

### Q1.2: Capture Only ICMP Packets

I practiced filtering network traffic to capture only ICMP packets by specifying the appropriate filter (icmp). This skill is essential for isolating specific network traffic types during analysis.

### Q1.4: Capture TCP Packets for Port 23

I gained experience in capturing TCP packets directed at a specific port (23, used by Telnet) and from a particular host. This task taught me how to use filters to capture targeted network traffic, which is useful for monitoring and analyzing specific communications.

### Q1.6: Capture Packets from a Particular Subnet

I learned how to capture packets within a specific subnet (10.9.0.0/24). I used subnet filters to focus on traffic within a defined network range, enhancing my ability to analyze subnet-specific traffic.

### Q2: Spoofing ICMP Packets

I practiced spoofing ICMP echo request packets using Scapy, setting a custom source IP address and sending the packet to a destination IP. This exercise demonstrated how to manipulate packet headers to create spoofed network traffic and verify responses with tools like Wireshark.

### Q3: Fully-Automated Traceroute

I implemented a custom ICMP traceroute program, learning to handle TTL (Time-to-Live) values to trace the route to a destination. This involved creating a script to increment TTL values and identify the path packets take through the network.

### Q4: Sniffing and Then Spoofing

I combined packet sniffing and spoofing in a single program. I captured ICMP packets and sent spoofed responses based on the captured data. This task taught me to handle scenarios where specific IP addresses might not be reachable and understand potential network configuration issues that could impact packet transmission.

Overall, this lab provided practical experience in network traffic analysis and manipulation, enhancing my understanding of packet sniffing and spoofing techniques in cybersecurity.

