# Cybersecurity Internship - Task 1: Local Network Port Scan

## 1. Objective
To map and discover open logical ports and active services within a local network to understand exposure risks, protocol mechanics, and the attack surface of an internal environment[cite: 6, 16].

## 2. Methodology and Tools
**Tool Used:** Nmap (v7.99) command-line interface, executed natively with administrative privileges on the primary Windows host
**Command Executed:** `nmap -sS -v -oN C:\Users\Public\Desktop\result_scan.txt 192.168.X.X/24`
**Network Mapping:** Scan performed across a sample space of 256 local IPv4 addresses (`/24`)

---

## 3. Masked Scan Results (Official Log)

```text
# Nmap 7.99 scan initiated Mon Jun 29 12:53:37 2026 as: nmap -sS -v -oN C:\\Users\\Public\\Desktop\\result_scan.txt 192.168.0.0/24
Increasing send delay for 192.168.0.X from 0 to 5 due to 11 out of 20 dropped probes since last increase.
Nmap scan report for 192.168.0.X [host down]
... [Hosts from 192.168.0.2 to 192.168.X.X marked as down have been omitted for clarity] ...

Nmap scan report for 192.168.X.X (Gateway / Router Device)
Host is up (0.0050s latency).
Not shown: 995 closed tcp ports (reset)
PORT     STATE    SERVICE
22/tcp   filtered ssh
23/tcp   filtered telnet
80/tcp   open     http
443/tcp  open     https
5000/tcp open     upnp
MAC Address: 06:02:B8:XX:XX:XX (Protected)

Nmap scan report for 192.168.X.X (ZTE Internal Device)
Host is up (0.0085s latency).
All 1000 scanned ports on 192.168.X.X are in ignored states.
Not shown: 1000 filtered tcp ports (no-response)
MAC Address: 30:42:40:XX:XX:XX (Protected)

Nmap scan report for 192.168.0.X (Primary Host - Windows)
Host is up (0.0011s latency).
Not shown: 995 closed tcp ports (reset)
PORT     STATE SERVICE
135/tcp  open  msrpc
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
2869/tcp open  icslap
5357/tcp open  wsdapi

### 4. Services Research and Risk Analysis

* **Router (Ports 80 & 443 - HTTP/HTTPS):** Hosts the router's web interface. Risk of complete takeover if default credentials aren't changed.
* **Router (Port 5000 - UPnP):** Used for automatic media device discovery. Vulnerable to legacy DoS attacks.
* **Router (Ports 22 & 23 - SSH/Telnet):** State is "Filtered" by firewall rules. Telnet transmits in plaintext and is highly risky if exposed.
* **Windows Host (Port 135 - MSRPC):** Handles message exchanges between remote processes in Windows.
* **Windows Host (Ports 139 & 445 - NetBIOS/SMB):** Used for local file and printer sharing. Port 445 is highly targeted by exploits (like EternalBlue/WannaCry), requiring constant updates.
* **Windows Host (Ports 2869 & 5357 - UPnP/WSDAPI):** Used for dynamic discovery of network devices and smart printers.

---

### 5. Interview Questions (Summary)

1. **What is an open port?** A communication endpoint where an application is actively listening and ready to accept network connections.
2. **How does a TCP SYN scan work?** It sends a SYN packet. If it receives a SYN-ACK, the port is open, and Nmap immediately sends a RST packet to close the connection before it's fully logged. If it receives a RST, the port is closed.
3. **What are the risks of open ports?** They increase the attack surface. Insecure, unpatched, or misconfigured services can be exploited for unauthorized access or data theft.
4. **TCP vs. UDP scanning:** TCP scanning is connection-oriented and reliable, forcing clear responses (SYN-ACK/RST). UDP scanning is connectionless, slower, and prone to false positives because a lack of response is ambiguous (open|filtered).
5. **How to secure open ports?** Disable unnecessary services, apply security patches regularly, enforce strong multi-factor or cryptographic authentication, and use firewalls to restrict access.
6. **What is a firewall's role?** It filters incoming and outgoing traffic based on security rules, dropping or rejecting packets before they can reach the system's ports.
7. **What is a port scan?** A reconnaissance technique to find active hosts and exposed services, used by attackers to map targets and find exploit vectors.
8. **How does Wireshark complement Nmap?** While Nmap provides a structured summary of active hosts and open ports, Wireshark works as a packet analyzer that captures raw network traffic in real time. It complements port scanning by allowing a deep, bit-by-bit examination of flag structures (such as individual SYN, SYN-ACK, and RST packets), helping analysts validate legitimate traffic, debug security rules, and detect evasive scanning techniques.

---
*(Traduzido para inglês)*

Read data files from: C:\Program Files (x86)\Nmap
# Nmap done at Mon Jun 29 12:54:18 2026 -- 256 IP addresses (3 hosts up) scanned in 40.75 seconds
