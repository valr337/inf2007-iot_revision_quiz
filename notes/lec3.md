# CSC2106 Lecture 3 — CoAP & MQTT

## What this lecture covers
Lecture 3 introduces two major IoT application-layer protocols:

- **CoAP** for constrained devices and constrained networks
- **MQTT** for broker-based publish/subscribe communication

The lecture also covers message models, reliability behavior, topic design, MQTT 5 additions, and the practical trade-offs between CoAP and MQTT.

---

## 1) CoAP overview

**CoAP** stands for **Constrained Application Protocol**.

Key ideas from the lecture:

- Developed by the **Constrained Resource Environments (CoRE)** IETF group
- A **REST-based web transfer protocol**
- Runs over **UDP** instead of TCP
- Designed for **resource-constrained microcontrollers**
- Can adapt **simple HTTP interfaces** into a more compact protocol
- Supports **GET, PUT, POST, DELETE, and OBSERVE**
- Re-designed for **low-power embedded devices** such as sensors
- Supports **multicast** and **congestion control**

### Why CoAP exists
HTTP works well on the traditional Internet, but it is relatively heavy for tiny IoT devices. CoAP keeps the REST style while reducing overhead for constrained environments.

---

## 2) CoAP architecture and stack

The lecture shows CoAP and HTTP working together across constrained and traditional Internet environments.

### CoAP protocol stack
- **CoAP**
  - **Request/Response sub-layer**
  - **Message sub-layer**
- **UDP**
- **IP**
- **Link layer**

### Role of each CoAP sub-layer
**Request/Response sub-layer**
- Handles methods such as **GET, POST, PUT, DELETE**
- Works with **URIs** and Internet media types

**Message sub-layer**
- Handles **deduplication**
- Handles **optional retransmissions**
- Supports confirmable communication using **CON**

---

## 3) CoAP message format

The lecture highlights a **4-byte message header**.

Important fields:
- **Ver** — Version
- **T** — Message type
- **TKL** — Token length
- **Code** — Request method or response code
- **Message ID** — 16-bit identifier for matching responses
- **Token** — Optional token used for response matching

### Example mapping
A typical example shown is:

- Client sends: `CON GET /humidity`
- Server replies: `ACK 2.05 Content`

This shows how CoAP maps request and response behavior into compact messages.

---

## 4) CoAP message types

The lecture gives four message types:

### 1. Confirmable (CON)
- Requires an acknowledgement
- Used for reliable delivery

### 2. Non-confirmable (NON)
- No acknowledgement needed
- Used when reliability is less critical

### 3. Acknowledgement (ACK)
- Acknowledges a confirmable message

### 4. Reset (RST)
- Means a confirmable message was received, but context is missing for processing

---

## 5) CoAP request/response patterns

The lecture shows several request/response patterns.

### Piggy-backed response
- A confirmable request is answered directly in the ACK
- Example: `CON GET /light` followed by `ACK 2.05 Content`

### Dealing with packet loss
- If no reply arrives, the client can **retransmit** the confirmable request after timeout

### Separate response
- The server first acknowledges that it received the request
- The actual response is sent later when ready

### Non-confirmable request/response
- Both request and response can use NON when acknowledgement is not needed

---

## 6) CoAP observe, discovery, and block transfer

### Observe
CoAP supports observation of resource changes.

The lecture shows a client sending:

- `GET /light Observe: 0`

After that, the server can push updated representations whenever the resource changes.

### Resource discovery
Resource discovery uses:

- `GET /.well-known/core`

This lets the client discover available resources, for example:
- temperature resource
- light resource

### Block transfer
Used when the payload is too large to fit comfortably in one transfer.

Important fields:
- **nr** — block number
- **m** — more blocks flag
- **sz** — block size

---

## 7) Other CoAP points

The lecture briefly lists:

- **Intermediaries and caching**
- **DTLS** for security
- **Alternative transport** such as SMS
- **Message header options**

### Security
**DTLS** is described as **TLS/SSL for datagrams**, which fits CoAP’s UDP-based design.

---

# 8) MQTT overview

**MQTT** stands for **Message Queuing Telemetry Transport**.

The lecture states that MQTT was invented by:

- **Andy Stanford-Clark**
- **Arlen Nipper**

Original motivation:
- minimal battery loss
- minimal bandwidth usage
- satellite-connected oil pipeline systems

### MQTT goals from the lecture
- Simple to implement
- Bi-directional asynchronous **push** communication
- QoS data delivery
- Lightweight and bandwidth efficient
- Data agnostic
- Continuous session awareness

---

## 9) MQTT protocol characteristics

From the lecture:

- Built for embedded systems and now widely used in IoT
- Can send **anything as a message**
- Designed for **unreliable networks**
- Used from hobby projects to enterprise-scale deployments
- **Decouples readers and writers**
- Each message has:
  - a **topic**
  - a **QoS**
  - a **retain** status
- MQTT is **binary-based**, not text-based

### MQTT stack
- **Application**
- **MQTT**
- Optional **TLS/SSL**
- **TCP**
- **IP**

### Ports
- **1883** — default MQTT port
- **8883** — MQTT over TLS/SSL

---

## 10) MQTT roles

The lecture identifies three parts:

- **Broker**
- **Publishers**
- **Subscribers**

### Core idea
Publishers send messages to the broker.
Subscribers receive messages from the broker based on topic subscriptions.

This means:
- clients do **not** need each other’s IP addresses
- clients **do** need the broker’s IP and port

---

## 11) MQTT topics

Every published message has a **topic**.

Example format from the lecture:

- `home/rooms/kitchen/temperature`

This is a hierarchy of slash-separated sub-topics.

### Wildcards
- **`+`** = single-level wildcard
- **`#`** = multi-level wildcard

Examples from the slides:
- `myhome/bedroom/+` → all bedroom data
- `myhome/+/temperature` → temperature data for multiple rooms
- `myhome/#` or `#` → everything

---

## 12) MQTT topic best practices

The lecture lists several naming practices:

- Do **not** use topics beginning with `$`
- Do **not** use a leading slash
- Embed a **unique identifier** or client ID when useful
- Plan for **future extensibility**
- Use **specific** topics instead of very general ones
- Do **not** subscribe to `#` casually
- Do **not** use spaces in topics
- Keep topics **short and concise**
- Use **ASCII** only and avoid non-printable characters

These are common exam traps.

---

## 13) MQTT message format and message types

The lecture states that the **shortest MQTT message is two bytes**.

The fixed header contains fields such as:
- Message Type
- DUP
- QoS Level
- RETAIN
- Remaining Length

The message type table includes examples such as:
- CONNECT
- CONNACK
- PUBLISH
- PUBACK
- PUBREC
- PUBREL
- PUBCOMP
- SUBSCRIBE
- SUBACK
- UNSUBSCRIBE
- UNSUBACK
- PINGREQ
- PINGRESP
- DISCONNECT

---

## 14) MQTT QoS levels

### QoS 0 — At most once
- Lowest overhead
- “Fire and forget”
- Good for continuous streams

### QoS 1 — At least once
- Message may be duplicated
- More overhead than QoS 0
- Often used where message loss is unacceptable

### QoS 2 — Exactly once
- Highest reliability
- Highest overhead
- Slowest of the three

### Quick comparison
- **QoS 0**: cheapest, least reliable
- **QoS 1**: practical middle ground
- **QoS 2**: most reliable, most expensive

---

## 15) MQTT retained messages

A retained message stores the **latest value** for a topic at the broker.

Effect:
- When a new subscriber joins later, it can immediately receive the **last retained state**

This is useful for state-like data such as:
- battery level
- current sensor value
- device online/offline state

---

## 16) MQTT Last Will and Testament (LWT)

LWT lets a client define a message that the broker publishes if the client disappears unexpectedly.

Typical use:
- device status topic
- notify others that the client failed or disconnected

The lecture example shows the broker publishing a status update like `"Bye!"` after keep-alive failure.

---

## 17) Paho-MQTT Python example

The lecture includes a Python example using `paho.mqtt.client`.

The example shows:
- creating a client
- connecting to a broker
- subscribing to one or more topics
- publishing with a chosen QoS
- using retain = FALSE in the example

Important reminder:
- the sample uses **port 1883**
- the sample also shows **subscribing to multiple topics**

---

## 18) MQTT 5 additions

The lecture lists these MQTT 5 features:

- **User Properties**
- **Shared Subscriptions**
- **Payload Format**
- **Request-Response**
- **Topic Alias**
- **Enhanced Authentication**
- **Flow Control**
- **Expiry Interval**
  - message
  - session
- More reason codes and better client feedback
- No retransmission for healthy TCP connections
- Passwords without usernames for authentication

---

## 19) MQTT 5 details to remember

### User Properties
Extra metadata attached to messages.

### Shared Subscriptions
Format:
- `$share/GROUPID/TOPIC`

Used to share load across multiple subscribers in the same group.

### Payload Format Indicator
- **0** = unspecified byte stream
- **1** = UTF-8 encoded payload

### Content Type
A MIME-like content descriptor.

### Topic Alias
Useful when:
- payloads are small
- topics are long
- throughput is high

### Flow Control
**Receive Maximum** limits how many unacknowledged PUBLISH messages a client can handle.

Default if unspecified:
- **65,535**

### Enhanced Authentication
- challenge-response style
- uses **AUTH** packets
- example methods mentioned: **SCRAM** and **Kerberos**

---

## 20) MQTT security outside the base protocol

The lecture separates these from MQTT itself.

### TLS / SSL
- Encrypts the client-broker connection
- Helps prevent eavesdropping, tampering, and MITM attacks
- Certificate verification helps confirm the broker is genuine

Important note from the lecture:
- **SSL** is old and deprecated
- **TLS** is the modern secure protocol

### ACL (Access Control List)
Implemented by the **broker**, not part of MQTT itself.

ACL defines:
- who can publish
- who can subscribe
- which topics each client can access

Benefits listed in the lecture:
- security
- privacy
- multi-tenancy
- stability
- integrity

---

## 21) MQTT vs CoAP

The lecture ends with a comparison.

### When MQTT is often preferred
- Reliability in unstable networks
- Better support for high-latency networks
- Strong ecosystem and widespread adoption
- Scales well with many devices
- Easy TCP/IP integration
- Strong broker-based publish/subscribe model

### When CoAP is often preferred
- Extremely low-power devices
- Very constrained processing environments
- UDP-based networks
- Direct web integration via REST/HTTP proxying
- Native multicast
- Lightweight device-to-device interaction

---

## 22) Fast revision checklist

### CoAP
- UDP
- REST-based
- 4-byte header
- CON / NON / ACK / RST
- Observe
- `/.well-known/core`
- Block transfer: `nr`, `m`, `sz`
- DTLS for security

### MQTT
- TCP
- Broker / Publisher / Subscriber
- Topic-based pub/sub
- Port **1883**, TLS port **8883**
- QoS 0 / 1 / 2
- Retain
- LWT
- Wildcards `+` and `#`
- MQTT 5: shared subscriptions, user properties, payload format, topic alias, flow control, enhanced auth
- ACL is broker-side, not part of core MQTT

---

## 23) Common traps

- **MQTT default port is 1883, not 1833**
- **CoAP uses UDP, MQTT uses TCP**
- **CoAP is REST-style; MQTT is publish/subscribe**
- **`+` matches one level, `#` matches many**
- **QoS 1 can duplicate**
- **QoS 2 is exactly once, but slowest and most expensive**
- **Retain is not the same as QoS**
- **ACL is not part of MQTT itself**
- **TLS is current; SSL is deprecated**
