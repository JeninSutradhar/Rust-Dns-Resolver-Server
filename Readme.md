# Rust DNS Resolver Server
This project implements a High-Performance DNS (Domain Name System) server in Rust. The server listens for DNS queries, processes them, and responds appropriately.

- **The server reads incoming queries, parses them, and performs a recursive lookup if necessary. The results are then sent back to the client.**

## Features
- Handles standard DNS queries
- Supports recursive DNS lookups
- Logs received queries and responses
- Error handling for malformed requests and server failures

# Usage

### Building the Project
- Clone the repository and navigate to the project directory:
```sh
git clone <repository_url>
cd <repository_directory>
```

### Build the project using Cargo:
```sh
cargo build --release
```

### Running the Server
- To run the DNS server, execute the following command:
```sh
cargo run --release
```
- **By default, the server will start listening on port 8080.**

### Usage
- Once the server is running, you can use the dig command to test it. Open a terminal and run:
```sh
dig @127.0.0.1 -p 8080 example.com
```
- for example 
```sh
dig @127.0.0.1 -p 8080 yahoo.com
```
- **This will send a DNS query for example.com to the server running on localhost at port 8080.**

### Example Output
- When a query is received, the server will log the details:
```sh
Received query: Question { name: "example.com", qtype: A }
Answer: A { domain: "example.com", addr: 93.184.216.34, ttl: 3600 }
```

## Use Cases
- **Local DNS Resolution:** Use this server for resolving DNS queries within a local network.
- **Learning and Experimentation:** Understand the inner workings of DNS by inspecting the code and modifying it.
- **Custom DNS Solutions:** Extend the server to include custom DNS records or integrate with other systems.

## Error Handling
- The server handles the following errors:
1. **Malformed requests:** Responds with FORMERR
2. **Server failures:** Responds with SERVFAIL

## Modifying Server Addresses and Ports
- To change the server address or port, modify the following lines in the main.rs file:

```rust
fn main() -> Result<(), Box<dyn Error>> {
    // Bind to the UDP socket
    let socket = UdpSocket::bind(("0.0.0.0", 8080))?; // Change the address and port here
    
    // Continuously handle incoming queries
    loop {
        match dns::handle_query(&socket) {
            Ok(_) => {},
            Err(e) => eprintln!("An error occurred: {}", e),
        }
    }
}
```
- **Replace "0.0.0.0" with the desired IP address and 8080 with the desired port number.**

# Code Explanation
- This section provides a brief overview of the main components of the DNS server implementation in Rust.

## main.rs
The main.rs file is the entry point of the application. It sets up a UDP socket that listens for incoming DNS queries on a specified address and port. The main function enters an infinite loop, continuously calling the handle_query function to process incoming requests. Any errors encountered during this process are logged to the console.

## byte_packet_buffer
The byte_packet_buffer.rs module provides a buffer implementation for reading and writing raw bytes. This buffer is used to parse DNS packets from incoming data and to serialize DNS packets for outgoing responses. It includes methods for reading and writing various data types, such as strings, integers, and byte arrays.

## dns_packet
The dns_packet.rs module defines the structure of a DNS packet. It includes definitions for the DNS header, questions, answers, and other sections of a DNS message. This module also provides methods for serializing and deserializing DNS packets, making it easy to convert between raw bytes and structured data.

## dns_record
The dns_record.rs module defines the various types of DNS records, such as A, NS, and CNAME records. It includes methods for reading and writing these records to and from the byte buffer. This module also provides functionality for handling the different record types and their associated data.

## dns.rs
The dns.rs module contains the core logic for handling DNS packets. It includes functions for reading and writing DNS packets, processing queries, and generating responses. The handle_query function reads an incoming DNS query from the UDP socket, parses it, and constructs an appropriate response. If a recursive lookup is necessary, the function performs the lookup and includes the results in the response.

- These components work together to provide a fully functional DNS server that can process queries, perform recursive lookups, and generate appropriate responses. Each module is responsible for a specific aspect of the DNS protocol, ensuring a modular and maintainable codebase.