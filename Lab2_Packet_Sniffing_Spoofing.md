# Packet Sniffing and Spoofing Lab

## Summary
- Capture and analyze ICMP, TCP, and subnet traffic using Python and Scapy.
- Focus on packet filtering and capturing specific types of traffic.

---

## Prerequisites
- Basic knowledge of networking and TCP/IP.
- Python installed with the `scapy` module.
- Root or sudo privileges to run sniffing programs.
- Virtual Machine set up with network interfaces.

---

## Sniffing ICMP Packets

### Code:
```python
from scapy.all import *

def print_pkt(pkt):
    pkt.show()

pkt = sniff(iface='br-c93733e9f913', filter='icmp', prn=print_pkt)
```

### Steps:
- Capture ICMP packets using `sniff()`.
- `iface`: Adjust to match your network interface.
- Run the program:  
  ```bash
  sudo ./sniffer.py
  ```

---

## Capture ICMP Packets (Screenshot)
- Use `ping <destination_ip>` to generate ICMP traffic.
- Screenshot example:
  ![ICMP capture](images/Task%201.1B%20ICMP.png)

---

## Capture TCP Packets on Port 23

### Code:
```python
def print_pkt(pkt):
    pkt.show()

pkt = sniff(filter='tcp and host 10.9.0.1 and port 23', prn=print_pkt)
```

### Steps:
- Capture only TCP packets from `10.9.0.1` on port 23 (Telnet traffic).
- Generate Telnet traffic:
  ```bash
  telnet 10.9.0.1 23
  ```

---

## Capture TCP Packets for Port 23 (Screenshot)
- Screenshot example:
  ![TCP capture](images/Q1.5%20Task%201.1B%20TCP.png)

---

## Capture Packets from Subnet 10.9.0.0/24

### Code:
```python
def print_pkt(pkt):
    pkt.show()

pkt = sniff(filter='net 10.9.0.0/24', prn=print_pkt)
```

### Steps:
- Capture all packets from subnet `10.9.0.0/24`.
- Screenshot example:
  ![Subnet capture](images/Q1.7%20Task%201.1B%20TCP.png)

### Packet Sniffing and Spoofing Lab

#### ICMP Packet Spoofing

- **Objective**: Spoof an ICMP echo request packet from a source IP (`8.8.8.8`) on one VM, and verify that the second VM replies using Wireshark.
  
- **Code**:
    ```python
    from scapy.all import *

    a = IP() 
    a.dst = '10.0.2.3'  # Destination IP address

    b = ICMP()  # ICMP packet

    p = a/b  # Combine IP and ICMP

    send(p)  # Send packet
    ```

- **Verification**: 
    - Capture traffic in Wireshark on the second VM to verify ICMP echo replies.

---

#### Scapy ICMP Spoofing Code

- **Objective**: Complete the Scapy code to spoof an ICMP packet.

- **Code**:
    ```python
    from scapy.all import *

    ip = IP(src='8.8.8.8', dst='10.9.0.5')  # Source and destination IPs
    icmp = ICMP()  # ICMP layer

    packet = ip/icmp
    packet.show()  # Display packet details

    send(packet)  # Send packet
    ```

- **Wireshark Screenshot**:
    ![Q2.2 Task 1.2 Wireshark Screenshot](images/Q2.2%20Task%201.2%20Wireshark.png)

---

#### ICMP Traceroute

- **Objective**: Implement a fully automated ICMP traceroute using Scapy (without the built-in function).

- **Code**:
    ```python
    from scapy.all import *

    ttl = 1
    while True:
        a = IP(dst='8.8.8.8', ttl=ttl)  
        b = ICMP()
        p = a/b
        pkt = sr1(p, verbose=0)
        if pkt[IP].type == 0:  # Destination reached
            print("Complete", pkt[IP].src)
            break
        else:
            print("TTL: %d, Source: %s" % (ttl, pkt[IP].src))
        ttl += 1
    ```

- **Traceroute Output**:
    ![Q3.2 Task 1.3 traceroute](images/Q3.2%20Task%201.3%20traceroute.png)

---

#### Sniffing and Spoofing ICMP Packets

- **Objective**: Create a program that sniffs for ICMP packets and sends a spoofed response.

- **Code**:
    ```python
    from scapy.all import *

    def sniff_and_spoof(packet):
        if ICMP in packet:
            ip = IP(src='1.2.3.4', dst=packet[IP].src)  # Spoofed source IP
            icmp = ICMP(type=0, id=packet[ICMP].id, seq=packet[ICMP].seq)
            new_packet = ip/icmp
            send(new_packet, verbose=0)

    pkt = sniff(filter='icmp', prn=sniff_and_spoof)  # Sniff ICMP packets
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

### Sniffing and Spoofing ICMP Packets Lab

#### Task: Ping 1.2.3.4 and 10.9.0.99

- **Ping 1.2.3.4** and **10.9.0.99** from the terminal and observe the outputs.
- Run the **sniffing and spoofing program** to verify handling of ICMP packets.

**Outputs:**

- **Program Output**: Shows the program handling ICMP packets.
- **Terminal Ping**: Results of the `ping` command for `1.2.3.4` and `10.9.0.99`.

**Screenshots:**

- **Ping 1.2.3.4 Output:**

  ![Ping 1.2.3.4](images/Q4.2%20Task%201.4%20-%20Ping%201.2.3.4.png)

- **Ping 10.9.0.99 Output:**

  ![Ping 10.9.0.99](images/Q4.3%20Task%201.4%20-%20Ping%2010.9.0.99.png)

  ### Explanation

**Why It Does Not Work:**

The failure to communicate with IP address `10.9.0.99` is likely due to network issues related to the ARP (Address Resolution Protocol). Here’s an explanation:

- **ARP Protocol**: ARP is used to map IP addresses to MAC addresses within a local subnet. When a device needs to communicate with another device on the same network, it uses ARP to find the MAC address associated with the IP address.

- **Unreachable Host**: If the ARP table does not contain an entry for `10.9.0.99`, or if there is no device with that IP on the subnet, the gateway or local router will return a "Destination Host Unreachable" message. This indicates that the network layer cannot find a route to the IP address because it is either not reachable or does not exist on the network.

- **Impact on Sniffing and Spoofing**: Since `10.9.0.99` is not a valid or reachable host, the sniffing and spoofing program cannot intercept ICMP echo requests or send spoofed responses. The ARP failure prevents the packets from being delivered correctly to the destination, thereby rendering the sniffing and spoofing operations ineffective.

**Summary**: The issue arises because `10.9.0.99` is a non-existent or unreachable IP address on the local subnet. The ARP protocol's inability to map this address to a MAC address results in the network reporting the destination as unreachable, which prevents the sniffing and spoofing program from functioning as intended.

### Sniffing and Spoofing ICMP Packets Lab

#### Task: Ping 8.8.8.8

- **Ping 8.8.8.8** from the terminal.
- Run the **sniffing and spoofing program** to verify handling of ICMP packets.

**Outputs:**

- **Program Output**: Shows handling of ICMP packets.
- **Terminal Ping**: Result of the `ping` command for `8.8.8.8`.

**Screenshot:**

- ![Ping 8.8.8.8](images/Q4.4%20Task%201.4%20-%20Ping%208.8.8.8.png)

---

### Extra Credit: ARP Cache Poisoning to Ping 10.9.0.99 (+5%)

**Objective**: Write a program to successfully ping `10.9.0.99` using ARP cache poisoning (ARP spoofing).

**Instructions:**
- **ARP Cache Poisoning**: Send fake ARP responses to associate `10.9.0.99` with your machine’s MAC address.
- **Configure Scapy**: Use the `sniff` function with the appropriate filter to capture packets.
- **Ping 10.9.0.99**: After ARP spoofing, ping `10.9.0.99` and check the response.

**Outputs:**

- **Program Output**: Results of ARP spoofing and pinging.
- **Terminal Ping**: `ping` result for `10.9.0.99`.
- **Wireshark**: Capture showing ARP spoofing and ping replies.

**Screenshots:**

- **Program Code:**
  ![Extra Credit Program Code](images/Q5%20Extra%20Credit%20-%20CODE.png)

- **Ping 10.9.0.99 Output:**
  ![Extra Credit Ping](images/Q5%20Extra%20Credit%20-%20Ping%2010.9.0.99.png)

  ## What I Learned:

In this lab, I gained hands-on experience with Scapy for network traffic analysis and manipulation. I learned to capture and filter ICMP and TCP packets, spoof ICMP packets, and implement a custom ICMP traceroute program. Combining packet sniffing and spoofing techniques enhanced my understanding of network traffic dynamics and practical cybersecurity skills.:

### Sniffing Packets

I learned to use Scapy to capture and display network packets, focusing on ICMP packets. I wrote a script to monitor network traffic, applied filters to isolate specific packet types, and used the show() function to detail packet contents.

### Capture Only ICMP Packets

I practiced filtering network traffic to capture only ICMP packets by specifying the appropriate filter (icmp). This skill is essential for isolating specific network traffic types during analysis.

### Capture TCP Packets for Port 23

I gained experience in capturing TCP packets directed at a specific port (23, used by Telnet) and from a particular host. This task taught me how to use filters to capture targeted network traffic, which is useful for monitoring and analyzing specific communications.

### Capture Packets from a Particular Subnet

I learned how to capture packets within a specific subnet (10.9.0.0/24). I used subnet filters to focus on traffic within a defined network range, enhancing my ability to analyze subnet-specific traffic.

### Spoofing ICMP Packets

I practiced spoofing ICMP echo request packets using Scapy, setting a custom source IP address and sending the packet to a destination IP. This exercise demonstrated how to manipulate packet headers to create spoofed network traffic and verify responses with tools like Wireshark.

### Fully-Automated Traceroute

I implemented a custom ICMP traceroute program, learning to handle TTL (Time-to-Live) values to trace the route to a destination. This involved creating a script to increment TTL values and identify the path packets take through the network.

### Sniffing and Then Spoofing

I combined packet sniffing and spoofing in a single program. I captured ICMP packets and sent spoofed responses based on the captured data. This task taught me to handle scenarios where specific IP addresses might not be reachable and understand potential network configuration issues that could impact packet transmission.

Overall, this lab provided practical experience in network traffic analysis and manipulation, enhancing my understanding of packet sniffing and spoofing techniques in cybersecurity.
