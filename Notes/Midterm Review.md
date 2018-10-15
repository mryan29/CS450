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








