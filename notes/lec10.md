# Lecture 10 — IEEE 802.15.4, 6LoWPAN, and Thread

## 1. Big picture
- **Thread** is presented as an open standard for **reliable, cost-effective, low-power, secure, wireless IPv6 communication**.
- It is aimed at **connected home and commercial applications** where **IP-based networking** is desired.
- Key characteristics from the lecture:
  - simple network installation, startup, and operation
  - secure joining and encrypted communication
  - support for both small and large networks
  - range sufficient for home-scale deployments
  - no single point of failure because the network is **auto-configuring and self-healing**
  - low power, open standards, and application-layer agnosticism

## 2. Thread protocol stack
The lecture stack places Thread across these layers:
- **Application**
- **UDP**
- **IP Routing**
- **6LoWPAN**
- **IEEE 802.15.4 MAC**
- **IEEE 802.15.4 PHY**

A vertical function of **Security / Commissioning** is also shown.

## 3. IEEE 802.15 family context
The lecture briefly situates Thread within the broader IEEE 802.15 family.
Important items mentioned:
- **802.15.1** — Bluetooth
- **802.15.4** — Low-rate WPAN, associated with ZigBee and used by Thread as well
- **802.15.5** — Mesh networking

## 4. IEEE 802.15.4 overview
IEEE 802.15.4 in this lecture is the **low-rate wireless personal area network (LR-WPAN)** foundation.

Important lecture points:
- most common band: **2.4 GHz**
- **16** “5-MHz channels”
- **250 kbps PHY**
- about **50 kbps application data rate**
- uses **DSSS**, **CSMA/CA**, backoff, beacons, and a coordinator concept
- lower rate and short range lead to lower power and low energy
- each node has a **64-bit EUI-64**
- maximum MAC frame size is **127 bytes**
- there is **no segmentation/reassembly at the MAC layer**

## 5. DSSS basics
The lecture explains **Direct Sequence Spread Spectrum (DSSS)** as follows:
- each data bit is replaced by multiple bits using a **spreading sequence**
- the spreading sequence is also called a **chipping sequence**
- the resulting bits are called **chips**
- the length of the spreading code affects bandwidth
- the slide uses **XOR** to illustrate spreading: **C = A ⊕ B**

### DSSS with BPSK
The BPSK form shown is approximately:
- \(s_d(t) = A d(t) \cos(2\pi f_c t)\)

Where:
- **A** = amplitude
- **fc** = carrier frequency
- **d(t)** = +1 for bit 1, −1 for bit 0

For DSSS:
- multiply the BPSK signal by the chipping sequence **c(t)**
- to recover the signal, multiply by **c(t)** again at the receiver

## 6. IEEE 802.15.4 topologies
The lecture introduces:
- **Star** topology
- **Peer-to-peer** topology
- **Cluster tree** extension

Device types:
- **FFD** — Full Function Device
- **RFD** — Reduced Function Device

Important details:
- an FFD that starts a PAN becomes the **coordinator**
- each PAN has a **PAN ID**
- devices join by sending an **association request** to the coordinator
- the coordinator assigns a **16-bit short address**
- devices may use either the short address or the **EUI-64** address
- in a cluster tree, another FFD can become coordinator for a subset of nodes, and the tree has **no loops**

## 7. IEEE 802.15.4 MAC behavior
### Beacon-enabled mode
- coordinator sends **beacons periodically**
- part of the beacon interval is **inactive**, so devices can sleep
- the active interval has **16 slots**
- **GTS** are used for real-time services in the **CFP**
- **CAP** uses **slotted CSMA**

### Beaconless mode
- uses **unslotted CSMA**
- if no coordinator beacons are sent, there are **no slots**
- acknowledgements are sent if requested by the sender
- short transmissions use **SIFS**, otherwise **LIFS**
- acknowledgement, beacon, and MAC command frames are treated as **short** frames
- data frames are **long**

### CSMA/CA summary
- wait until the channel is free
- wait a random backoff period
- if still free, transmit
- if busy, back off again
- in battery life-extension mode, the backoff exponent is limited to **0–2**
- acknowledgements and beacons are sent **without CSMA/CA**

## 8. EUI-64 addressing
The lecture compares:
- Ethernet MAC: **48-bit**
- IEEE 802.15.4 EUI: **64-bit**

The slide also notes:
- the old “local bit” interpretation caused issues
- RFC4291 changed the meaning so **L = 0** indicates local
- the second bit is now called the **U-bit**

## 9. 6LoWPAN
6LoWPAN is introduced as:
- **IPv6 over Low-Power Wireless Personal Area Networks**
- a networking-layer adaptation that makes IPv6 practical over constrained 802.15.4 links

### Why it is needed
The lecture highlights the size mismatch:
- IPv6 MTU is at least **1280 bytes**
- IEEE 802.15.4 frame size is only **127 bytes**

Worst-case header accounting from the slide:
- frame header: **25 bytes**
- link-layer security: **21 bytes**
- IPv6 header: **40 bytes**
- UDP header: **8 bytes**
- payload left: only **33 bytes**

This is why an **adaptation layer** is needed.

### What 6LoWPAN adds
- IPv6 address formation from EUI-64
- address resolution
- optional mesh routing in the data-link layer
- IPv6 and UDP header compression

### Adaptation-layer notes
- balances MAC-level retransmissions with end-to-end retransmissions
- defines compact extension headers, including:
  - **mesh addressing header**
  - **fragment header**
  - **hop-by-hop LowPAN broadcast header**

### Header compression used in Thread
- **IPHC** — Improved Header Compression
- **NHC** — Next Header Compression

## 10. 6LoWPAN IPv6 address formation
The lecture gives these forms:
- **Link-local IPv6 address = FE80:: + U-bit formatted EUI64**
- example shown:
  - local EUI64 = **40::1**
  - U-bit formatted EUI64 = **0::1**
  - resulting IPv6 link-local = **FE80::1**

When IEEE 802.15.4 short addressing is used:
- **IPv6 link-local = FE80::PAN ID:Short Address**
- if PAN ID is unknown, use **0**

## 11. Thread device types and roles
### Two main device classes
- **FTD** — Full Thread Device
- **MTD** — Minimal Thread Device

### Routing FTDs
- **Router**
  - provides routing, joining, and security services
  - cannot sleep
- **Leader**
  - an extra role played by one router in the network

### Non-routing FTDs
- **REED** — Router Eligible End Device
  - can become a router if elected by the leader
- **Full End Device**
  - similar to a REED but cannot become a router

### MTD variants
- **Minimal End Device**
  - communicates only through its parent router
  - radio stays on or idle
- **Sleepy End Device**
  - turns radio off during idle periods and wakes periodically
- **Synchronized Sleepy End Device**
  - wakes periodically at scheduled intervals

## 12. Border Router
A **Border Router** is a role that connects the Thread network to adjacent networks such as:
- **Wi-Fi**
- **Ethernet**

Lecture points:
- it provides off-network routing services
- there may be **several** border routers in one Thread network
- any **Routing FTD** can provide border-router services

## 13. Thread addressing and mesh-local prefix
The device that forms the Thread network generates a **/64 Mesh Local Prefix**.

This prefix is used by each device to generate **two Mesh Local Addresses**.

The lecture distinguishes:
- **IPv6 ULA** — used for communication within the Thread network
- **FE80::/64 link-local** — used for mesh establishment and link maintenance

## 14. Thread topology
- Thread gives **full mesh connectivity between routers**
- if there is only **one router**, the network behaves like a **star**
- if there is **more than one router**, a **mesh** is formed automatically

## 15. Thread routing and connectivity
The lecture states:
- the network can have up to **32 active routers**
- it uses **next-hop routing**
- devices maintain a **link-layer routing table**

### MLE
Routers exchange routing cost information using **MLE (Mesh Link Establishment)**.
MLE is used for:
- establishing/configuring secure radio links
- detecting neighbors
- maintaining routing costs
- handling **asymmetric link costs**

Routers periodically exchange MLE advertisements containing:
- link quality
- link cost to neighbors
- path cost to other routers

This supports **self-healing routing**.
If a route fails, routers can choose another suitable path quickly.

## 16. Forwarding in Thread
Inside the Thread network:
- devices use **IP routing**
- routing tables store a compressed form of each router’s mesh-local address and the correct next hop

For off-network forwarding:
- **Border Routers** are used
- each border router tells the **Leader** which IPv6 prefixes it serves
- the Leader distributes that information as **Thread Network Data** using **MLE**

## 17. Adding a new device to the Thread network
The lecture presents three phases:

### 1. Discovery
- the joining device sends **MLE Discovery Requests** on all channels
- it waits for **MLE Discovery Responses**
- the response includes the **network name** and **steering data**

### 2. Commissioning
- authenticates the new device
- provides the **network credentials**

### 3. Attaching
- a detached device that already has credentials multicasts **MLE Parent Requests** to nearby Routers and REEDs
- the device and its parent router then use MLE to:
  - configure a secure link
  - provision IPv6 addresses

Additional notes:
- a **REED** may upgrade to Router if needed to support the new device
- a device always attaches as an **End Device** first
- it may later become a Router by requesting a **Router ID** from the Leader
- **Discovery and Commissioning are needed only for the first attachment**
- for later re-attachments, the device already has credentials, so **Attaching** is the key remaining phase

## 18. Key takeaways
- **IEEE 802.15.4** provides the low-rate PHY/MAC foundation.
- **6LoWPAN** adapts IPv6 to fit constrained 802.15.4 links.
- **Thread** builds a low-power IPv6 mesh network on top of these components.
- Thread’s main strengths in this lecture are:
  - low power
  - secure joining
  - IP-based design
  - self-healing routing
  - flexible device roles
  - simple interoperability with adjacent IP networks through border routers
