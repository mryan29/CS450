# Aug 29th

# Layering (cntd.)
Kinds of networking functionality
- addressing
- routing
- flow control
- security

How should they be organized?

How should they interact?

## Message Encapsulation
| - Header - | --------- Data ---------- |

**Message**: contains header and data
**Data**: What sender wants receiver to know
**Header**: info to support protocol
- source and dest addresses
- state of protocol operation
- error control (to check integrity of received data)
- also known as "metadata"

Layering is basically the separation of functions/responsibilities. 
Sender/Receivers don't need to know about delivery procedures, and delivery system doesn't need to know about data.

## 4 Layer Internet Model
- App: the app i.e. email, ftp
- Transport: end-to-end reliable connections
- Network: 
- Link: 
- (Physical)

## Application POV
- Program(mer)s interact w/ network
- Uses the **socket** API
- Protocols can be defined for specific use cases
  - HTTP (web), SMTP (email), DNS (name resolution)
  
 ## Transport POV
 - two processes on diff computers exchanging bytes
 - how to **multiplex** diff app-level messages
 - what **abstraction** to expose to the app developer: what **APIs** to provide?
 - how to **manage** realities/problems of the underlying network i.e. congestion, loss
 
  ## Network POV
 - Routing
  - internet is network of networks; **routing** is choosing which hops to take to get from a to b
  - how to get from laptop to phone, chicago to africa
- Naming/Addressing
  - how to **identify** each computer
- Resource Mgmt
  - what happens when more packets coming in than capacity to send to destination?
  
## Link POV
- Forwarding
  - how to store and **send** bytes to next hop (not the endpoint)
- Media Access Control
  - if multiple senders want to send on shared channel, how do they **coordinate**
  
Q: Which layer should only have one option for protocol?

A: Network because it is the only layer offered globally, and therefore everyone has to speak the same language.

## Whats involved transfering file from A to B
### App pov
- authenticate, permission, etc.

### Network
- identify/address server
- which routers to pick
- ensure reliable in-order delivery
- how to traverse packet to each hop

### Encapsulation
- Higher level n with lower level n-1
- multiplexing and ...

## Benefits of Layering
- New application: interface to all existing media
  - requires O(m) work, m = number of media
...

  
When you make an application, it needs to work with so many different techs. with one layer, complexity is multipliCATIVE, WITH MULTIPLE, ITS LINEAR

solution: intermediate layer tht provides a single abstraction for various network technologies
- O(1) work to add app/media
- variation on "add another level of indirection"

| --- | --- | -- Applications -- | --- | --- |

&nbsp;&nbsp; | Intermediate Layer |

| --- | --- | -- Transmission Media -- | --- | --- |

## Protocol
- goal: get message from sender to receiver
- agreed message format and transfer procedure
- multiparty, so no central thread of control
  - sndr recvr are separate processes
- expectations of operation
  - first you do x, then i do y, then you do z, ...
  - if you do q, ill do p

## Advanced topic
- abstractions: tunnelling
- take entire packet rcvd from lower layer, treat as app, and send it thru the layers

## Which of these belong in the network? ...
- Reliability
- Congestion control
- Quality of Service (QoS)
- Security/Firewalls &rarr; Yes
