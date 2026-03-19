# Lecture 9 — Cellular IoT

## 1. Cellular-IoT preliminaries
- In cellular systems, user messages are first represented as **bits (1 and 0)**.
- These bits are **not transmitted directly as raw bit strings** over the air.
- Before wireless transmission, groups of bits are mapped into **symbols**.
- This process is called **modulation**.

## 2. Modulation basics
### BPSK
- **BPSK** = **Binary Phase Shift Keying**.
- The lecture gives:
  - bit **1** → symbol **+1**
  - bit **0** → symbol **-1**

### QPSK
- **QPSK** = **Quadrature Phase Shift Keying**.
- In QPSK, **every 2 bits are assigned to 1 symbol**.
- This means QPSK carries more bits per symbol than BPSK.

### Carrier multiplication
- To generate the transmitted signal, the symbols are multiplied by a **carrier**.
- The lecture expresses the carrier as approximately:
  - **cos(2πfct)**
- Here **fc** is the **carrier frequency**.

## 3. Signal representation
- A wireless signal can be represented in:
  - the **time domain**
  - the **frequency domain**

## 4. Why multiple access is needed
- Cellular systems must support **multiple users** sharing physical wireless resources.
- The lecture introduces these multiple-access schemes:
  - **TDMA** — Time Division Multiple Access
  - **FDMA** — Frequency Division Multiple Access
  - **CDMA** — Code Division Multiple Access
  - **OFDMA** — Orthogonal Frequency Division Multiple Access

## 5. Core multiple-access ideas
### FDMA
- Users are separated mainly by **frequency**.
- Guard bands may be inserted to reduce interference between allocations.

### TDMA
- Users share the same spectrum but transmit in **different time slots**.

### CDMA
- Users are separated by **codes** (spreading sequences).
- The lecture discusses:
  - **PN sequences**: pairwise cross-correlation is approximately zero
  - **orthogonal codes**: pairwise cross-correlation is exactly zero
- In CDMA reception, the receiver can **despread** the received signal using the desired user’s code.

### OFDMA
- The lecture contrasts OFDMA with TDMA/FDMA/CDMA by noting that those schemes allow one symbol per user at a point in time, while **OFDMA can transmit multiple symbols at a given point of time**.
- OFDMA uses multiple **subcarriers**.
- The slide also notes an implementation cost of **N modulators and N demodulators**.

## 6. OFDMA resource allocation example
From the lecture example:
- **Time slot** = **0.5 ms**
- **Sub-band size** = **15 kHz**
- **Physical Resource Block (PRB)** = **12 subcarriers over one time slot**
- **Minimum allocation** = **2 PRBs per frame**

### Useful derived calculations
- If each subcarrier is **25 kHz** instead of **15 kHz**, then one PRB bandwidth becomes:
  - **12 × 25 kHz = 300 kHz**
- If the available usable spectrum is **900 kHz**, then at **15 kHz per subcarrier**, the maximum number of subcarriers is:
  - **900 / 15 = 60**

## 7. LTE-M
- **LTE-M** is introduced in **4G/LTE-Advanced**.
- It targets **machine-type applications**.

### Example application types from the lecture
- **Cameras**
  - high uplink traffic
  - no mobility
- **Fleet tracking**
  - low traffic
  - high mobility
- **Meter reading**
  - very low traffic
  - no mobility

### LTE-M design considerations
- **Signaling overhead reduction** for devices with infrequent transfers
- Expected **UE behavior** is communicated to the **eNB**
- **Power optimization** matters because some devices, such as meters, are battery-powered
- Devices may use power-saving behavior to **sleep for long periods**

## 8. NB-IoT overview
- **NB-IoT** = **Narrowband IoT**
- It was introduced in **LTE-Advanced Pro**
- It is a **3GPP initiative**
- It is designed for:
  - **very low data rate** devices
  - **long-range** operator-network connectivity
  - **low-power** operation

### Standardization goal
- As a cellular standard, NB-IoT aims to make IoT devices more:
  - **interoperable**
  - **reliable**

## 9. What NB-IoT uses from LTE
- NB-IoT reuses LTE design extensively.
- The lecture explicitly states:
  - **Downlink (DL)**: **OFDMA**
  - **Uplink (UL)**: **SC-FDMA**

## 10. Key NB-IoT characteristics
- Narrow bandwidth support: **180 kHz**
- Extended coverage with around **164 dB** maximum coupling loss / link budget (standalone case)
- Example receiver sensitivity: **-141 dBm**
- Long battery life target: about **10 years** with a **5 Wh battery** depending on usage
- Massive scale: at least **50,000 devices per cell**

## 11. Link budget concept
- The lecture describes **link budget** as the allowed power loss in the link:
  - **Link Budget = Transmit Power − Received Power**
- Intuition:
  - if receiver sensitivity is poor, more transmit power is needed
  - stronger link budgets support better coverage

### Example calculation
If:
- link budget = **170 dB**
- minimum received power / sensitivity = **-150 dBm**

Then:
- **170 = Tx − (-150)**
- **Tx = 20 dBm**

## 12. NB-IoT power-saving features
### Power Save Mode (PSM)
- In LTE, devices must periodically perform tracking area updates.
- NB-IoT devices can extend this timer to **several days**.
- This helps reduce energy usage significantly.

### eDRX — Extended Idle Mode Discontinuous Reception
- Normal LTE idle paging interval in the lecture: **1.28 s**
- NB-IoT devices can request extension to **20.48 s up to 175 minutes**
- If accepted by the network, the device can remain powered off for that period **without losing state**, including its **IP address**

## 13. NB-IoT communication style
- NB-IoT is intended for **small amounts of data**
  - typically **tens or hundreds of bytes per day**
- It is **message-based**, similar in spirit to **Sigfox** and **LoRa**
- The lecture notes that it can still handle more data than those technologies due to a faster modulation rate

## 14. NB-IoT vs LTE-M
- The lecture explicitly states that **NB-IoT is not an IP-based communication protocol like LTE-M**.
- It is aimed at:
  - simpler devices
  - more power efficiency
  - more infrequent communication

### Important mobility limitation
- NB-IoT devices **cannot move while they have an active connection**.
- They must:
  1. disconnect
  2. move
  3. reconnect

This is one reason NB-IoT is better suited to mostly **static assets**.

## 15. NIDD and SCEF
### NIDD
- **NIDD** = **Non-IP Data Delivery**
- It avoids carrying full IP headers for every small data transfer
- This is useful for small NB-IoT payloads

### SCEF
- **SCEF** = **Service Capability Exposure Function**
- In the lecture, SCEF:
  - encapsulates / decapsulates packets
  - sends / receives data without IP headers to/from the NB-IoT device

### Important implication
- NB-IoT is **not used like a smartphone IP connection**
- To connect to IP networks, the lecture notes that **IP tunneling** is required

## 16. Frequency-band and deployment modes
The lecture gives:
- **128 kbps** using a **180 kHz** band
- one 180 kHz band corresponds to a **single PRB of 12 subcarriers (12 tones)**

### Three NB-IoT deployment modes
#### Stand-alone
- Uses a **stand-alone carrier**
- Example given: spectrum previously used by **GERAN/GSM**

#### Guard-band
- Uses **unused resource blocks within an LTE carrier’s guard band**

#### In-band
- Uses **resource blocks within a normal LTE carrier**

### Additional note
- The lecture states that all three deployment modes are **invisible to non-NB-IoT devices**

## 17. Advantages of NB-IoT
The lecture lists these advantages:
- **Very good coverage**
- Works well **indoors** and in **dense urban areas**
- **Faster response times than LoRa**
- Better **quality of service** than LoRa
- **Minimal power consumption** during operation
- **Lower component cost**
- **Deeper building penetration than LTE-M**

## 18. Disadvantages of NB-IoT
The lecture lists these disadvantages:
- **FOTA / file transfer is difficult**
  - downlinking larger amounts of data is hard
- **Network and tower handoffs are problematic**
  - therefore NB-IoT is better for **static** assets than roaming ones

## 19. Early-stage non-technical issues
The lecture notes a historical ecosystem issue:
- the 3GPP NB-IoT specification had **two competing variants**
  - **Huawei / Vodafone**
  - **Ericsson / Nokia / Intel**

### Characterization of the two approaches
- **Ericsson** approach:
  - scaled-down, lower-power variant of 4G
- **Huawei** approach:
  - “clean-sheet” approach

The lecture also mentions compatibility / infrastructure-upgrade concerns in the early stage.

## 20. NB-IoT going forward in 5G
The lecture gives three 5G-NR application areas:

### eMBB — Enhanced Mobile Broadband
- better mobile phones and hot spots
- high data rates
- high user density
- human-centric communications

### URLLC — Ultra-Reliable and Low-Latency Communications
- vehicle-to-vehicle communication
- Industrial IoT
- 3D gaming
- human and machine centric communication

### mMTC — Massive Machine-Type Communications
- huge number of devices
- low data rate
- low power
- long battery life
- machine-centric communication

The lecture also notes that 5G mMTC includes support for **LTE-M and NB-IoT**.

## 21. Network slicing in 5G
- 5G can support different service areas on the **same physical infrastructure**
- **Network slicing** divides bandwidth and resources across these service areas
- This allows different service functions to be **customized**
- Higher-priority applications can be **prioritized**
- Each slice can be configured independently in terms of:
  - **security**
  - **quality of service**
  - **network provisioning**

## 22. Example shown in lecture
- The lecture includes **“NB-IoT in 5G: A Smart Parking Solution”** as an example application scenario.

## 23. Exam-oriented comparisons to remember
### LTE-M vs NB-IoT
- **LTE-M**
  - IP-based
  - supports mobility better
  - useful for machine-type communication with more movement
- **NB-IoT**
  - not IP-based in the same way
  - uses NIDD / SCEF support
  - more power efficient
  - better suited to infrequent, small-message, mostly static deployments

### TDMA / FDMA / CDMA vs OFDMA
- TDMA, FDMA, CDMA: a user typically transmits **one symbol at a point in time**
- OFDMA: can transmit **multiple symbols at a given point in time** using multiple subcarriers

## 24. Quick memory list
- **BPSK**: 1 bit mapping example with +1 / -1 symbols
- **QPSK**: 2 bits per symbol
- **PRB**: 12 subcarriers over one time slot
- **Time slot example**: 0.5 ms
- **Sub-band example**: 15 kHz
- **NB-IoT channel**: 180 kHz
- **NB-IoT link budget**: 164 dB
- **Receiver sensitivity example**: -141 dBm
- **Battery target**: 10 years with 5 Wh battery
- **Scale**: at least 50,000 devices/cell
- **eDRX**: 20.48 s to 175 min
- **Deployment modes**: stand-alone, guard-band, in-band
