Interview Q&A — Wireshark and Network Basics

1) What is Wireshark used for?
- A network protocol analyzer that captures, decodes, and inspects packets in real time or from saved files for troubleshooting, security analysis, and learning.

2) What is a packet?
- The fundamental unit of data transmitted over a network. It contains headers (metadata like source/destination, ports, sequence numbers) and a payload (the actual data). Different layers (Ethernet/IP/TCP/UDP/HTTP) add their own headers.

3) How to filter packets in Wireshark?
- Display filters (after capture) refine what you see, e.g., `dns`, `http`, `tcp.port == 443`, `ip.addr == 1.1.1.1`, `tcp.flags.syn == 1 && tcp.flags.ack == 0`.
- Capture filters (before capture) limit what is recorded, using BPF syntax, e.g., `port 53`, `host 8.8.8.8`, `tcp and port 80`. Display and capture filters use different syntaxes.

4) What is the difference between TCP and UDP?
- TCP: connection-oriented (3-way handshake), reliable (retransmissions, ordering), flow and congestion control; used when correctness matters (HTTP/1.1, TLS, SMTP).
- UDP: connectionless, no built-in reliability/order; lower latency/overhead; used for DNS, streaming, VoIP, QUIC (note: QUIC runs over UDP but adds reliability at a higher layer).

5) What is a DNS query packet?
- Usually UDP/53 (can be TCP for large messages). A query has QR=0 (request), includes QNAME (domain), QTYPE (A/AAAA/MX), and QCLASS (usually IN). The response has QR=1 with answers (RRs). Example display filter: `dns.flags.response == 0` for queries, `dns.flags.response == 1` for responses.

6) How can packet capture help in troubleshooting?
- Isolates where failures occur (DNS resolution vs TCP handshake vs app layer), reveals retransmissions/timeouts, SSL/TLS handshake errors, high latency paths, server resets, misconfigured MTU, and blocked ports. Correlates timestamps and flows to root-cause issues.

7) What is a protocol?
- A defined set of rules/formats for communication between systems. Protocols exist at different layers (e.g., Ethernet, IP, TCP/UDP, HTTP, DNS, TLS) and interoperate to deliver data end-to-end.

8) Can Wireshark decrypt encrypted traffic?
- Sometimes, with keys/access:
  - TLS: If you have a key log file (`SSLKEYLOGFILE`) from the client, Wireshark can decrypt modern TLS (ECDHE). Server private keys only help older RSA key exchange.
  - Wi‑Fi (WPA2/WPA3-Personal): With the PSK/passphrase and a captured 4-way handshake, frames can be decrypted.
  - Without keys, end-to-end encrypted payloads remain unreadable; you can still analyze metadata (handshakes, SNI, certificates, timing).

