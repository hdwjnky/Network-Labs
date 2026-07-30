# TCP Round-Trip Time (RTT) Estimation with Wireshark

## What I Did
Analyzed a captured TCP trace (`lab15_tcp_trace.pcap`) to measure round-trip time between a client and web server, then used the measured values to model how TCP dynamically estimates RTT and sets retransmission timeouts.

## Tools Used
- Wireshark (packet capture analysis)
- Spreadsheet calculations (Jacobson-Karels algorithm)

## How I Did It
1. Extracted timestamps for the first 10 request/response packet pairs from the trace and calculated `SampleRTT` for each by subtracting send time from the corresponding ACK time.
2. Fed the sample values into the **Jacobson-Karels algorithm** to compute a smoothed `EstimatedRTT`, deviation, and dynamic `Timeout` value for each packet, using standard parameters (α = 0.125, β = 0.25, K = 4).
3. Plotted Measured RTT vs. Packet and Estimated RTT vs. Packet to visualize how the algorithm tracks and smooths real network variation.

## What I Found
- Measured RTT varied noticeably across the 10 packets (roughly 25–49 ms), reflecting normal jitter in network delay.
- The Estimated RTT curve tracked the measured values but rose and fell more gradually, since it's a weighted average rather than a raw sample.
- When RTT spikes above a threshold (e.g. 150 ms), it typically signals congestion or queuing delay. The Jacobson-Karels algorithm responds by increasing both EstimatedRTT and Timeout, which helps TCP avoid firing off unnecessary retransmissions during temporary slowdowns, then gradually tightens back up once the network settles.

## Key Takeaway
This lab showed why TCP doesn't set a single fixed timeout value — a smoothed, adaptive estimate is what lets a connection stay reliable across changing network conditions without either timing out too aggressively or too slowly.

*(Add your RTT and Timeout-vs-Packet screenshots here.)*
