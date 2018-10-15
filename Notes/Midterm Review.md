# Protocol Layers
Each layer provides its service by:
1. performing actions w/i that layer
2. using services of layer directly below it

**Internet protocol stack**

| |
| ------------|
| Application |
| Transport |
| Network |
| Link |
| Physical| 

**7 layer ISO OSI Reference Model**

| |
| --- |
| Application |
| Presentation |
| Session |
| Transport |
| Network |
| Link |
| Physical |

| IP Stack Layer | Implementation | Service | Protocols |
| --- | --- | --- | --- |
| Application | SW | Exchanges packets of information (**messages**) w/ other systems' application | HTTP, SMTP, FTP, DNS |
| Transport | SW | Transports app-layer messages (**segment**: transport layer packet) between application endpoints, passes segment and destination address from source host to network layer | TCP, UDP |
| Network | HW/SW | Moves network-layer packets (**datagrams**) from one host to another, provides the service of delivering the segment to the transport layer in the destination host, routes a datagram thru a series of routers between the source and destination, passes datagram down to the link layer| IP |
| Link | HW | Delivers the datagram (**frames**: link-layer packet) to the next node/network element along the route and sends the datagram up to the network layer | Ethernet, Wifi, DOCSIS, PPP |
| Physical | HW | Move individual bits w/i the frame from one node to the next | twisted-pair copper wire, single-mode fiber optics |

| OSI Stack Layer | Implementation | Service | Protocols |
| --- | --- | --- | --- |
| Presentation | | Provide services (encryption, compression, description) tht allow communicating applications to interpret the meaning of data exchanged | |
| Session | | Provides for delimiting and synchronization of data exchange, including means for checkpointing and recovery scheme | |

**Pros of Layering**
- structured way to discuss sys components
- modularity makes it easier to updatecomponents

**Cons of Layering**
- one layer map duplicate lower-layer functionality
- functionality at one layer may need info present only in another layer

## Protocols
| Protocol | Description |
| --- | --- |
| HTTP | Provides for web document request and transfer |
| SMTP | Provides for transfer of email-messages |
| FTP | Provides for transfer of files between two end systems|
| DNS | Translates domain names to network address,etc |
| TCP | Provides connection-oriented service to its applications, etc. |
| UDP | Provides connectionless service to its applications | 
| IP | Defines fields in datagram as well as how end systems/routers act on them, necessary for all Internet components tht have a network layer |

## Encapsulation
Host --> app-layer message M --> transport layer --> adds on header info to be used by recevier side transport layer H<sub>t</sub> i.e. error-detection bits, M+H<sub>t</sub> = t layer segment --> network layer --> adds header info H<sub>n>/sub> i.e. source/destination addresses, = n layer datagram --> link layer --> adds header info to create link layerframe 

At each layer, packet is composed of two fields:
1. header fields
2. payload field (typicaly packet from layer above)

# Security
**Malware**: malicious software,

**Botnet**: network of compromised devices,

**Self-replicating**, 

**Viruses**: malware that require user interaction,

**Worms**: malware that can enter device without user interaction,

** Denial of Service (DoS) attacks**: renders piece of infrastructure as unusable
- vulnerability:
- bandiwth flooding:
- connection flooding: 

**distributed dos**: attacker controls multiple sources and has each source blast traffic at the target
...

# 2.1 Application Layer - Principles of Network Applications
## Network Application architectures
network architecture: fixed
application architecture: desgined by app developer
**client-server architecture**: always-on host (server) which services requests from many other hosts (clients)
- clients don't communicate directly ww each other
- server has fixed, well known IP address
- used by: Web, FTP, Telnet, email
- **data center** may be used to house a large number of hosts and create powerful virtual server for handling a lot of requests

**p2p architecture**: direct communication between pairs of intermittently connected hosts (peers
- minimal/no reliance on dedicated servers in data centers
- used by file sharing (Bittorrent), internet telephont (skype)
- pros: 
  - self-scalability
    - each peer generates workload by requests, but also add service capacity by distributing files to other peers
  - cost effective: little server infrastructure/bandwith
- future challenges:
  - ISP friendly: most ISPs dimensioned for more downstream than upstream traffic
  - security: due to open nature/highly distributed, hard to be secure
  - incentives: convincing users to volunteer bandwith, storage, computation resources
  
**hybrid architecture**: combines both elements
- used by: instant messaging apps
- servers track ip address of users, messages sent directly between user hosts


## processes communicating
**process**: program running within an end system

communcate by sending **messages** across network

client: process that initiates communication

server: waits to be contacted

**socket**: (aka API) what messages are sent into and received from, sw interface between proccess (application layer) and transport layer

### addressing processes
to identify receiving process we need:
1. address of host (IP address)
2. port number: the process running in the host
  a. Web servers use port 80
  b. mail server process uses port 25
  
 ## transport services available to applications
 ### reliable data transfer
 aka guaranteed data delivery service
 
 **loss tolerant apps**: multimedia apps where it results in small glitch
 
 **throughput**: the rate at which the sending process can deliver bits to the receiving process
 **bandwidth sensitive apps**: have throughput applications (i.e. multimedia)
 **elastic apps**: make us of as much throughput as available (i.e. email, file transfer, web transfer)
 
 **timing, security**
 
 ## Application-layer protocols
 defines types of messages exchanged and syntax
 - i.e. HTTP for the Web
 
 # 2.2 HTTP
 ## Overview
 HTTP: Web's app-layer protocol
 - implemented in client (browser) and server program
 - web page: consists of objects + base HTML file
 - object: file (i.e. html, jpeg, etc) addressable by single URL
 - uses TCP, client-server application architecture, persistent (defualt) but can be configured to use nonpersistent
 - **stateless protocol**: no stored state information about client 
 - pull protocol
 - encapsulation
 
 
 URL components:
 <protocol>://<host>:<port>/<path>?<query>
 
 Process:
 - initiates TCP connection w server
 - browser and server processes access TCP thru sockets
 - client sends HTTP request into socket and receives HTTP response from socket, vice versa for server

 
**Non and Persistent connections**
**non-persistent**: each request/response pair sent over separate TCP connection, each connection closed after server sends object

cons:
- new connection must be established and maintained for each object requested
- each object suffers delivery delaay of two RTTS (one to establish TCP connection and one to request/receive object)

**persistent**: all request/response pairs sent over same TCP connection, TCP connection left open after server sends response

 
 ## HTTP w/ Non-persistent
 1. HTTP client initiates TCP connection to <servername> on <port number (80)> through sockets
 2. HTTP client sends HTTP request message to server via socket
 3. HTTP server receives request via socket, retrieves some object from storage, encapsulates object in HTTP response, and sends response via socket
 4. HTTP server tells TCP to close TCP connection
 5. HTTP client receives response, TCP connection terminates
 6. HTTP client extracts file from response and finds references to each object, repeating the first 4 steps for each object
  
**round-trip time RTT**: time for small packet to travel from client to server and back to client
- includes packet propagation delays
- packet queing delays
- packet processing delays

**three-way handshake**
1. the client sends a small TCP segment to the server
2. the server acknowledges and responds with a small TCP segment
3. the client acknowledges back to the server

- after third handshakeand request message arrives at server, server sends HTML file into TCP connection. this takes up another RTT
- first two parts take one RTT for non-persistent, so total response time is 2 RTTs + transmission time at server of HTML file

## HTTP w/ Persistent
- requests made w same connection, made back to back w/o waiting for pending requests (pielining)
- connection closed when not used for some set timeout interval
- objects are sent back to back when requests are made back to back

## HTTP message format

### HTTP Request Message
```
GET /somedir/page.html HTTP/1.1 
Host: www.someschool.edu
Connection: close 
User-agent: Mozilla/5.0 
Accept-language: fr
```
aka
```
<request line> made up of:
  <method>: <url> <HTTP version>
  <method> can be GET (browser requests object), POST, HEAD, PUT, DELETE
<header lines> made up of:
  <Host:> host on which object resides
  <Connection: close> browser telling server to close after sending requested object (non-presistent)
  <User-agent:> browser type making request
  <Accept-language:> language
<entity body>
```
with GET method, entity body is empty
with POST (used in forms), body includes input from form fields
with HEAD: server responds with an HTTP message but leaves out requested object (debugging)
with PUT: allows user to upload object to specific path on web server
with DELETE: allows for deletion of object on web server

### HTTP Response 
```
HTTP/1.1 200 OK
Connection: close
Date: Time and date when HTTP response was created/sent by server
Server: Apache/2.2.3 (CentOS) 
Last-Modified: ..
Conent-Length: <number of bytes in object being sent>
Content-Type: 

(data...)
```
aka
```
<status line> containing:
  <protocol version>, <status code>, <status message>
<6 header lines>
<entitity body> requested object
```
**status codes**:
- 200 OK: request succeeded and info returned in response
- 301 Moved Permanently: requested object permanently moved, new url specified in Location: header of response
- 400 Bad request: request couldnt be understood by server
- 404 not found: requested doc doesn't exist on this server
- 505 HTTP version not supported: requested http protocol not supported by this server

## Cookies
allow sites to keep track of users

components:
1. cookie header line in http response
2. cookie header line in http request
3. cookie file kept on user's end system and managed by user's browser
4. back-end db at web site

process:
- request comes into server
- server creates unique id number and enters it into db
- server responds to browser including the set-cookie header in the response, containing the id number
- browser receives response, sees header, appends line to cookie file including hostname of server and the id number passed
- each time browser requests a page, it consults cookie file, extracts the id for the site, and puts a <Cookie:> header line in the http request with the id number

## Web cache aka proxy server
- satisfies http requests on behalf of origin web server
- has its own disk storage and keeps copies of recently requested objects
- both server and client at same time
- pros: reduces resposne time for requests, reduces traffic on instutions access link and therefore reduces traffic in internet as whole

> do some calculations here

process:
1. browser establishes tcp connection to web cache and sends http request for object to web cache
2. web cache checks to see if it has copy of object stored locally. if so, returns object in http response
3. if not, web cache opens tcp connection to origin server and sends http request for object into the cache-to-server tcp connection. after receiveing request, server sends object in http response to web cache
4. web cache receives object, stores copy in its locall storage and sends copy in http response to client browser in browser to web cache tcp connnection

## conditional GET
- problem: copy of object in cache may be stale
- solution: conditional GET which includes an if-modified-since header line

# Email
3 components:
- user agents: allow users to read/reply/ etc messages
- mail servers
- simple mail transfer protocol SMTP 
  - client side (sender) and server (recipient), both sides run on ever mail server
  - requires data to be encoded to ASCII and then being decoded (unlike http)

- each recipient has mailbox and a mail server
- if not delivered, goes into a message queue in the sender's mail server
- uses TCP, persistent connections
- no itnermediate mail servers
- push protocol
- no encapsulation
- client-server architecture

process:
1. user invokes user agent and provides senders email address and message 
2. user agent sends message to senders mail server where its placed in message queue
3. client side of senders smtp sees message in message queue and opens TCP connection to smtp server running on receiver's side
4. after some smtp handshaking, smtp client sends message into tcp conenction
5. server side of smtp receives message and receiver's mail server places message in mailbox
6. receiver invokes user agent to read message

commands on client side:
HELO, MAIL FROM, RCPT TO, DATA, QUIT

## Mail format
<header>: From: To: Subject: (part of the mail message itself)
  
## Mail protocols:
POP3

IMAP

HTTP

# DNS
translates hostnames to IP addresses
1. distributed db implemented in hierarchy of dns servers
2. app layer protocol allowing hosts to query the distributed db

- uses UDP, port 53
- emplyed by other applayer protocols
- adds additional delay but ip addr often cached in nearby dns server
- no single dns server has all of mappings for all hosts

services:
1. host aliasing: dns obtains canonical hostname for supplied alias hostname as well as ip addr of the host
2. mail server aliasing: dns can obtain canonical hostname for supplied alias hostname as well as ip addr of the host
3. load distribution: for sets of ip addresses mapped to one canonical host name, dns rotates the ip address provided to distribute traffic among replicated servers

process for http using dns:
1. user machine runs on client side of dns app
2. browser extracts hostname from url and passes hostname to client side of dns app
3. dns client sends query containing hostname to dns server
4. dns client eventually receives a reply, including ip address for host name
5. browser receives ip address and initiates tcp connection to http server process at port 80 at that ip address

3 classes of dns servers:
1. root dns servers
2. top level domain dns servers: responsible for top level domains like com, org, net, edu
3. authoritative: houses organizations dns records tht map the names of hosts to ip addr

to get ip addr, client contacts root, root provides ip addresses for tld, client contacts one of tld, this returns the ip of authoritative, client contacts authoritative, which returns the ip addr for the hostname

other type: local dns server: each isp has one
- when host makes a dns query, query sent to local dns server, which forwards the query into the dns server hierarchy like a proxy

**recursive query**: query from requesting host to local dns server, remaining are **iterative**

dns caching:
- in query chain, when dns server receives dns reply, it can cache mapping to local memory
- cached info is discareded after roughly two days
- local dns can cache ip addr of tld, allowing bypass of root dns servers

## dns records and messages
resource records RR contains:
1. Name
2. Value
3. Type
  a. if = A, Name=hostname, Value=IPaddr, provides standard hostname to ip addr mapping
  b. if = NS, Name=domain, Value=hostname of authoritative dns server tht knows how to obtain ip addr for hosts in domain, used to route dns queries further
  c. if = CNAME, Value=canonical hostname for alias hostname Name, provides querying hosts conanonical name for host name
  d. if = MX, Value = canonical name of mail server tht has alias hostname Name, allows hostnames of mail servers to have simple aliases
4. TTL : time to live (when it should be removed from cache)

- if a dns server is authoritative for a particular hostname, then it will contain a type a record for the host name
- if not authoritative for hostname, it will contain a type ns record for thte domain tht incldudes the hostname and a type a record tht provides the ip of the dns server in the value field of the ns record

## dns message format
1. header section
2. question section
3. answer section
4. authority section: records of other authoritative servers
5. addtional section

registrar: verifies uniqueness of domain name and enters it into DNS database
enters type NS and type A records into TLD servers

# P2P
file distribution,distributed hash table

distribution time: time to get copy of file to all N peers
 
 # UDP
 - minimal services
 - connectionless: no handshaking before processes communicate
 - unreliable data transfer: no guarantee message will reach receiving process, may be out of order
 - no congestion-control: sender can pump data into network layer at any rate
 - used by internet telephony to circumvent tcp's congestion control
 


  



---
# 3.4 Reliable Data Transfer
reliable channel: no transferred data bits corrupted/lost, delivered in order sent (i.e. TCP)
reliable data transfer protocol's job
difficult bc layer below this may be unreliable (i.e. TCP on top of unreliable IP)
**unidirectional data transfer**= data transfer from sender to receiving (as opposed to bidirectional)
still, sending and receiving need to transmit packets in both directions

## Perfectly reliable chanel
### Procedure
sender accepts data from upper layer, creates packet containing the data, and sends packet into channel

receiver receives packet from underlying channel, removes data from packet, passes data up to the upper layer

### Notes
- no diff between unit of data and packet here
- no need for receiver to provide any feedback bc nothing can go wrong
- no need for receiver to ask sender to slow down

## With bit errors
**positive/negative acknowledgements**: allow receiver to let sender know if/what has been received correctly and what needs repeating
--> **Automatic Repeat reQuest protocols**
- needs error detections mechanism (error bits in checksum field of packet)
- needs receiver feedback: positive (ACK) and negative (NAK) acknowledgements
  - 1 bit long: 0 could indicate NAK and 1 could indicate ACK
- needs retransmission

### Procedure
- send side waits for data to be passed down from upper layer, then creates a packet containing data to be sent along with checksum packet and sends the packet
- sender waits for ACK or NAK packet from receiver
- if ACK received, sender knows most recently transmitted packed has been received and returns to waiting for data
- if NAK, protocol retransmits last packet and waits again for ACK or NAK
- receiver replies w either ACK or NAK

### Notes
- when sender is in wait for ACK or NAK state, it cant get more data from upper layer --> sender wont send new peice of data til receiver has correctly received the current one (**stop and wait protocol**)
- ACK or NAK could be corrupted
  - solution: **sequence number**: new field in data packet where sender numbers its data packets
  - recevier checks this number to determine whether or not the packet is a retransmission
  - one bit
  - when out of order packet received, receiver sends ACK for packet OR
  - when corrupted packet received, receiver ends ACK for last corrrectly received packet (**duplicate ACKs**)
  - sender knows receiver didn't correctly receive the following packet

## lossy channel with bit errors
- need to detect packet loss and what to do when it occurs (earlier techniques allow us to address the second)
- sender waits at least as long as round trip delay between sender and receiver plus processing time
- possibility of **duplicate data packets** if delay occurs although it might not have been lost
- **countdown timer** interrupts sender after given amt of time
- sender needs to be able to start timer each time a apacket is sent, respond to timer interrupt, stop timer
- **alternating bit protocol** --> packet sequence numbers

## Pipeline
pipelinng: allows sender to transmit multiple n packets before having to wait for acknowledgments, increasing utilization of sender by n * old utilization

### cons
- range of sequence numbers must be increased 
- sender and receiver sides of protocols have to buffer >1 packet

### calculations
- Given round trip propogation delay between end systems (RTT) = 30 milliseconds
- Connected by channel w transmission rate R of 1 Gbps (10<super>9</super> bits per second)
- Packet size L of 1k bytes (8k bits) including header fields and data
- Time to transmit packet into 1Gbps link:
- d<sub>trans</sub> = L/R = 8000 bits/packet / 10<super>9</super> bits/s = 8 microseconds

- if sender begins sending packet at t=0, then at t = L/R = 8 microseconds the last bit enters the channel aat the sender side

- packet then makes 15 msec journey w/ last bit of packet emerging at receiver at t = RTT/2 + L/R = 15.008 msec

- ACK emerges back at sender at t = RTT + L/R = 30.008 msec

- utilization of the sender/channel: fraction of time sender is actually busy sending bits into the channel (.008 of 30.008 here)
- stop-and-wait protocol:
  - U<sub>sender</sub> = (L/R) / (RTT + L/R) = .008/30.008 = .00027
  


### go-back-n (GBN)
in GBN protocol: sender can transmit multiple packets w/o waiting for acknowledgment, but constrained to some max N of unacknowledged packets in pipeline --> for flow control
- N = window size
- GBN = sliding-window protocol

GBN sender responds to 3 events:
- invocation from above: sender checks if window is full and if not, packet is created and sent, else sender returns data back to upper layer
- receipt of an ACK: *cumulative acknowledgment*: acknowledgemnt for a packet w sequence number n aka all packets w sequence number up to and including n have been correctly received by receiver
- timeout: same as in stop-and-wait

#### determine optimal sliding window size
- given packet size P = 1000 bytes, latency L= 100 ms, link of l = 1GB/s 
- P(bytes) * L(ms) * l(GB/s) = x packets (no conversions)



#### Notes
- fast retransmit: dont wait for timeout if we get n duplicate ACKs
- retransmit from point of loss

### selective repeat
- receiver ACKs each segment individually (not cumulative)
- sender only resends those not ACKd
- requires extra buffering and state on the receiver

# 3.5 connection-oriented transport (TCP)
- connection oriented: before one application can send data to another, a handshake must occur
- TCP only runs in the end systems and not in the intermediate network elements (which dont maintain TCP connection sate)
- full-duplex service: if TCP connection between A on one host and B on another, application-layer data flows from A to B at the same time it flows from B to A
- point-to-point: between single sender and receiver
- offers congestion control mechanism: service for general welfare of the internet which limits tcp connections to their fair share of network bandwith and throttles sending process when network is congested
- reliable data transfer: no missing or out of order or duplicate bytes

## making tcp connection
- client process: initiates connection
- server process: other
1. client application informs client transport layer tht it wants to make a connection accross the server, sends special TCP segment
  a. clientSocket.connect(serverName, serverPort)
2. client TCP estalishes a TCP connection w/ server TCP, server responds w/ second special TCP segment
3. client responds with third special TCP segment

**three-way handshake**
1. the client sends a small TCP segment to the server
2. the server acknowledges and responds with a small TCP segment
3. the client acknowledges back to the server

- first two segments carry no payload aka application-layer data, third carries payload
- first two pars take one RTT for non-persistent, total response time is 2 RTTs + transmission time at server of HTML file



## sending data over tcp from client to server
1. client passes data thru socket, to client TCP
2. client TCP directs data to connection's **send buffer**
3. TCP grabs chunks of data from send buffer and passes to the network layer
  a. max amt of data grabbed and placed in segment limited by **maximum segment size** MSS
  b. set using the MTU , etc.
4. TCP pairs each data chunk w a TCP header, forming **TCP segments**
5. segments passed to network layer, where encpapsulated w/i network-layer IP datagrams
6. IP datagrams sent into network
7. Server TCP receives segment and the data is placed in server TCP receive buffer
8. application reads stream of data from receive buffer
  a. each side of connection has its own send and reeive buffer
  
  ## connection components:
  1. 1st set of buffers
  2. variables
  3. socket connection in one host
  4. 2nd set of buffers
  5. 2nd set of variables
  6. 2nd socket onnection in other host
  
  ## TCP segment components
  in addition to source and destination port numbers and checksum field...
  1. sequence number (for a segment): byte stream number of first byte in the segment
  2. acknowledgement number: A's sequence number of the next byte HOST A is expecting from Host B
    a. TCP provides cumulative acknowledgemnts bc only acknowledges bytes up to first missing byte in stream
  3. receive window: number of bytes receiver can accept
  4. header length field
  5. options field: for negotiating MSS
  6. flag field of 6 bits:
    a. ACK bit: valid?
    b. RST, SYN, FIN: connection setup and teardown
    c. PSH: receiver should pas data to upper layer immediately
    d. URG: sending-side upper layer marked some data as urgent
  7. urgent data pointer field: location of last byte of this urgent data
  
combine ACKs and Data so that we don't have to use different sequence numbers for sender and receiver

## Round Trip Time
EstimatedRTT = (1- &alpha;)* EstimatedRTT + &alpha; * SampleRTT

&alpha; = .125 typically

- more weight on recent samples bc changin network conditions
- **exponential weighted moving average**

DevRTT = (1 – &beta;) • DevRTT + &beta; • | SampleRTT – EstimatedRTT | where &beta; = .25

TimeoutInterval = EstimatedRTT + 4 • DevRTT

## TCP's reliable data transfer
on top of ip's unreliable due to:
- pipelined segments
- cumulative acks
- single retransmission timer

retransmissions triggered by:
- timeout events
- duplicate acks

## TCP sender events
data rcvd from app:
- create segment with seq #
- start timer

timeout:
- retransmit segment tht caused timeout
- restart timer

ack rcvd:
- if ack acknowledges previously unacked segments
  - update what's ackd
  - start timer if still unacked segments
  
  
## doubling timeout
- every time packet times out, double timer interval (for given packet)
- limited form of congestion control
  - assumes delay caused by too many packets in network
  - resends could make problem worse
## TCP fast retransmit
- if sender receives 3 acks for same data (triple duplicate acks), resend unacked segment w smallest seq #
- dont wait for timeout


# 3.5 flow control

> mail protocols
> Study conversions by gigabytes to bits, microseconds, milliseconds
> need calculations for utlization of link, throughput
> study segment structures, possible inputs (TCP)
> TIME_WAIT state in TCP
> How to calculate throughput(kilobits per second?)
> 3.5, 3.6, 3.7, Congestion control and TCP open issues
> What is ACK Channel?






