# TCP Attacks Lab

## Overview

This lab involves various TCP attacks, including SYN Flooding, TCP RST attacks, TCP Session Hijacking, and creating a Reverse Shell. Each task requires specific steps and screenshots to demonstrate the attacks and their mitigation techniques.

## Lab Setup

1. **Download the Lab Setup:**
   - [Lab Setup ZIP File](https://seedsecuritylabs.org/Labs_20.04/Networking/TCP_Attacks/)
   - [Docker Manual](https://github.com/seed-labs/seed-labs/blob/master/manuals/docker/SEEDManual-Container.md)

2. **Ensure Docker is Set Up:**
   - Follow the instructions in section 2.1 of the [Lab PDF](https://seedsecuritylabs.org/Labs_20.04/Files/TCP_Attacks/TCP_Attacks.pdf) to set up your environment.

## Tasks

### Q1 Task 1: SYN Flooding Attack

#### Introduction

In this lab, we will explore TCP SYN Flood attacks and their countermeasures. A SYN Flood is a type of Denial of Service (DoS) attack where attackers send numerous SYN requests to a target’s TCP port, but do not complete the 3-way handshake. This fills up the target's queue of half-open connections, preventing legitimate connections from being established.

#### Understanding SYN Flood Attack

##### How SYN Flood Works

1. **Attack Mechanism**: Attackers send a flood of SYN packets to the target TCP port.
2. **Half-Open Connections**: The target machine’s queue for half-open connections (those that have received a SYN and SYN-ACK but not yet the final ACK) gets filled.
3. **Result**: The target cannot accept new connections when the queue is full, leading to a denial of service.

![SYN Flood Attack](tcp2.png)

##### Checking Queue Size

On Ubuntu, check the maximum number of SYN requests the system can handle with:

```bash
sysctl -q net.ipv4.tcp_max_syn_backlog
Observing Connections
Use netstat to observe the state of connections:

bash
Copy code
netstat -nat
SYN-RECV: Represents half-open connections.
ESTABLISHED: Represents completed connections.
SYN Cookie Countermeasure
Enabling/Disabling SYN Cookies
Ubuntu uses SYN cookies as a default measure against SYN flood attacks. You can check and modify SYN cookies settings with:

bash
Copy code
# Check SYN cookies status
sudo sysctl -a | grep syncookies

# Disable SYN cookies
sudo sysctl -w net.ipv4.tcp_syncookies=0

# Enable SYN cookies
sudo sysctl -w net.ipv4.tcp_syncookies=1
Note: These commands only work on the VM, not within the provided container, which is restricted from changing these settings. To modify the SYN cookie setting in a container, add the following to the docker-compose.yml file:

yaml
Copy code
sysctls:
  - net.ipv4.tcp_syncookies=0
Launching the Attack
Compiling and Running the Attack
Compile the Attack Program:

bash
Copy code
gcc -o synflood synflood.c
Run the Attack:

bash
Copy code
# Run the attack from the attacker container
synflood 10.9.0.5 23
Observing the Impact
Before and After Attack: Compare the output of netstat -nat before and during the attack.
Telnet Test: Attempt to telnet into the target machine from another machine.
Observations
Impact on Ubuntu 20.04:

New Connections: Machines that have never connected to the victim before may fail to telnet during the attack.
Existing Connections: Machines that had previously established a connection may still be able to connect during the attack. This behavior does not occur in Ubuntu 16.04 and earlier versions.
Explanation: Ubuntu 20.04 implements a mitigation where 1/4 of the backlog is reserved for known destinations. To remove this effect, run:

bash
Copy code
# Check TCP metrics
ip tcp_metrics show

# Flush TCP metrics
ip tcp_metrics flush
Conclusion
Enabling SYN Cookies:

Enable SYN cookies and re-run the attack. Compare the results with SYN cookies turned off. Take screenshots to document:
SYN Cookies Off: Attack in progress, and telnet attempts are failing.
Screenshot 1: ![TCP Attack with the SYN cookie mechanism](TCP Attack with the SYN cookie mechanism.png)
SYN Cookies On: Attack in progress, and telnet is successful.
Screenshot 2: ![TCP Attack without the SYN cookie mechanism](TCP Attack without the SYN cookie mechanism.png)