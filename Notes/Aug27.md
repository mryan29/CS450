# August 27th, 2018
6-8 Programming assignments over course of semester
*hosts*:endpoints
*link*:plumbing

# The internet
- global scale
- general purpose
	- not for one application
- heterogeneous-technologies
	- used by both new and old machines speaking the same language
- public
- computer network

# Nuts and bolts
*hosts, end-systems*: millions of connected computing devices i.e. pcs, servers, smartphones, apps

*communication links*: fiber, copper, radio, satellite

*routers*: forward data thru network

# Lecture 3 - Layering
- Addressing: How to specify a node
- Routing: Which path to follow
- Flow Control: Avoid congestions
- Security: etc

# Example: Transferring file from point A to point B
App view: authenticate client, ensure permissions, check file exists, encrypt if confidential

Network view ...

# TCP/IP Layering
&uarr; Highest level

&nbsp; **Application layer**: telnet, ftp, email, www, AFS

&nbsp; **Transport layer**: tcp, udp

&nbsp; **Network layer**: ip, icmp, igmp

&nbsp; **Data Link layer**: device drivers i.e. ethernet, wifi, Pos, T1

&nbsp; **Physical layer**

&darr; Lowest level

Computers (hosts) implement all 5 layers.
Routers (gateways) only have the bottom three layers.

## Data Link layer
Low level

**Service**: transfer of frames over a link

**Functions**: 
- synchronization
- channel access
	- if shared medium, only one can access at a time
- error/flow control
	- check that senders aren't sending more than receiver can access

## Network Layer (IP)
**Service**: Moves packets inside the network

**Functions**:
- routing
- addressing

One of most complex layers

### IP Delivery Model
Best-effort delivery

&nbsp; If a packet being sent is lost, network layer won't promise you anything

Given a packet, send to remote point but it could be lost, reordered, delayed

## Transport Layer
**Service**: Controls delivery of data between hosts

**Functions**: 
- connection establishment
- termination, 
- error/flow control

Provides these functions beyond the link that the link layer provides them

Delivery models/protocols:
	
&nbsp; tcp: use if you want reliability/your program can't tolerate packet loss, a little slow

&nbps; udp: use for media playback/streaming/gaming, where loss is ok but need it to be faster

q: does ping work for both tcp and udp? idts
	
### TCP and UDP
Both sit on top of IP

TCP:
- connection oriented
- ensure reliable, in-order delivery
- mechanisms for congestion control
- but latencies could be high

UDP:
- barebones functionality
- connectionless
- retains ip delivery model

## Application Layer
**Service**: Handles details of application programs

**Functions**: 

# Key Concepts
**Service**: what a layer does

**Interface**: how to access the service (i.e. socket interface)

**Protocol**: how the service is implemented (sets of rules/formats that govern communication btwn peers)
