Wireshark Network Traffic Capture and Analysis

Overview
- Capture live packets with Wireshark, filter by protocol, and summarize findings.
- Includes a step-by-step guide, filter cheat sheet, and a report template.

Objectives
- Capture live traffic on the active interface
- Generate traffic (web browse or ping)
- Filter and identify at least 3 protocols (e.g., DNS, TCP, HTTP/HTTPS, ICMP)
- Export capture as `.pcap`
- Write a short report summarizing protocols and key packets

Deliverables
- `captures/<your-filename>.pcap`
- `REPORT.md` (use the template below)
- Optional: `screenshots/` showing your capture, filters, and statistics

Prerequisites
- Wireshark (free): https://www.wireshark.org/download.html

Install Wireshark
- Windows: Download installer and follow defaults. Allow WinPcap/Npcap if prompted.
- macOS: `brew install --cask wireshark` (or use the DMG). If prompted, allow packet capture permissions.
- Linux (Debian/Ubuntu): `sudo apt update && sudo apt install wireshark`; add your user to `wireshark` group and re-login if needed.

Step-by-Step
1) Identify the active interface
   - Launch Wireshark → Home screen shows interfaces and live packet rates.
   - Pick the interface with the highest changing packet counter (e.g., Wi-Fi/ethernet).

2) Start capture
   - Double-click the interface to start.
   - Optionally set a capture filter (leave empty for full capture; you will filter later).

3) Generate traffic
   - Browse any website (e.g., https://example.com) or run a ping:
     - Windows: `ping 1.1.1.1 -n 4`
     - macOS/Linux: `ping -c 4 1.1.1.1`

4) Stop capture (after ~60 seconds)
   - Click the red square Stop button.

5) Filter by protocol (Display Filters)
   - DNS: `dns`
   - HTTP: `http`
   - TLS/HTTPS: `tls`
   - TCP: `tcp`
   - UDP: `udp`
   - ICMP (ping): `icmp || icmpv6`
   - Source/Dest IP: `ip.addr == 1.1.1.1`
   - TCP handshake (SYN): `tcp.flags.syn == 1 && tcp.flags.ack == 0`
   - TCP retransmissions: `tcp.analysis.retransmission`

6) Identify protocols and traffic types
   - Use Statistics → Protocol Hierarchy to see protocol breakdown.
   - Use Statistics → Conversations (TCP/UDP) to see endpoints and ports.
   - Right-click a flow → “Follow TCP Stream” (or UDP) to see session data.

7) Export capture
   - File → Save As… → choose `captures/<your-filename>.pcap` (pcapng also fine).

8) Summarize your findings
   - Create `REPORT.md` with brief notes: what you captured, top protocols, and 3+ protocol examples with packet details.

Filter Cheat Sheet
- HTTP requests: `http.request`
- HTTP responses: `http.response`
- TLS handshakes: `tls.handshake`
- DNS queries only: `dns.flags.response == 0`
- DNS responses only: `dns.flags.response == 1`
- Traffic to a host: `ip.addr == <IP>` (e.g., `ip.addr == 8.8.8.8`)
- Port filters: `tcp.port == 443` or `udp.port == 53`

Sample Findings (example)
- DNS: Query A for example.com to 1.1.1.1 (UDP/53); Response with IP.
- TCP: 3-way handshake to 93.184.216.34:80 (SYN → SYN/ACK → ACK).
- HTTP: GET / from example.com with 200 OK response.
- ICMP: Echo request/reply to 1.1.1.1 showing round-trip times.
- TLS: ClientHello to 443 with SNI set (e.g., www.google.com).

Report Template (copy into REPORT.md)
---
Title: Wireshark Capture and Protocol Analysis

Date/Time:
Interface used:
Traffic generated:
Capture duration:
File: `captures/<your-filename>.pcap`

1) Overview
- Short description of what was captured and why.

2) Protocols observed
- Summary from Statistics → Protocol Hierarchy (add a screenshot if desired).

3) At least three protocols with packet details
- DNS
  - Filter: `dns`
  - Example packet/frame number:
  - Query/Response details (QNAME, QTYPE, server IP, response IPs):

- TCP
  - Filter: `tcp`
  - Example handshake (SYN, SYN/ACK, ACK) frame numbers:
  - Source/Destination IPs and ports:

- HTTP or TLS (HTTPS)
  - Filter: `http` or `tls`
  - Example request/handshake details:

- (Optional) ICMP
  - Filter: `icmp || icmpv6`
  - Echo request/reply details and RTTs:

4) Notable observations
- Retransmissions? DNS failures? Slow response? Any anomalies?

5) Conclusion
- What you learned; how filtering aided analysis.
---

Repository Structure
- `captures/` — place your `.pcap` or `.pcapng` files here
- `screenshots/` — optional screenshots (filters, protocol hierarchy)
- `REPORT.md` — your written summary based on the template
- `README.md` — this guide
- `INTERVIEW_QA.md` — concise answers to the interview questions

Submission
- Push this repo to GitHub.
- Include the capture file(s), report, and any screenshots.
- Submit the repository link using your program’s submission portal.

Ethics & Safety
- Only capture traffic you are authorized to analyze.
- Avoid capturing sensitive data on shared networks.
- Prefer test sites and non-sensitive traffic for practice.

