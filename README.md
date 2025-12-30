# Network Applications Suite (Python)

## Overview

This project implements a suite of core network applications from scratch using Python sockets, demonstrating a practical understanding of network protocols, concurrency, and low-level systems programming.

The suite includes implementations of ICMP Ping, Traceroute, an HTTP Web Server, and an HTTP Proxy Server, all built without external libraries.

## Objectives

- Understand how common network tools work internally
- Apply socket programming and protocol specifications (ICMP, HTTP)
- Handle concurrency, timing, and packet-level operations
- Build reliable network applications using only the Python standard library

## Applications Implemented

### 1. ICMP Ping Client

- Constructs and sends ICMP Echo Request packets
- Receives and parses ICMP Echo Replies
- Calculates round-trip time (RTT) and packet loss
- Displays per-packet statistics and summary metrics
- Implements checksum calculation according to RFC 792

### 2. Traceroute Client

- Discovers network paths using TTL manipulation
- Supports both ICMP and UDP probing
- Identifies intermediate routers via ICMP Time Exceeded messages
- Measures hop-by-hop latency with multiple probes per hop
- Resolves hostnames where available

### 3. HTTP Web Server

- Multi-threaded HTTP/1.1 server
- Serves static files from a local directory
- Handles GET requests with proper URI parsing
- Returns appropriate HTTP status codes (200, 404)
- Supports concurrent client connections using threading

### 4. HTTP Proxy Server

- Acts as an intermediary between clients and origin servers
- Forwards HTTP requests and responses
- Implements disk-based caching to reduce repeated requests
- Differentiates cache hits and cache misses
- Maintains in-memory metadata for cache management

## Technologies Used

- Python 3
- Socket Programming
- ICMP & HTTP Protocols
- Multi-threading
- Raw Sockets
- Linux Networking Concepts

> **Note:** ICMP functionality requires root/administrator privileges due to raw socket usage.

## Project Structure

- `NetworkApplications.py` – Main program containing all network utilities
- `htdocs/` – Directory for static web server content (created at runtime)

## How to Run

Install Python 3.10 or later:
```bash
python3 --version
```

### ICMP Ping
```bash
sudo python3 NetworkApplications.py ping google.com --count 5 --timeout 2
```

### Traceroute
```bash
sudo python3 NetworkApplications.py traceroute google.com --protocol icmp
sudo python3 NetworkApplications.py traceroute google.com --protocol udp
```

### HTTP Web Server
```bash
python3 NetworkApplications.py web --port 8080
```

### HTTP Proxy Server
```bash
python3 NetworkApplications.py proxy --port 8000
```

## Key Learning Outcomes

- Built network tools without relying on system utilities
- Gained hands-on experience with ICMP, TTL, and packet inspection
- Learned how HTTP servers and proxies handle requests internally
- Improved understanding of concurrency and timing in network systems
- Developed robust error handling and protocol-compliant implementations

## Academic Context

This project was developed as part of the Computer Networks coursework at Lancaster University, focusing on network application design and socket programming fundamentals.

## License

MIT License
