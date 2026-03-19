# Lecture 11 — ZigBee and Z-Wave

## 1. Matter brief introduction
- Matter is introduced as an **open standard for smart home technology**.
- It allows a device to work with any **Matter-certified ecosystem** using a **single protocol**.
- The lecture associates Matter with the **Connectivity Standards Alliance (CSA)**.

## 2. ZigBee overview
- ZigBee targets **industrial monitoring and control** applications that send **small amounts of data**.
- Example use cases mentioned:
  - wireless light switches
  - meter reading
  - patient monitoring
- Typical duty cycle is **very low**: devices are off most of the time, **less than 1% duty cycle**.
- First standard publication: **2004**.
- Key characteristics:
  - **ultra-low power**
  - **low-data rate**
  - **multi-year battery life**
  - explicit **power management** for low power consumption
- Stack complexity comparison from the lecture:
  - ZigBee protocol stack: **32 kB**
  - Bluetooth stack: **250 kB**
- Range from the lecture: **1 to 100 m**.
- Network size mentioned: **up to about 65,000 nodes**.

## 3. ZigBee bands and data rates
ZigBee is presented as a **tri-band** technology:
- **2.4 GHz ISM**: **16 channels**, **250 kbps**
- **915 MHz ISM (Americas)**: **10 channels**, **40 kb/s**
- **868 MHz (Europe)**: **1 channel**, **20 kb/s**
- The lecture also mentions **920 MHz in Japan**.

## 4. ZigBee architecture and networking style
- ZigBee uses **IEEE 802.15.4 PHY and MAC**.
- Higher-layer support and interoperability are attributed to the **ZigBee Alliance**.
- ZigBee is described as a **multi-hop ad-hoc mesh network**.
- Concepts highlighted:
  - **multi-hop routing**: messages can go to non-adjacent nodes
  - **ad-hoc topology**: no fixed topology; nodes discover each other
  - **mesh topology**: loops are possible
- The name “ZigBee” is linked to the **zigzag dance of honeybees** indicating the **location of food**.

## 5. Interoperability
### ZigBee compliant platform
- Ensures **network interoperability**.
- Does **not automatically imply application-layer interoperability**.

### ZigBee compliant product
- End-to-end interoperability depends on products using the **same application profile**.
- The lecture shows a compliant product as including:
  - **IEEE 802.15.4**
  - **ZigBee stack**
  - **Application profile**

## 6. ZigBee device types
### ZigBee Coordinator (ZC)
- Exactly **one required per network**.
- **Initiates network formation**.

### ZigBee Router (ZR)
- Participates in **multi-hop routing**.

### ZigBee End Device (ZED)
- Does **not allow association or routing**.
- Supports **very low-cost solutions**.

## 7. ZigBee topologies
The lecture shows three supported topologies:
- **Star**
- **Mesh**
- **Cluster Tree**

A **bus** topology is not presented as a ZigBee topology in the lecture.

## 8. ZigBee routing
### AODV (Ad-Hoc On-Demand Distance Vector)
- **Reactive / on-demand** routing.
- Route is constructed **when needed**.
- Source broadcasts **RREQ** containing:
  - source
  - destination
  - broadcast ID
- Intermediate nodes may create a **reverse route entry**.
- If a route is known, a node returns a **route reply**.
- Source picks the **lowest-cost path**.
- If forwarding fails, a node sends **Route Error** back to the source.

### Tree hierarchical routing
- Leaf nodes send packets to their **parent** first.
- Parent checks whether destination is in its **subrange**.
  - if yes, forward to the proper child
  - if no, send upward to its parent

### Many-to-one routing
- Used for **sensor data collection** toward a **concentrator/gateway**.
- Gateway has **large memory** and can hold full routes.
- Ordinary nodes remember only the **next hop toward the gateway**.

## 9. Self-healing mesh
- The lecture uses a sequence of diagrams to show ZigBee mesh **self-healing**.
- Meaning:
  - when some nodes fail,
  - the network can **reroute via alternate paths**,
  - preserving connectivity between source and destination.

## 10. ZigBee stack architecture
The lecture stack contains:
- **Application**
- **Application Objects (AOs)**
- **ZDO**
- **App Support (APS)**
- **NWK**
- **SSP**
- **MAC (802.15.4)**
- **PHY (802.15.4)**

### Responsibilities highlighted
- **Application**:
  - initiate and join network
  - manage network
  - determine device relationships
  - send and receive messages
- **NWK**:
  - network organization
  - route discovery
  - message relaying
- **APS**:
  - device binding
  - messaging
- **ZDO**:
  - device management
  - device discovery
  - service discovery
- **SSP**:
  - security functions

### Application Objects
- AO = **Application Object** or **endpoint**.
- Up to **250 AOs per node**.

## 11. ZigBee application layer
- Consists of **application objects** and **ZDOs**.
- Endpoint addressing:
  - **EP0** = ZDO
  - **EP1 to EP240** = application objects
  - **EP241 to EP254** = reserved
  - **EP255** = broadcast
- Each endpoint has **one application profile**.
- A profile contains data items called **attributes**.
- Each attribute is assigned a **16-bit attribute ID**.

## 12. ZigBee public profiles
The lecture lists these profiles:
- **Home Automation (HA)**
- **Smart Energy (SE)**
- **Commercial Building Automation (CBA)**
- **ZigBee Health Care (ZHC)**
- **Telecom Applications (TA)**
- **ZigBee RF4CE Remote Control**

## 13. ZigBee home automation
The Home Automation slide shows a **ZigBee Home Area Network (HAN)** for home control.
Examples shown include:
- TV/display
- set-top box
- lighting
- switches
- security
- heating/cooling
- closures
- remote access

## 14. Smart Energy and home automation
- The lecture emphasizes strong demand for **Smart Energy**.
- Compatibility with mainstream **home automation systems** is said to **enable customer choice**.

## 15. Z-Wave overview
- Z-Wave is presented as a **low-power MAC protocol**.
- Main target domains:
  - **smart home**
  - **small commercial** environments
- Point-to-point range from the lecture: about **30 m**.
- Suitable for **small IoT messages** such as:
  - light control
  - energy control
  - wearable healthcare control
- Reliability and access:
  - **CSMA/CA**
  - **ACK** for reliable transmission
- Network style:
  - **master/slave architecture**
  - master controls slaves, sends commands, and handles scheduling

## 16. ZigBee vs Z-Wave — common points from the lecture
The lecture states that both:
- are **mesh networks**
- allow nodes to act as **data sources and repeaters**
- are used in **local area sensor networks**
- are used in areas such as:
  - security systems
  - HVAC control
  - home automation
  - lighting control

The lecture slide also states that both use **IEEE 802.15.4 LR-PAN** for unified physical-layer packet structuring.

## 17. ZigBee vs Z-Wave — differences from the lecture
### Ecosystem
- **Z-Wave**: tightly controlled product ecosystem
- **ZigBee**: used across several application areas

### Frequency band
- **ZigBee**: global **2.4 GHz ISM**
- **Z-Wave**:
  - **915 MHz** in the U.S.
  - **868 MHz** in Europe

### Interference
- 2.4 GHz can face stronger interference from **Wi-Fi** and **Bluetooth**.
- The lecture says Z-Wave’s **sub-GHz** bands avoid the same level of interference.

### Radio ecosystem
- Many vendors make **ZigBee radios**.
- Z-Wave is described as using a **proprietary radio system from Sigma Designs**.

### Modulation
- **Z-Wave**: **FSK**
- **ZigBee**: **DSSS**

## 18. Lecture summary
- ZigBee is positioned for **sensors, industrial automation, and remote control** using **IEEE 802.15.4 PHY/MAC**.
- ZigBee supports **up to 250 kbps** and a **large number of devices**.
- Newer ZigBee protocols support:
  - **many-to-one routing**
  - **fragmentation**
  - **mesh topologies**
- Control and management are associated with **ZDOs**.
- Z-Wave uses **sub-GHz** bands with lower susceptibility to interference and occlusions, but it is **proprietary**.
