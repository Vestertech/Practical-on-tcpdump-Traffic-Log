# How to Read the tcpdump Traffic Log

This guide explains how to identify a brute force attack using `tcpdump`.
1. [Understanding DNS & HTTP Traffic Logs](#understanding-dns--http-traffic-logs)
2. [TCP Handshake and Flags](#tcp-handshake-and-flags)
3. [Identifying a Redirect to a Malicious Site](#identifying-a-redirect-to-a-malicious-site)
4. [Additional Resources](#additional-resources)

## Introduction
`tcpdump` is a powerful command-line packet analyzer used for network monitoring and security analysis. By inspecting traffic logs, we can detect potential threats, such as brute force attacks or DNS hijacking.

14:18:32.192571 IP your.machine.52444 > dns.google.domain: 35084+ A? yummyrecipesforme.com. (24) 14:18:32.204388 IP dns.google.domain > your.machine.52444: 35084 1/0/0 A 203.0.113.22 (40)
---
- The source computer (`your.machine.52444`) sends a DNS resolution request to the DNS server (`dns.google.domain`) for the URL `yummyrecipesforme.com`.
- The DNS server responds with the resolved IP address: `203.0.113.22`.

### TCP Handshake Example:
14:18:36.786501 IP your.machine.36086 > yummyrecipesforme.com.http: Flags [S], seq 2873951608, win 65495, options [mss 65495,sackOK,TS val 3302576859 ecr 0,nop,wscale 7], length 0 14:18:36.786517 IP yummyrecipesforme.com.http > your.machine.36086: Flags [S.], seq 3984334959, ack 2873951609, win 65483, options [mss 65495,sackOK,TS val 3302576859 ecr 3302576859,nop,wscale 7], length 0


- The source computer (`your.machine.36086`) initiates a connection to `yummyrecipesforme.com.http` (port 80).
- The destination acknowledges the request, confirming the connection.

### TCP Flags Overview:
| Flag  | Meaning |
|--------|---------|
| `[S]`  | Connection Start (SYN) |
| `[F]`  | Connection Finish (FIN) |
| `[P]`  | Data Push (PSH) |
| `[R]`  | Connection Reset (RST) |
| `[.]`  | Acknowledgment (ACK) |

### HTTP Request Example:
14:18:36.786589 IP your.machine.36086 > yummyrecipesforme.com.http: Flags [P.], seq 1:74, ack 1, win 512, options [nop,nop,TS val 3302576859 ecr 3302576859], length 73: HTTP: GET / HTTP/1.1


- The log shows an HTTP GET request for `yummyrecipesforme.com` using HTTP/1.1.

## Identifying a Redirect to a Malicious Site

14:20:32.192571 IP your.machine.52444 > dns.google.domain: 21899+ A? greatrecipesforme.com. (24) 14:20:32.204388 IP dns.google.domain > your.machine.52444: 21899 1/0/0 A 192.0.2.172 (40)

14:25:29.576493 IP your.machine.56378 > greatrecipesforme.com.http: Flags [S], seq 1020702883, win 65495, options [mss 65495,sackOK,TS val 3302989649 ecr 0,nop,wscale 7], length 0 14:25:29.576510 IP greatrecipesforme.com.http > your.machine.56378: Flags [S.], seq 1993648018, ack 1020702884, win 65483, options [mss 65495,sackOK,TS val 3302989649 ecr 3302989649,nop,wscale 7], length 0


- The log shows a DNS request resolving to a different website (`greatrecipesforme.com`).
- The source machine is now communicating with the spoofed/malicious website.

## Useful tcpdump Commands used

- **To Capture all traffic:**  
  ```sh
  tcpdump -i eth0
  tcpdump port 80
  tcpdump port 53
  tcpdump -w capture.pcap


## Additional Resources
- **[An Introduction to Using tcpdump](https://opensource.com/article/18/10/introduction-tcpdump)** – Overview of tcpdump commands and output explanations.
- **[tcpdump Cheat Sheet](https://www.comparitech.com/net-admin/tcpdump-cheat-sheet/)** – Commands, packet capturing, protocol codes, and filters.
- **[What is a Computer Port?](https://www.cloudflare.com/learning/network-layer/what-is-a-computer-port/)** – Overview of ports and their common uses.
- **[Service Name and Protocol Port Registry](https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml)** – Database of port numbers and protocols.
- **[How to Capture and Analyze Network Traffic with tcpdump](https://geekflare.com/tcpdump-examples/)** – Guide on using tcpdump for network analysis.
- **[Masterclass – Tcpdump Output Interpretation](https://packetpushers.net/masterclass-tcpdump-interpreting-output/)** – Color-coded reference guide to tcpdump logs.

---

This guide helped me to understand `tcpdump` logs to detect brute force attacks and malicious redirects. 

Talk soon!.




