# TCP Attacks Lab

## Summary
- Hands-on TCP attacks: SYN Flooding, TCP RST attacks, TCP Session Hijacking, Reverse Shell.
- Includes attack execution, mitigation techniques, and screenshots.

## Lab Setup
- **Download:** [Lab Setup ZIP](https://seedsecuritylabs.org/Labs_20.04/Networking/TCP_Attacks/), [Docker Manual](https://github.com/seed-labs/seed-labs/blob/master/manuals/docker/SEEDManual-Container.md)
- **Ensure Docker is set up:** Follow [Lab PDF](https://seedsecuritylabs.org/Labs_20.04/Files/TCP_Attacks/TCP_Attacks.pdf) for instructions.

---

### SYN Flooding Attack
- **Objective:** Launch SYN Flood attack, overwhelm SYN queue, and test mitigation with SYN cookies.
- **Commands:**
  - Check SYN queue size: `sysctl -q net.ipv4.tcp_max_syn_backlog`
  - Monitor queue: `netstat -nat`
  - Enable SYN cookies: `sudo sysctl -w net.ipv4.tcp_syncookies=1`
  - Disable SYN cookies: `sudo sysctl -w net.ipv4.tcp_syncookies=0`
  - Launch attack: `gcc -o synflood synflood.c` and `synflood 10.9.0.5 23`
- **Screenshots:**
  - Without SYN cookies:
    ![TCP Attack without SYN cookie](images/TCP%20Attack%20without%20the%20SYN%20cookie%20mechanism.png)
  - With SYN cookies:
    ![TCP Attack with SYN cookie](images/TCP%20Attack%20with%20the%20SYN%20cookie%20mechanism.png)

---

### TCP RST Attack
- **Objective:** Forge RST packets to terminate active Telnet connections.
- **Steps:**
  - Prepare Scapy code to create RST packets.
  - Capture values with Wireshark (SEQ/ACK numbers).
  - Automate with sniffing-spoofing techniques.
- **Screenshots:**
  - Python Scapy code:
    ![Q2 Python Scapy code](images/Q2%20Python_Scapy%20code.png)
  - Wireshark outputs:
    ![Wireshark 1](images/Q2%20TCP%20RST%20ATTACK%20Wireshark%201.png)
    ![Wireshark 2](images/Q2%20TCP%20RST%20ATTACK%20Wireshark%202.png)
- **Bonus (Optional):**
  ![Bonus](images/Bonus_Q2_1.png) ![Bonus](images/Bonus_Q2_2.png)

---

### TCP Session Hijacking
- **Objective:** Inject malicious content into an existing session.
- **Steps:**
  - Use Scapy to craft packets with Wireshark-captured SEQ/ACK numbers.
  - Inject commands into the session.
- **Screenshots:**
  - Python code:
    ![Q3 Python code](images/Q3%20Python_Scapy%20code.png)
  - Hijacking Attack before/after:
    ![Before](images/Q3%20TCP%20Session%20Hijacking%20Attack%20before.png) ![After](images/Q3%20TCP%20Session%20Hijacking%20Attack%20after.png)

---

### Reverse Shell Attack
- **Objective:** Gain shell access by injecting a reverse shell command into a session.
- **Steps:**
  - Set up netcat: `nc -lvp 9090`
  - Inject reverse shell command using Scapy.
- **Screenshot:**
  - Reverse shell output:
    ![Reverse shell](images/Q4_reverse_shell.png)

---

## Lessons Learned
- **SYN Flooding:** Learned to overwhelm TCP SYN queue and mitigate with SYN cookies.
- **TCP RST Attack:** Disrupted connections using RST packets with Scapy.
- **Session Hijacking:** Injected malicious commands into an active session.
- **Reverse Shell:** Gained remote access via hijacking TCP session and injecting reverse shell commands.

