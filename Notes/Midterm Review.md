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
> What is ACK Channel?




