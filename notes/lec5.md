# Lecture 5 — LoRa & LoRaWAN

## 1. Where LoRa/LoRaWAN fit in IoT
- **LoRa/LoRaWAN** are part of the **LPWAN** family: **Low Power Wide Area Networks**.
- LPWAN targets **low-power, low-bandwidth IoT devices over long distances**.
- It is suitable for sensors that send **small amounts of data intermittently**.
- In the lecture’s connectivity pyramid, LoRa sits in the **simple, low-cost LPWA** tier.

## 2. LPWAN characteristics
LPWAN focuses on:
- **Energy efficiency**
- **Massive scalability**
- **Cost-effective communication**
- **Deep penetration**
- **Long battery life**
- **Low bandwidth**

The lecture also distinguishes:
- **Cellular LPWAN**: NB-IoT, LTE-M / Cat-M1
- **Non-cellular LPWAN**: **Sigfox, LoRa**

## 3. LoRa basics
- LoRa was originally developed by **Cycleo**.
- Cycleo was acquired by **Semtech** in **2012**.
- The lecture notes LoRa products became rapidly available after public release.
- LoRa is the **radio / PHY side**, not the full networking protocol stack.

### Key properties of LoRa
- **Long range**
- **Low power**
- **Low data rate**
- Good fit for large-scale sensing rather than heavy data transfer

## 4. Frequency usage
The lecture lists LoRa operation in **ISM license-exempt bands**:
- **915 MHz** in the US
- **868 MHz** in Europe
- **433 MHz** in Asia

For Singapore, the lecture later uses the **AS923-1** example with common uplink channels:
- **CH0 = 923.2 MHz**
- **CH1 = 923.4 MHz**
- **CH2 = 923.6 MHz**

Also noted:
- In the example deployment, **all gateways report to the same server**
- A device can **talk to any gateway**
- The network server can offer the allowed channel list
- The device still chooses its actual uplink channel according to the allowed channels, regional rules, and its internal hopping algorithm

## 5. LoRa modulation
LoRa uses **Chirp Spread Spectrum (CSS)**.

### What CSS means
- Information is encoded using **frequency-modulated chirps**
- A chirp is a tone whose frequency:
  - **increases with time** → **up-chirp**
  - **decreases with time** → **down-chirp**
- Spread spectrum deliberately spreads signal power across a wider frequency range

### Benefits of CSS
- **Resistance to interference**
- **Increased range**
- **Improved robustness / harder unnoticed manipulation**
- Supports multiple simultaneous transmissions using different spreading settings

## 6. Spreading Factor (SF) and Coding Rate (CR)

### Spreading Factor
Spreading Factor controls the trade-off between:
- **Range**
- **Data rate**
- **Airtime**

Higher **SF**:
- **Improves range**
- **Lowers data rate**
- **Increases Time-on-Air**
- Can increase collisions and energy use if overused

### Time-on-Air examples from the slide
For a 12-byte message at BW = 125 kHz and CR = 4/5:
- **SF7 ≈ 56 ms**
- **SF9 ≈ 246 ms**
- **SF12 ≈ 1483 ms**

This means:
- **SF12** uses far more airtime than **SF7**
- Longer airtime reduces channel capacity and can hurt battery life

### Coding Rate
- Higher **CR** → more redundancy → more robustness
- Lower **CR** → higher throughput

### Design takeaway
- The lecture explicitly recommends: **use ADR whenever possible**

## 7. LoRaWAN basics
LoRaWAN is **not the same thing as LoRa**.

### LoRaWAN definition
- LoRaWAN is a **standardized MAC-layer protocol built on top of the LoRa PHY**
- It uses a **Star-of-Stars** topology:
  - **End devices → Gateways → Centralized Network Server**

### Key LoRaWAN features
- **Bidirectional communication**
- **Controlled downlinks**
- Support for things like **firmware updates**
- **Low data rates** (lecture: typically **0.3–22 kbps**)
- **Strong security** using **AES-128-based keys**
- Good robustness due to LoRa PHY underneath

### Important distinction
- **LoRa** = radio / modulation layer
- **LoRaWAN** = networking behavior over LoRa radio

## 8. EUIs and identity
The lecture makes one explicit correction:
- **DevEUI** and **JoinEUI** are **64-bit identifiers**
- They are **not keys**

Their job is identity, not encryption.

## 9. LoRaWAN network server functions
The network server acts as the **brain** of the network.

The lecture lists these key functions:
- **Device assignment**
  - Assign frequency, spreading code, and initial data rate
- **Optimized communication**
  - Prevent duplicate packets
  - Schedule acknowledgments
  - Adjust data rates
- **Network synchronization**
  - Important for **Class B**
- **Security management**
- **ADR support**

The summary slide also states:
- Devices broadcast uplinks that may be received by multiple gateways
- The server handles duplication and chooses the best path for downlink behavior

## 10. Three LoRaWAN device classes

### Class A
- Mandatory baseline class
- **Most energy efficient**
- Device listens only **after its own uplink transmission**
- Best for **battery-powered sensors**
- Example use cases:
  - Fire detection
  - Earthquake early detection

### Class B
- Uses **scheduled receive slots**
- Synchronized by **beacons**
- Good for battery-powered devices needing controlled downlink latency
- Example use cases:
  - Smart metering
  - Temperature rise monitoring

### Class C
- Device is **always listening**
- Best for very low downlink latency
- Suitable for devices with **no strict power constraint**
- Often **grid-powered**
- Example use cases:
  - Fleet management
  - Real-time traffic management

## 11. Adaptive Data Rate (ADR)
ADR = **Adaptive Data Rate**

### What ADR does
ADR automatically adjusts a device’s data rate based on:
- Signal quality
- Distance to gateway
- Interference levels
- Communication needs

### Why ADR matters
- **Improves network capacity**
- **Reduces power consumption**
- **Optimizes transmission speed**
- Helps avoid wasting airtime

### Example from the lecture
- Close to gateway → can use **higher data rate**
- Further away / more interference → uses **lower data rate** to stay reliable

## 12. FUOTA
FUOTA = **Firmware Update Over-The-Air**

### Building blocks
- **Remote Multicast Setup**
- **Fragmentation**
- **Clock Synchronization**

### Why FUOTA is hard
- Firmware images are large
- LoRaWAN payloads are small
- The lecture gives an example payload size of only **51 bytes**
- Without **Class B or Class C**, **Class A** can send only **1 fragment per uplink**
- Scalable FUOTA therefore relies on **Class B/C**

### Typical flow
1. Join multicast group
2. Sync clocks
3. Multicast fragments
4. Device reassembles and validates the firmware

## 13. MIOTY
The lecture briefly introduces **MIOTY**.

### Key idea
- Based on **Telegram Splitting Multiple Access (TSMA)**
- Applies **FEC** to the full message before splitting
- Splits a message into many tiny sub-packets

### Benefits listed
- Can recover the message with only **half** the sub-packets
- Survives interference and collisions
- Supports massive network capacity
- Works well in noisy spectrum

## 14. Important exam-style distinctions

### LoRa vs LoRaWAN
- **LoRa**: physical layer radio modulation
- **LoRaWAN**: MAC/networking behavior over LoRa

### High SF vs low SF
- High SF:
  - more range
  - less data rate
  - more airtime
- Low SF:
  - less range
  - more data rate
  - less airtime

### Device classes
- **A**: lowest power, receives after transmit
- **B**: scheduled receive slots
- **C**: always listening, lowest latency, highest power use

## 15. Quick memory list
- **LPWAN** = long range + low power + low bandwidth
- **LoRa** = **CSS modulation**
- **LoRaWAN** = **MAC over LoRa PHY**
- **Topology** = **Star-of-Stars**
- **ADR** = adaptive rate tuning for efficiency
- **Class A/B/C** = energy vs latency trade-off
- **FUOTA** needs fragmentation and usually **Class B/C**
- **MIOTY** = split message + FEC + robustness
