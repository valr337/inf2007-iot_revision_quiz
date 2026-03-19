# Lecture 7 — Bluetooth

## 1. Where Bluetooth fits in IoT
- In the lecture’s IoT ecosystem, **Bluetooth Smart / BLE** is positioned as a **datalink-layer technology**.
- The protocol survey slide also places **Bluetooth Low Energy** among the IoT **datalink** options.
- This matters because Bluetooth is mainly about **short-range wireless communication** between nearby devices, not long-haul Internet routing.

## 2. Wireless preliminaries before Bluetooth
The lecture starts with a short wireless refresher.

### Bits, symbols, and carriers
- Messages are represented as **bits**: `1` and `0`.
- Before transmission, bits are grouped and encoded into **symbols**.
- These symbols are then multiplied by a **carrier signal** such as a cosine wave.

### Example modulation schemes
- **BPSK**
  - bit `1` → `+1`
  - bit `0` → `-1`
- **QPSK**
  - **2 bits per symbol**

### Why multiple access is needed
When multiple users need to transmit over the same wireless medium, we need multiple-access schemes such as:
- **TDMA**
- **FDMA**
- **CDMA**
- **OFDMA**

## 3. Bluetooth background and history
- Bluetooth began with **Ericsson’s Bluetooth Project** in **1994**.
- Its original goal was **short-distance radio communication between cell phones**.
- The name comes from **Danish king Herald Blatand**.
- **Bluetooth SIG** was formed in **May 1998** by:
  - Intel
  - IBM
  - Nokia
  - Toshiba
  - Ericsson
- **Bluetooth 1.0A** appeared in **late 1999**.
- **IEEE 802.15.1** approved in **early 2002** was based on Bluetooth.

### Early Bluetooth launch features
The lecture highlights these launch-era features:
- **Low power**: about **10 mA standby**, **50 mA transmitting**
- **Cheap**: around **$5 per device**
- **Small**: about **9 mm²** single chips

## 4. Bluetooth version progression
The version table in the lecture summarizes how Bluetooth evolved:

- **1.0–2.x**
  - basic wireless cable
  - later higher data rate and better pairing
- **3.0**
  - high speed via **Wi-Fi**
  - faster file transfers
- **4.x**
  - **Low Energy (BLE)**
  - years of battery life for small sensors
- **5.0–5.3**
  - better range, speed, direction finding, LE Audio
  - useful for industrial IoT and tracking
- **5.4**
  - massive low-power broadcast fleets
- **6.0**
  - precise distance, smarter scanning, lower latency

## 5. Classic Bluetooth radio details
The lecture gives these core radio properties for classic Bluetooth:

- Frequency range: **2402–2480 MHz**
- Total band: **79 MHz**
- Nominal data rate: **1 Mbps**
- User data rate: about **720 kbps**
- Enhanced data rate: up to **3 Mbps**
- Spread spectrum method: **FHSS**
- Hopping rate: **1600 hops/s**
- Hop time: **625 µs**

### Security
- **Challenge/Response authentication**
- **128-bit encryption**

### Transmit power classes
- **Class 1**: **20 dBm**, around **100 m**
- **Class 2**: **4 dBm**
- **Class 3**: **0 dBm**, around **10 m**

## 6. FHSS: Frequency Hopping Spread Spectrum
Bluetooth classic uses **Frequency Hopping Spread Spectrum (FHSS)**.

### FHSS idea
- The carrier **hops from one frequency to another**.
- The sequence looks random, but both transmitter and receiver follow the **same predefined seed**.

### Symbols used in the lecture
- `Th` = hopping time
- `Wd` = signal bandwidth
- `Ws` = spread spectrum bandwidth
- `C` = number of carrier frequencies / channels

### Key formula
- **Ws = C × Wd**

### Important implications
- A larger number of carrier frequencies spreads the signal over a wider band.
- With a **k-bit PRN generator**, the maximum number of channels is **2^k**.

## 7. Classic Bluetooth channel access
The lecture’s channel-access slides describe classic Bluetooth as follows:

- Total bandwidth divided into **1 MHz physical channels**
- Hopping sequence shared by all devices in the **piconet**
- Maximum radio frequency hopping:
  - **1600 times/s**
  - **625 µs slot / hop time**
- Packets may be **1-slot**, **3-slot**, or **5-slot**
- The hop is **skipped during a packet**

### Duplexing
Bluetooth uses **TDD**:
- **Master** starts in **even-numbered slots**
- **Slaves** start in **odd-numbered slots**

## 8. Bluetooth topology
### Piconet
A **piconet** is the basic Bluetooth network unit.

- One **master**
- Multiple **slaves**
- Up to **7 active slaves**
- Up to **255 parked slaves**

Other lecture points:
- Slaves transmit **only when requested by the master**
- Active slaves are **polled by the master**
- Parked devices use an **8-bit parked address**
- A device can be **master in one piconet** and **slave in another**

### Scatternet
A **scatternet** is formed when **multiple piconets are joined together**.

Benefits:
- More devices can share the same area
- Better bandwidth usage

Main disadvantage:
- **Collisions** can happen if different piconets use the same hopping sequence

## 9. Bluetooth operational states
The lecture introduces these classic Bluetooth states:

- **Standby**
  - initial state
- **Inquiry**
  - master sends inquiry packets
  - slaves respond with address and clock after a random delay
- **Page**
  - master invites devices into the piconet
  - page message sent in **3 consecutive slots / frequencies**
- **Connected**
  - a **3-bit logical address** is assigned
- **Transmit**
  - actual data transmission

## 10. Energy management in classic Bluetooth
Three inactive / low-power states are highlighted:

### Hold
- No **ACL**
- **SCO** continues
- node may do other actions such as scan, page, inquire

### Sniff
- low-power mode
- slave wakes and listens at fixed **sniff intervals**

### Park
- very low-power mode
- device gives up its **3-bit active address**
- receives an **8-bit parked address**
- wakes periodically and listens to beacons

## 11. Classic Bluetooth MAC frame
The lecture gives the Bluetooth MAC frame structure and notes:

- Frames can be up to **5 slots**
- **5 slots = 3125 µs**

### Access codes
Used for packet arrival detection, timing synchronization, and offset compensation.

Types:
- **Channel access code**
  - identifies the piconet
  - used in connected state
- **Device access code**
  - used during paging
- **Inquiry access code**
  - used during inquiry

### Header contents
The header may include:
- member address
- type code
- flow control
- ack/nack
- sequence number
- header error check

### Bluetooth MAC address structure
Bluetooth devices use a **48-bit IEEE MAC address**:
- **LAP** = 24 bits
- **UAP** = 8 bits
- **NAP** = 16 bits

The lecture states:
- **UAP + NAP = OUI**

## 12. Why BLE matters for IoT
The second half of the lecture moves from classic Bluetooth to **Bluetooth Low Energy (BLE)**.

BLE is designed for:
- **low energy**
- **small messages**
- **short broadcasts**
- **years of battery life from coin cells**

Typical BLE use cases from the lecture:
- body temperature
- heart rate
- wearables
- sensors
- automotive
- industrial applications

BLE is **not** intended mainly for:
- voice
- video
- large file transfers

## 13. BLE core properties
The lecture describes BLE as:

- using around **1% to 50%** of the energy of classic Bluetooth
- having **lower connection time**
- being **lower cost**
- based on Nokia’s **WiBree**
- sharing the same **2.4 GHz** radio band as Bluetooth
- supported by **dual-mode chips**
- common in modern smartphones

### BLE topology
The lecture explicitly says BLE is simple:
- **Star topology**
- **Mesh from BLE 5.1**

## 14. BLE PHY details
The BLE PHY slide lists:

- two device types:
  - **Peripheral**
  - **Central**
- peripherals are simpler than centrals
- BLE transmits at lower power than classic Bluetooth
- BLE has intelligent power management
- BLE 4.2: **1 Mbps**
- BLE 5 and 6: **2 Mbps**
- **40 channels** total
- **2 MHz spacing**
- **3 advertising channels**
- **37 data channels**

The advertising channels are specially placed to **avoid Wi-Fi interference**.

## 15. BLE operational procedures
The lecture lists several BLE procedures, including:

- device filtering
- advertising
- scanning
- discovering
- connecting
- connected mode
- periodic advertising
- synchronization with periodic advertising
- decision-based scanning

### Why BLE is energy efficient
The lecture explicitly says BLE’s low energy usage comes from:
- adjustable advertising behaviour
- configurable connection intervals
- sleep scheduling

## 16. BLE protocol stack
The BLE stack in the lecture includes:

### GAP
- device discovery
- network topology
- security procedures

### HCI
- communication between host and controller

### Link Layer
- packet structure and control

### PHY
- transmission / reception
- modulation / demodulation
- 40 channels of 2 MHz

### SMP
- pairing
- authentication
- encryption

### L2CAP
- higher-layer multiplexing
- packet segmentation / reassembly
- QoS information

### ATT
- defines how data is represented in a BLE server database
- defines how it can be read or written

### GATT
- service framework built on ATT
- used to discover services
- read / write characteristics

## 17. ATT: Attribute Protocol
ATT defines **server** and **client** roles.

Each attribute has:
- an **attribute type** defined by a **UUID**
- an **attribute handle**
- a set of **permissions**

The lecture also lists ATT PDU types:

- **Commands**
  - client → server
  - no response
- **Requests**
  - client → server
  - response required
- **Responses**
  - server → client
- **Notifications**
  - unsolicited server → client
  - no confirmation
- **Indications**
  - unsolicited server → client
  - confirmation required
- **Confirmations**
  - client → server
  - confirms an indication

## 18. GATT: Generic Attribute Profile
GATT defines a **service framework** based on ATT.

It organizes BLE data into:
- **services**
- **characteristics**
- **descriptors**

The lecture states that **services, characteristics, and descriptors are all attributes** identified by **UUIDs**.

### GATT operations
The lecture says there are **11 features / operations** in GATT, including:
- server configuration
- primary service discovery
- relationship discovery
- characteristic discovery
- characteristic descriptor discovery
- reading a characteristic value
- writing a characteristic value
- notification of a characteristic value
- indication of a characteristic value
- reading a characteristic descriptor
- writing a characteristic descriptor

## 19. BLE data exchange
In the lecture’s BLE data exchange flow:

- the **peripheral** advertises itself
- the **central** scans for peripherals
- the central initiates a **connection request**

For request-response data exchange:
- a **request PDU** is sent by the client to the server
- a **response PDU** is sent by the server back to the client when a reply is needed

## 20. Bluetooth beacons
Bluetooth beacons are explained as **advertising based on proximity**.

### Main purpose
- device discovery
- indoor navigation

### Beacon frame details
- header
- maximum **27-byte payload**
- multiple **TLV-encoded** items

The lecture also notes:
- signal strength may be used to estimate **distance**
- phones can send and receive beacons
- beacons can support:
  - customized advertising
  - indoor location
  - geofencing

## 21. Bluetooth Smart applications
Examples listed in the lecture:
- **Proximity**
- **Locator** for keys, watches, animals
- **Health devices**
  - heart-rate monitor
  - physical activity monitor
  - thermometer
- **Sensors**
  - temperature
  - battery status
  - tire pressure
- **Remote control**
  - locks
  - lights

## 22. Key takeaways
- Classic Bluetooth uses **FHSS over 79 channels of 1 MHz**
- BLE is optimized for **short-burst sensor communication**
- BLE uses **40 channels of 2 MHz**
- **3 channels** are reserved for advertising
- **GATT + ATT** make BLE extensible for many IoT applications
- Bluetooth remains important in IoT because it balances:
  - low power
  - short-range wireless connectivity
  - easy integration with phones and embedded devices
