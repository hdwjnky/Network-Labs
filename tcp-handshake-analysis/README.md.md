# TCP Connection Analysis with Wireshark

## What I Did
Captured and analyzed a live HTTP session to walk through TCP's three-way handshake, sequence/acknowledgment number behavior, and per-segment round-trip time for a real client-server exchange.

## Tools Used
- Wireshark (packet capture, TCP stream filtering, follow-stream analysis)

## How I Did It
1. Captured traffic between a client (`192.168.1.102`) and a web server (`gaia.cs.umass.edu`, `128.119.245.12`) on TCP port 80.
2. Filtered the capture to isolate a single TCP stream and identified the handshake packets by their TCP flags (SYN, SYN-ACK, ACK).
3. Traced sequence and acknowledgment numbers through the handshake and into the HTTP POST request that followed.
4. Pulled send/ACK timestamps for six segments in the exchange and calculated RTT for each.

## What I Found
- **Handshake:** The client's SYN carried relative sequence number 0. The server's SYN-ACK also showed relative sequence 0, with an acknowledgment number of 1 — the client's initial sequence number plus 1, since SYN consumes a sequence number.
- **HTTP POST:** The HTTP POST request exchange begins at Frame 199 (Seq=163769). Inspecting Frame 196 in the stream details confirmed a relative sequence number of 164041 with a segment length of 50 bytes and relative ACK 1, showing the byte stream's progression right before the server responded.
- **RTT per segment:** Measured RTT rose across the six segments tracked (from ~0.027s to ~0.139s), showing how delay can increase over the life of a single connection rather than staying constant.

| Segment | Seq. Number | RTT |
|---|---|---|
| 1 | 1 | 0.027s |
| 2 | 566 | 0.036s |
| 3 | 2026 | 0.070s |
| 4 | 3486 | 0.069s |
| 5 | 4946 | 0.092s |
| 6 | 6406 | 0.139s |

## Key Takeaway
Reading the handshake and sequence numbers directly from packet captures made TCP's reliability mechanism tangible — rather than a diagram in a textbook, it's a specific pair of numbers you can point to in every single packet.

*(Add your Wireshark screenshots here.)*
