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

# Application Layer




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

## making tcp connection
- client process: initiates connection
- server process: other
1. client application informs client transport layer tht it wants to make a connection accross the server, sends special TCP segment
  a. clientSocket.connect(serverName, serverPort)
2. client TCP estalishes a TCP connection w/ server TCP, server responds w/ second special TCP segment
3. client responds with third special TCP segment

first two segments carry no payload aka application-layer data, third carries payload
**three-way handshake**

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

> Study conversions by gigabytes to bits, microseconds, milliseconds
> need calculations for utlization of link, throughput
> study segment structures, possible inputs (TCP)
> TIME_WAIT state in TCP
> How to calculate throughput(kilobits per second?)
> 3.5, 3.6, 3.7, Congestion control and TCP open issues
> What is ACK Channel?






