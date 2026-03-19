# CSC2106 Lecture 1 — Introduction to IoT

## 1. What is IoT?
Internet of Things (IoT) refers to a network of physical objects (“things”) embedded with sensors, software, and connectivity so that they can exchange data with other devices and systems over the internet.

Common points across the lecture definitions:
- physical objects are connected
- sensing / actuation is involved
- devices exchange data
- value comes from processing that data into useful services

### IEEE view
IoT is described as a **network of networks** where a massive number of objects, sensors, and devices are connected through communication and information infrastructure to provide value-added services through intelligent data processing and applications.

Examples mentioned:
- smart cities
- smart health
- smart grid
- smart home
- smart transportation
- smart shopping

## 2. What is a “thing”?
A “thing” in IoT is usually made up of:

**Thing = Controller + Sensors / Actuators + Network connectivity**

### Breakdown
- **Controller**: the “brain” that runs the logic
- **Sensors**: collect data from the environment
- **Actuators**: perform actions
- **Network / Internet**: allows communication with other systems

## 3. Purpose of IoT
The lecture frames the purpose of IoT as:

- **Collect**
- **Connect**
- **Comprehend**

### Meaning
- **Collect** data from the physical world
- **Connect** devices, gateways, cloud, and users
- **Comprehend** the collected data to support decisions, automation, and insights

## 4. Data vs Knowledge
Raw IoT data alone is not the end goal.

The lecture emphasizes:
- transforming raw data into **actionable insights**
- using **real-time analytics**
- using **visualisation**
- combining data from **multiple sources**
- supporting **decision-making and automation**

### Key idea
IoT is useful when data becomes knowledge that can trigger actions or better decisions.

## 5. IoT is pervasive
IoT spans many domains and is not limited to consumer gadgets.

Examples of broad IoT application areas:
- home automation
- industrial systems
- transport
- healthcare
- infrastructure
- retail
- smart environments

## 6. Characteristics of IoT Communications
The lecture highlights common communication traits in IoT:

- wireless
- low cost
- low power / long battery life
- low processing capacity
- low storage capacity
- large number of connected devices
- different bitrate / bandwidth needs
- different range requirements
- different QoS requirements
- small-sized devices
- relatively simple network architecture and protocols

### Implication
IoT communication choices are usually constrained by:
- energy
- cost
- device capability
- deployment scale
- range and reliability needs

## 7. IoT Connectivity Technologies
The lecture introduces multiple IoT connectivity options and protocol layers.

Examples shown:
- IEEE 802.15.4
- BLE
- Wi-Fi 802.11 b/g/n/ac/ax
- HaLow (802.11ah)
- RFID / NFC
- 3GPP technologies
- LoRa
- LoRaWAN
- Zigbee
- Thread
- 6LoWPAN
- IPv4 / IPv6 / RPL
- UDP / TCP
- HTTP / MQTT / CoAP

### Stack perspective
IoT protocols span multiple layers:
- **Physical layer**
- **Data link layer**
- **Network layer**
- **Transport layer**
- **Application / data layer**

## 8. Layered and data-centric views
The lecture presents:
- **Layered functional capabilities for IoT systems**
- **Data-centric view of IoT operations**

### Why this matters
These views help explain:
- where sensing happens
- where communication happens
- where storage / analytics / applications sit
- how data flows from device to service to user / decision logic

## 9. IoT design requirements
IoT system design must consider real constraints, not just functionality.

### Network connectivity requirements discussed
- **Resource-constrained endpoints**
  - very limited memory and compute
- **Low power**
  - devices may sleep most of the time
  - battery life is critical
- **Embedded deployment**
  - devices may be hard to access or replace
  - security matters more in hostile / remote environments
- **Longevity**
  - many devices are deployed for long periods
  - lifetime can exceed normal ICT refresh cycles
- **High sensitivity on reception**
  - gateways / endpoints may need strong receive sensitivity

## 10. Key IoT network metrics
The lecture highlights four important metrics.

### 10.1 Latency
Time between sending data and receiving acknowledgement / echo.

Why it matters:
- important for real-time control
- affects interactive applications

### 10.2 Throughput
Amount of data transferred per unit time.

Why it matters:
- affects how fast data, logs, or firmware can be transferred

### 10.3 Packet loss
Fraction of transmitted packets that never arrive or are not acknowledged.

Why it matters:
- indicates reliability
- high loss can break workflows

### 10.4 Jitter
Variation in packet delay over time.

Why it matters:
- important for timing-sensitive systems
- can affect motion, voice, or synchronized actions

## 11. Measuring IoT network metrics
The lecture explains that even without dedicated tools, these metrics can be estimated with custom send-and-reply logic.

### General approach
1. Send known packets
2. Timestamp send / receive times
3. Count sent vs received packets
4. Stress the network with larger traffic volume
5. Compute:
   - latency
   - throughput
   - packet loss
   - jitter

### Protocol-specific examples
- **Wi-Fi**: send UDP / TCP over IP
- **Bluetooth / BLE**: use a custom GATT characteristic and notifications
- **LoRaWAN / Zigbee**: use uplinks and acknowledgements / confirmations

### Practical constraints
- store logs for offline analysis
- respect duty cycle and battery limits
- account for payload size limits
- optionally use clock sync for advanced timing measurement

## 12. AIoT
AIoT = **AI + IoT**

The lecture shows examples such as:
- crowd counting
- object localization / classification
- license plate recognition

### Main idea
AI adds intelligence to IoT data streams so devices / systems can:
- interpret sensor data
- detect patterns
- automate higher-level decisions

## 13. Edge computing
The lecture motivates edge computing because IoT generates massive amounts of data.

### Why edge computing is needed
Sending everything to the cloud is not always efficient.

### Advantages of edge computing
- reduces network bandwidth requirement
- faster response time
- reduces storage requirements in the cloud / backend
- supports more local decision-making

### Edge platform diversity
The lecture notes heterogeneous edge platforms, such as:
- heavy edge
- medium edge
- mini edge
- micro edge

## 14. Designing for the edge — key considerations
The lecture lists the following design considerations:

1. latency and response time  
2. local data processing and analysis capability  
3. scalability  
4. security and privacy  
5. network connectivity and bandwidth  
6. hardware constraints  
7. energy efficiency  
8. data management and storage  
9. robustness and reliability  
10. user interface and experience  
11. regulatory compliance  
12. cost-effectiveness  
13. customization and flexibility  

## 15. Final factors to consider in IoT communication choices
The lecture summary emphasizes these decision factors:

### Range
- short range: Bluetooth, Zigbee
- long range: LoRaWAN, Sigfox

### Power consumption
- especially important for battery-powered devices
- low-power examples: BLE, Zigbee

### Network metrics
- latency
- throughput
- packet loss
- jitter

### Compatibility and scalability
- must work with existing systems
- should support large deployments

### Security
- strong encryption and authentication
- examples: TLS, DTLS for sensitive data

### Cost
- includes hardware and maintenance
- specialized protocols may cost more

## 16. Fast revision sheet
### Core definitions
- IoT = connected physical things exchanging data
- Thing = controller + sensors / actuators + network
- Purpose = collect, connect, comprehend

### Core constraints
- low power
- low cost
- limited compute / memory
- large scale
- varied QoS / range / bandwidth needs

### Core metrics
- latency
- throughput
- packet loss
- jitter

### Big-picture trends
- AIoT adds intelligence to IoT systems
- edge computing reduces bandwidth and latency burden
- protocol / connectivity choice depends on trade-offs, not one “best” answer
