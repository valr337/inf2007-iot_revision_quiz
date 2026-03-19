# Lecture 4 — Wireless Mesh

## 1. What is a wireless mesh network?
- A wireless mesh network is a **decentralized** network architecture made up of radio nodes organised in a **mesh topology**.
- Nodes connect **directly and dynamically** to multiple other nodes without relying on a central router or server.
- Each node can **send and receive data**.
- Reliability is improved because traffic can be **rerouted through alternative paths** when nodes or paths fail.
- Typical use cases: **IoT, smart cities, broadband home networking, emergency communications**.

## 2. Why mesh?
- Failure of one device usually does **not break the network**.
- **New devices can be added easily**.
- Mesh provides better traffic distribution through multiple possible paths.

## 3. Key characteristics
- **Decentralized architecture**
- **Efficient data routing**
- **Reliability** through multiple potential paths
- **Self-forming and self-healing** behavior
- **Scalability** when new nodes are added
- **Flexibility** for IoT and environments where traditional structures are impractical

## 4. Types of mesh

### Centralised mesh
- A **central node** assigns roles and establishes topology/routes.
- Saves node resources because end nodes do not need to discover adjacencies themselves.
- The central node can detect failures and reassign routes.
- Example: **Zigbee** with a **Network Coordinator**.

### Distributed mesh
- Nodes communicate with each other to determine **adjacency and routes**.
- Can use **reactive or proactive** routing for multi-hop networking.
- Nodes detect failures and establish new routes.
- Example: **AODV**.

### Hybrid mesh
- A central node helps establish **topology**.
- Nodes maintain **routes and adjacencies autonomously**.
- Failure detection is done by nodes, which then re-establish routes.
- Example: **HWMP** with **RM-AODV** fallback behavior.

## 5. Routing requirements in mesh
A good routing algorithm should be:
- **Decentralized**
- **Self-organizable**
- **Self-healing**

It should also:
- adapt to **wireless bandwidth limitations**
- exploit **multi-hopping** for better load balancing
- consider **power consumption** for energy efficiency

## 6. Flooding-based mesh routing

### General idea
- Packets are sent widely so they can still reach the destination even if the best path is unavailable.
- Main downside: **redundant transmissions**, **overhead**, and **congestion**.

### Advantages of flooding
- Can help achieve useful **shortest-path dissemination** in some cases
- Can reduce some retransmission overhead in synchronized scenarios
- Can reduce time-to-deliver in some scenarios, helping power use

### Disadvantages of flooding
- High **resource utilization**
- **Security exposure**
- More **complex troubleshooting**
- Poor **scalability** as the network grows
- High **overhead**
- Higher **latency**

## 7. Types of flooding algorithms

### Simple Flooding
- All nodes act as **sender and receiver**.
- Uses **sequence numbers** to decide whether to forward.
- Pros: reliable, easy to scale by adding nodes.
- Cons: overhead, inefficiency, latency, **broadcast storms**, battery drain.

### Reduced Flooding
- Divides the network into **subnets/clusters**.
- Each subnet has a **subnet coordinator**.
- Uses **TTL** to limit hop distance.
- Pros: reduced congestion, better scalability, lower memory/processing needs.
- Cons: more complexity, possible coordinator failure issues, added latency.

### Adaptive Flooding
- Adjusts forwarding based on **network load**.
- High load → nodes **reduce forwarding**.
- Low load → nodes **increase forwarding**.
- Pros: scalability, lower overhead, better reliability under changing load.
- Cons: more complexity, higher resource requirements, possible extra latency.

### Selective Flooding
- Forwards to nodes **likely to have a route**.
- Floods to neighbours only when no route is known.
- Pros: lower overhead, better scalability.
- Cons: higher complexity, route chosen may not be optimal.

### Probabilistic Flooding
- Chooses to flood/forward using probability values.
- Factors can include:
  - **link quality**
  - **number of hops to destination**
  - **number of packets previously forwarded**
- Pros: lower overhead, better scalability.
- Cons: complexity and possible longer delivery time if thresholds are poorly chosen.

### Hybrid Flooding
- Based on **Zone Routing Protocol (ZRP)**.
- **IARP** = intra-zone proactive routing
- **IERP** = inter-zone reactive routing
- Pros: reduced overhead for longer routes, useful mix of strategies.
- Cons: overlapping routing zones can create issues.

## 8. Structured mesh routing

### Proactive routing (table-driven)
- Maintains routes **before** they are needed.
- Pros: **lower latency** for sending data.
- Cons: **higher overhead**.
- Examples:
  - **OLSR**
  - **DSDV**
  - **WRP**
  - **BATMAN**

### Reactive routing (on-demand)
- Discovers routes **only when needed**.
- Pros: **lower overhead**.
- Cons: **higher latency**.
- Examples:
  - **AODV**
  - **DSR**
  - **TORA**

### Hybrid routing
- Mixes proactive and reactive ideas.
- Examples:
  - **ZRP**
  - **HWMP**

## 9. OLSR
- Each node collects information about **direct neighbours**.
- This information is shared with **periodic broadcasts**.
- OLSR uses **Multipoint Relays (MPRs)** to reduce unnecessary flooding.
- Path choice can consider **hop count, bandwidth, and link speed**.

## 10. WRP
Each node keeps **four tables**:
1. **Distance table**
2. **Routing table**
3. **Link-cost table**
4. **Message retransmission list table**

Other points:
- Link changes are propagated with **update messages**.
- **Hello messages** check connectivity.
- It avoids **count-to-infinity** by checking predecessor consistency.

## 11. DSR
### Route discovery
- Check **route cache** first.
- If no route exists, broadcast a **route request**.
- Route reply can come from the destination or an intermediate node with an unexpired cached route.
- The reply contains the **sequence of hops taken**.

### Route maintenance
- Uses **route error packets** and **acknowledgements**.
- Bad hops are removed from cache, and affected routes are truncated.

## 12. AODV
### Route discovery
- If no valid route exists, send **RREQ**.
- Intermediate nodes record the neighbour they first received the RREQ from, creating a **reverse path**.
- Destination or valid intermediate node responds with **RREP**.

### Route maintenance
- Uses **destination sequence numbers** for loop-free and up-to-date routes.
- Each node maintains a **sequence number** and **broadcast ID**.
- Route errors invalidate route entries and trigger new path discovery.

## 13. AODV vs DSR
- **AODV**: hop-by-hop routing, single route per destination, lower storage, better for higher mobility.
- **DSR**: source routing, multiple routes per destination, higher storage, better for lower mobility / fewer topology changes.

## 14. Challenges and limitations
- **Timing and synchronization** issues can cause collisions and retransmissions.
- **Interference and reliability** issues can increase packet loss and reduce performance.
- **Bandwidth allocation** problems can cause congestion, latency, and degraded QoS.

## 15. Security threats
- **Unauthorized access / malicious nodes**
- **Blackhole attacks**
- **Wormhole attacks**
- **MITM / eavesdropping**
- **DoS and flooding attacks**
- **Jamming**

## 16. Security countermeasures from the lecture
- Use **authentication** before allowing devices to join.
- Use **periodic reauthentication** for already connected nodes.
- Use **encryption** to protect privacy.

### Examples from the slides
- **Bluetooth pairing**: challenge-response with Secret Link Key, E1 algorithm
- **Wi-Fi mesh**: key-based authentication
- **LoRa mesh**: AES128, herd key, cattle-specific key
- **Bluetooth mesh**:
  - **App Key** for app data encryption/decryption
  - **Network Key** to identify network membership
  - **Device Key** for provisioning/configuration

## 17. Key takeaways
- Mesh networks are **decentralized, self-healing, and scalable**.
- Flooding is simple but often **inefficient**.
- Structured routing improves efficiency:
  - **Proactive** = lower latency, higher overhead
  - **Reactive** = lower overhead, higher latency
- Security remains a major concern: authentication, reauthentication, and encryption matter.
- Future trends mentioned:
  - **5G + LoRaWAN integration**
  - **AI-driven self-healing**
  - **energy-efficient protocols**
