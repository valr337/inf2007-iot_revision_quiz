# Lecture 8 — Wi‑Fi and Wi‑Fi HaLow (IEEE 802.11 / 802.11ah)

## 1. Where Wi‑Fi fits in IoT
- In the lecture’s IoT protocol landscape, **Wi‑Fi** and **IEEE 802.11ah** are treated as **datalink-layer technologies**.
- Wi‑Fi is still relevant in IoT because of:
  - **wide deployment**
  - **compatibility with the Internet**
  - mature ecosystem and infrastructure

## 2. IEEE 802.11 vs Wi‑Fi
- **IEEE 802.11** is a **standard**.
- **Wi‑Fi** is a **trademark**.
- “Fidelity” refers to **compatibility between wireless equipment from different manufacturers**.
- The **Wi‑Fi Alliance** performs compatibility testing.
- Two devices can both be **802.11-compliant** and still be incompatible if they implement different optional features.
- Devices carrying the **Wi‑Fi logo** are intended to **interoperate**.

## 3. Quick Wi‑Fi physical-layer timeline
### Original IEEE 802.11 (1997)
- Included **MAC** plus **3 PHY specifications**
- 2 PHYs in **2.4 GHz**
- 1 PHY in **infrared**
- Supported **1 Mbps** and **2 Mbps**
- No longer used in practice

### 1999 amendments
- **802.11a**
  - **5 GHz**
  - **54 Mbps / 20 MHz**
  - **OFDM**
- **802.11b**
  - **2.4 GHz**
  - **11 Mbps / 22 MHz**

### 2003 amendment
- **802.11g**
  - **2.4 GHz**
  - **54 Mbps / 20 MHz**
  - **OFDM**

### Later Wi‑Fi generations
- **Wi‑Fi 4 (802.11n)**
  - 2.4 GHz or 5 GHz
  - first Wi‑Fi with **MIMO**
  - up to **72 Mbps / 20 MHz** and **600 Mbps / 40 MHz**
- **Wi‑Fi 5 (802.11ac)**
  - 5 GHz
  - **MIMO**, **MU-MIMO**, **OFDM**
  - can use **256 QAM**
  - up to about **1.1 Gbps / 80 MHz**
- **Wi‑Fi 6 / 6E (802.11ax)**
  - introduces **OFDMA**
  - up to **160 MHz**
  - up to **10 Gbps**
  - can use **1024 QAM**
- **Wi‑Fi 7 (802.11be)**
  - up to **46 Gbps**
  - can use **4096 QAM**
  - wider bandwidth and improved OFDMA resource allocation

## 4. Wi‑Fi channels
### 2.4 GHz band
- About **100 MHz** between **2400 MHz and 2500 MHz**
- **14 channels** spaced **5 MHz** apart
- Only **3 non-overlapping** channels

### 5 GHz band
- The slide shows multiple non-overlapping channels.

## 5. Core IEEE 802.11 components
### Station
- A **station** is any device that connects to the wireless medium.

### BSS — Basic Service Set
Two forms are highlighted:
- **Infrastructure BSS**
  - stations associate with one **Access Point (AP)**
- **Independent BSS (IBSS)**
  - stations communicate directly with one another
  - ad-hoc
  - usually short-lived and for a particular purpose

### ESS — Extended Service Set
- A set of infrastructure BSSs
- APs communicate to:
  - forward traffic between BSSs
  - support movement of mobile stations

### DS — Distribution System
- Mechanism for AP-to-AP communication
- Used to:
  - exchange frames for stations
  - forward frames as users move
  - exchange frames with the wired network

## 6. How a station joins a BSS
### Passive scanning
Sequence:
1. APs send **beacon frames**
2. station selects an AP
3. station sends **association request**
4. AP sends **association response**
5. AP assigns an **Association ID (AID)** in the range **1–2007**

### Active scanning
Sequence:
1. station broadcasts **probe request**
2. APs reply with **probe response**
3. station sends **association request**
4. selected AP sends **association response**

### Key idea
- **Active scanning** can be faster because the station does not need to wait for beacons.

## 7. Channel access basics
- Wireless nodes share the medium.
- Random access can cause **collisions**.
- A **collision** occurs when **two nodes transmit at the same time**.

### Important timing terms
- **Slot time**: defined by the standard
- **Frame**: transmitted data spanning one or more slots

## 8. DCF and CSMA
### DCF — Distributed Coordination Function
- Collision-avoidance rule set used in regular contention-based Wi‑Fi
- Based on **CSMA**
- A station transmits only when it senses the channel is **idle**
- The channel must remain idle for **DIFS**

### Inter-frame spaces
- **DIFS** = **DCF Inter Frame Space**
- **SIFS** = **Short Inter Frame Space**
- The lecture gives:
  - **DIFS = SIFS + 2 slot times**

### ACK
- Positive **ACK** confirms successful frame reception.

## 9. Virtual carrier sense
- Each frame contains a **Duration ID**
- This indicates how long the medium will remain busy
- Stations maintain a **NAV (Network Allocation Vector)** timer
- While NAV is non-zero, a station defers access

## 10. Why CSMA alone is not enough
- Even if nodes sense the channel, **collisions can still happen**
- This is why Wi‑Fi uses **CSMA/CA**

### CSMA/CA and exponential backoff
- Each station selects a random **CW (Contention Window)**
- Backoff counter decreases while the medium is idle
- When the counter reaches zero and the medium is free, the node transmits
- If the medium is still unavailable:
  - **CW = min(2CW + 1, CWmax)**

## 11. Hidden-node problem
- Two nodes may both reach the AP
- But not hear each other
- They may therefore transmit simultaneously and collide at the AP

### RTS/CTS solution
- Sender sends **RTS (Request To Send)**
- AP responds with **CTS (Clear To Send)**
- Other stations hearing CTS defer access using NAV
- RTS may still collide, but RTS packets are short, so wasted airtime is smaller

## 12. Overall channel-access sequence
A typical exchange in the lecture includes:
- **DIFS**
- **Backoff**
- **RTS**
- **CTS**
- **SIFS**
- **Data**
- **ACK**

The example slide states a propagation delay of **2 slots**.

## 13. PCF — Point Coordination Function
- Intended for **time-critical services**
- AP acts as a **point coordinator**
- AP polls devices and grants scheduled transmission opportunities
- Built as an **optional mechanism on top of DCF**

### Important PCF terms
- **CFP** = Contention-Free Period
- **PIFS** = PCF Inter Frame Space
- During CFP:
  - ordinary DCF access is preempted
  - stations using DCF defer access through NAV

## 14. Data rate and MCS
### MCS — Modulation and Coding Scheme
- MCS combines:
  - modulation choice
  - coding rate
- Used to improve data rate while preserving signal quality
- Selection depends on channel quality

### SNR
- **SNR** = signal-to-noise ratio
- Higher SNR generally allows higher data rates with fewer errors

### MCS index
- Standards assign an index to each modulation/coding combination
- This number is the **MCS index**

## 15. Power management in legacy Wi‑Fi
### Two radio states
- **Awake**
  - can receive and transmit
- **Doze**
  - radio module switched off
  - cannot receive or transmit

### Two station modes
- **Power Save (PS) mode**
  - alternates between awake and doze
- **Active mode**
  - always awake

### AP support for sleeping devices
- AP buffers frames for stations in PS mode
- Beacons include **TIM (Traffic Indication Map)**
- TIM tells whether buffered traffic exists for each station

### Listen interval and DTIM
- A PS station wakes periodically to hear beacons
- It may sleep for a **listen interval** longer than the beacon period
- In practice, stations often wake just before a **DTIM beacon**

### PS-Poll
- Station sends **PS-Poll**
- AP returns buffered frames

## 16. Types of 802.11 frames
Three categories are highlighted:
- **Data frames**
  - user data transmission
- **Control frames**
  - access-control support such as **RTS**, **CTS**, **ACK**
- **Management frames**
  - transmitted similarly to data frames
  - not forwarded to upper layers

### Frame components shown in lecture
- **Preamble**
- **PLCP Header**
- **MAC Data**
- **CRC**

## 17. Why legacy Wi‑Fi is not ideal for all IoT devices
The lecture argues that IoT devices are heterogeneous:
- some are **high-end / heavy traffic**
- some are **moderate traffic**
- some are **light traffic / energy constrained**

A symmetric design is inefficient because:
- not all devices need the same sampling/listening behavior
- legacy Wi‑Fi was built mainly for:
  - **high throughput**
  - **few stations**
  - **indoor**
  - **short distance**

## 18. IEEE 802.11ah / Wi‑Fi HaLow
### Identity
- **IEEE 802.11ah**
- called **Wi‑Fi HaLow** by the Wi‑Fi Alliance

### Purpose
- **Low-rate, long-range IoT**
- designed for:
  - short data transmissions
  - power savings
  - efficient MAC behavior

### Spectrum
- **Sub-GHz license-exempt spectrum**
- regional examples shown in the lecture:
  - USA: **902–928 MHz**
  - Europe: **863–868.6 MHz**
  - Japan: **916.5–927.5 MHz**
  - China: **755–587 MHz** *(as shown on the lecture slide)*
  - Korea: **917.5–923.5 MHz**

### Benefits of Sub-GHz
- **longer range**
- **less congestion**
- **better penetration**

### Scale target
- support at least **4× more devices per AP** than legacy 802.11

## 19. 802.11ah PHY characteristics
- Inherited from **802.11ac**
- adapted to **sub-1 GHz**
- lecture describes PHY as **down-clocked by 10×**
- narrower channels than legacy/ac

### Channel widths mentioned
- introductory slide: **2 / 4 / 8 / 16 MHz**
- detailed comparison slide: **1 / 2 / 4 / 8 / 16 MHz**
- **1 MHz and 2 MHz are mandatory** according to the detailed slide

### Transmission method
- **OFDM**

### Data subcarriers
- **1 MHz** channel → **24 data subcarriers**
- **2 MHz** channel → **52 data subcarriers**

## 20. 802.11ah scaling and efficiency improvements
### Larger AID space
- Legacy Wi‑Fi maximum AID: **2007**
- 802.11ah maximum AID: **8191**
- enables support for many more devices

### Header reduction
- Legacy infrastructure MAC header with 3 addresses is **30 bytes**
- **FCS** adds another **4 bytes**
- With a **100-byte payload**, overhead exceeds **30%**
- 802.11ah therefore introduces a **short MAC header**

### RID — Response Indication Deferral
- Short header omits **Duration/ID**
- This motivates **RID**
- Difference from NAV:
  - **RID** is set right after the **PHY header**
  - **NAV** requires the **whole frame** to be received
- RID estimates duration using the **2-bit Response Indication field**

### NDP MAC frames
- **Null Data Packet (NDP)** MAC frames reduce overhead
- Useful because frames like **ACK** and **CTS** may carry no payload but still cost airtime
- 802.11ah extends the idea so useful information can be conveyed in the PHY header SIG field

### Short beacons
- 802.11ah uses:
  - **full beacons**
  - **short beacons**
- Short beacons omit or shorten some fields to reduce:
  - medium occupancy
  - AP energy
  - station receive energy

## 21. Handling many devices in 802.11ah
### RAW — Restricted Access Window
- Used because many devices create **high collision probability**
- RAW:
  - divides stations into groups
  - splits the channel into slots
  - assigns slots to groups
- Stations transmit only in their assigned slots

### Fast association and authentication
The lecture describes two approaches for large populations:
- **Centralized authentication control**
- **Distributed authentication control**

#### Centralized
- AP announces a **control threshold** in beacon
- device generates random number in **[0, 1022]**
- if value is less than threshold, it attempts authentication

#### Distributed
- station generates:
  - **l** in **[0, L]**
  - **m** in **[0, TI]**
- it attempts authentication in selected beacon interval / slot
- if unsuccessful, **TI** increases like binary exponential backoff

## 22. Longer sleep support in 802.11ah
### Increased doze duration
- Legacy max idle period is a **16-bit field** counted in **1.024 ms** units
- 802.11ah modifies max idle handling in two ways:
  1. add a **scaling factor**
  2. allow different max idle periods per station

### Scaling-factor values
The two most significant bits represent:
- `00` → **1**
- `01` → **10**
- `10` → **1000**
- `11` → **10,000**

This makes the maximum value about **2500× greater**.

## 23. 802.11ah BSS modes
The lecture lists three BSS operation modes:
- **Sensor Only BSS**
- **Non-sensor only BSS**
- **Mixed mode**

These can be separated spatially or by channel to reduce interference from non-sensor stations.

## 24. TIM segmentation
- TIM information may become large with many stations
- 802.11ah supports **TIM segmentation**
- Information is split into multiple segments
- Grouping hierarchy:
  - **pages**
  - **blocks**
  - **subblocks**

## 25. Relays in 802.11ah
- 802.11ah can extend a hotspot-style network using **Relays**
- Relays forward frames between:
  - stations associated with the relay
  - the **Root AP**

### Benefits
- extends distance to edge stations
- improves reliability when there are obstructions
- reduces energy usage because a station may:
  - transmit at **lower power**
  - transmit at **higher rate** due to shorter distance

## 26. Big-picture takeaway
Lecture 8 covers two things:
1. how **legacy Wi‑Fi / IEEE 802.11** works
2. how **Wi‑Fi HaLow / IEEE 802.11ah** adapts Wi‑Fi for IoT

The key story is:
- legacy Wi‑Fi is powerful and widely deployed but not ideal for huge fleets of light-traffic, battery-constrained devices
- **802.11ah** addresses this with:
  - sub-GHz operation
  - narrower channels
  - more device support
  - lower overhead
  - longer sleep cycles
  - better scaling mechanisms such as **RAW**, **TIM segmentation**, and **relay support**
