# Network Applications Suite

A Python implementation of essential network utilities including ICMP Ping, Traceroute, HTTP Web Server, and HTTP Proxy. This project demonstrates low-level socket programming, network protocols, and concurrent server handling.

Developed as part of SCC.203 Computer Networks Coursework 1 covering network application development and socket programming fundamentals.

---

## Features

### Task 1.1: ICMP Ping Client
- Sends ICMP Echo Request packets (Type 8) to target hosts
- Receives and processes ICMP Echo Reply packets (Type 0)
- Calculates round-trip time (RTT) for each packet
- Displays packet statistics including TTL, sequence number, and delay
- Computes packet loss percentage and min/avg/max RTT statistics
- Implements RFC 792 ICMP protocol specification
- Handles ICMP error codes (Destination Host/Network Unreachable)
- Custom checksum calculation for packet integrity verification

### Task 1.2: Traceroute Client (Assessed - 50% of CW1)
- Traces the network path to a destination host using TTL manipulation
- Supports both ICMP and UDP protocols (configurable via command line)
- Performs three delay measurements per hop for accuracy
- Displays hop-by-hop latency measurements with hostname resolution
- Handles ICMP Type 11 (TTL Exceeded) messages
- Identifies intermediate routers along the network path
- Configurable timeout for request handling
- Implements Dijkstra-style shortest path discovery

### Task 2.1: HTTP Web Server
- Multi-threaded HTTP/1.1 web server implementation
- Serves static files from local directory
- Handles HTTP GET requests with proper URI parsing
- Returns HTTP 200 OK for successful requests
- Returns HTTP 404 NOT FOUND for missing files
- Configurable port binding (default: 8080)
- Supports concurrent client connections using Python threading
- Complies with RFC 2616 HTTP/1.1 specification

### Task 2.2: Web Proxy Server (Assessed - 50% of CW1)
- Acts as intermediary between clients and web servers
- Forwards HTTP requests from clients to destination servers
- Returns server responses back to requesting clients
- Implements intelligent object caching to disk
- Reduces bandwidth usage through cached content delivery
- Configurable port binding (default: 8000)
- Handles cache hits and cache misses efficiently
- Maintains in-memory data structure for cache management
- Supports HTTP/1.1 GET requests

---

## Project Structure

- `NetworkApplications.py` — Main program containing all network utilities (ping, traceroute, web server, proxy)
- `index.html` — Example HTML file for web server testing (not included in repository)
- `htdocs/` — Directory for web server static content (create as needed)

---

## Requirements

Python 3.10 or newer

Standard library modules only:
- socket
- argparse
- threading
- struct
- time
- select
- binascii
- os
- sys
- traceback
- random

Note: ICMP operations require root/administrator privileges due to raw socket usage

External libraries are explicitly prohibited per coursework requirements

---

## Build and Run Instructions

1. Clone the repository:
   git clone https://github.com/acetrow/networking.git
   cd network-apps-suite

2. Ensure Python 3.10+ is installed:
   python3 --version

3. Run the desired network application:

   For ICMP Ping:
   sudo python3 NetworkApplications.py ping lancaster.ac.uk --count 10 --timeout 2

   For Traceroute (ICMP):
   sudo python3 NetworkApplications.py traceroute lancaster.ac.uk --protocol icmp --timeout 2

   For Traceroute (UDP):
   sudo python3 NetworkApplications.py traceroute lancaster.ac.uk --protocol udp --timeout 2

   For Web Server:
   python3 NetworkApplications.py web --port 8080

   For Proxy Server:
   python3 NetworkApplications.py proxy --port 8000

Note: sudo is required for ping and traceroute due to raw socket operations (SOCK_RAW)

---

## Usage

### ICMP Ping

sudo python3 NetworkApplications.py ping <hostname> [options]

Options:
  --count, -c    Number of ping requests to send (default: 10)
  --timeout, -t  Timeout in seconds (default: 2)

Examples:
sudo python3 NetworkApplications.py ping google.com --count 5
sudo python3 NetworkApplications.py ping bbc.co.uk --timeout 3
sudo python3 NetworkApplications.py p lancaster.ac.uk

### Traceroute

sudo python3 NetworkApplications.py traceroute <hostname> [options]

Options:
  --timeout, -t   Timeout in seconds (default: 2)
  --protocol, -p  Protocol to use: icmp or udp (default: icmp)

Examples:
sudo python3 NetworkApplications.py traceroute lancaster.ac.uk --protocol icmp
sudo python3 NetworkApplications.py traceroute google.com --protocol udp --timeout 3
sudo python3 NetworkApplications.py t mit.edu

### Web Server

python3 NetworkApplications.py web [options]

Options:
  --port, -p  Port number to listen on (default: 8080)

Example:
python3 NetworkApplications.py web --port 8080

Then test with:
curl http://127.0.0.1:8080/index.html

Or visit in browser: http://127.0.0.1:8080/index.html

### Proxy Server

python3 NetworkApplications.py proxy [options]

Options:
  --port, -p  Port number to listen on (default: 8000)

Example:
python3 NetworkApplications.py proxy --port 8000

Then test with:
curl http://neverssl.com --proxy 127.0.0.1:8000
curl http://captive.apple.com --proxy 127.0.0.1:8000

---

## Implementation Details

### ICMP Packet Structure
- Type: 8 (Echo Request) → 0 (Echo Reply)
- Code: 0
- Checksum: 16-bit one's complement (RFC 792)
- Identifier: Process ID (must be > 0)
- Sequence Number: Incremental counter
- Data: Timestamp for RTT calculation

### Traceroute TTL Mechanism
- Starts with TTL = 1, increments until destination reached
- Routers decrement TTL and return ICMP Type 11 when TTL = 0
- Measures delay at each hop by timing request/response
- Stops when receiving ICMP Type 0 (Echo Reply) from destination
- Performs 3 measurements per hop for statistical accuracy

### Web Server Architecture
- Binds to specified port (recommended: > 1024 for unprivileged access)
- Listens for incoming TCP connections
- Parses HTTP GET requests to extract URI
- Serves files from execution directory
- Multi-threaded: spawns new thread per client connection
- Returns proper HTTP status codes (200 OK, 404 NOT FOUND)

### Proxy Caching Strategy
- Checks cache before forwarding requests to origin server
- On cache miss: fetches from server, serves to client, stores in cache
- On cache hit: serves directly from disk cache
- Maintains in-memory index of cached objects
- Writes responses to disk for persistent storage
- No cache validation or replacement policy implemented (per requirements)

### Checksum Algorithm
- Implements RFC 1071 Internet Checksum
- Computes 16-bit one's complement sum
- Handles byte-order conversion for cross-platform compatibility
- Requires dummy checksum value of 0 before calculation

---

## Testing and Debugging

### Recommended Test Hosts
- lancaster.ac.uk (low latency, ~5 hops)
- google.com (reliable ICMP responder)
- bbc.co.uk (UK-based server)
- www.ed.gov (US server for high latency testing)

### HTTP Test Sites (No HSTS)
- http://neverssl.com
- http://captive.apple.com

### Debugging Tools

Use Wireshark to inspect packets:
1. Capture on eth0 for external traffic
2. Capture on lo (loopback) for local testing
3. Apply filters: icmp or http

Compare results with built-in utilities:
ping lancaster.ac.uk
traceroute -I lancaster.ac.uk
curl http://127.0.0.1:8080/index.html

---

## Example Output

### ICMP Ping
Ping to: lancaster.ac.uk...
64 bytes from lancaster.ac.uk (148.88.67.82): icmp_seq=0 ttl=54 time=23.456 ms
64 bytes from lancaster.ac.uk (148.88.67.82): icmp_seq=1 ttl=54 time=22.789 ms
64 bytes from lancaster.ac.uk (148.88.67.82): icmp_seq=2 ttl=54 time=24.123 ms

0.00% packet loss
rtt min/avg/max = 22.79/23.46/24.12 ms

### Traceroute (ICMP)
Traceroute to: lancaster.ac.uk...
1 gateway (192.168.1.1) 1.234 ms  1.456 ms  1.289 ms
2 isp-router (10.0.0.1) 5.678 ms  5.432 ms  5.789 ms
3 * * *
4 lancaster.ac.uk (148.88.67.82) 23.456 ms  23.789 ms  24.123 ms

### Web Server
Web Server starting on port: 8080...
The server is ready to receive
[Server handles GET requests and serves files from local directory]

### Proxy Server
Web Proxy starting on port: 8000...
[Proxy forwards requests, implements caching, serves cached content]

---

## License

This project is released under the MIT License.

Academic Integrity Notice: This coursework is submitted for SCC.203 Computer Networks at Lancaster University. Please respect academic integrity policies.