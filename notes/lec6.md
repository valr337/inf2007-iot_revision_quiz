# Lecture 6 — AIoT, Edge & Cloud Computing

## 1. Big picture
This lecture explains how **AIoT systems** combine **AI + IoT**, and why this usually pushes computation closer to devices instead of relying entirely on distant cloud servers.

Core idea:
- **AIoT = AI + IoT**
- Devices become **intelligent, not just connected**
- This is why **edge computing** becomes important

The lecture frames the topic as **how edge, fog, and cloud work together in AIoT systems**.

## 2. Why edge computing is needed
The lecture motivates edge computing with scale:
- **41.6 billion connected IoT devices** in 2025
- **79.4 zettabytes of data** generated

This creates practical problems if everything is sent to the cloud:
- **Latency-sensitive tasks** cannot wait
- **Bandwidth** becomes a bottleneck
- Sending raw telemetry continuously costs more
- Mobile / industrial networks may be unreliable

### Main benefits of edge computing
According to the lecture, edge computing helps to:
- **Reduce bandwidth consumption**
- **Reduce latency**
- **Reduce storage requirements**
- Reduce dependence on always-available cloud connectivity

## 3. Latency in AIoT systems
The lecture separates latency into two broad groups.

### Network-side latency
- **Transmission delay**: time to push all packet bits onto the link
- **Queuing delay**: time waiting in router queues
- **Processing delay**: time for routers to process packet headers

### Application-side latency
- Running inference on an edge GPU
- Executing business logic
- Handling service requests
- Sensor fusion / analytics / aggregation

The lecture also notes that communications latency is not only about delay, but also:
- **Reliability** — whether packets are dropped
- **Jitter** — whether delay is consistent

## 4. Illegal parking detection example
The lecture uses an **illegal parking detection** pipeline to show why mapping and optimisation matter.

Main stages shown:
1. **Pre-processing**
2. **Background modelling & foreground detection**
3. **Vehicle localization & tracking**
4. **Classification**
5. **Evidence collection**

### Key observation
The **classification** stage is the bottleneck in the slide’s timing example.

Examples shown:
- Preprocessing: **3.3 ms on one A15**, **30 ms on one A7**
- Background + foreground: **6 ms on one A15**, **50 ms on one A7**
- Vehicle localization & tracking: **12 ms on one A15**, **80 ms on one A7**
- Classification: **500 ms on one A15**, **350 ms on two A15**
- Evidence collection: **20 ms on one A15**, **100 ms on one A7**

The pipeline target shown is about **15 FPS (~66 ms)**, so computation must be mapped carefully across heterogeneous hardware.

## 5. Edge hardware spectrum
The lecture shows several classes of edge hardware.

### SBC-centric devices
Examples shown:
- **Raspberry Pi 4**
- **Odroid N2+**

Raspberry Pi 4 slide highlights include:
- Quad-core Cortex-A72 CPU
- **2GB / 4GB / 8GB** LPDDR4 variants
- **2.4 GHz and 5.0 GHz IEEE 802.11ac** wireless
- **Bluetooth 5.0 BLE**
- **Gigabit Ethernet**
- **2 USB 3.0 + 2 USB 2.0**
- **40-pin GPIO**

### GPU-centric devices
The lecture compares multiple **NVIDIA Jetson** families, including:
- Jetson Nano
- TX2 series
- Xavier NX / AGX Xavier
- Orin family

Example from the updated slide:
- **Jetson AGX Orin 64GB** reaches **up to 275 TOPS**

### MiniPC-centric edge AI
The lecture also shows AMD-based MiniPC-style platforms, such as:
- **Ryzen AI Max+ 395**
- **Ryzen AI 300 series**
- **Ryzen 8040 / 8845HS / 7840HS / 7940HS**

This highlights that edge AI is no longer limited to tiny microcontrollers only.

## 6. Edge AI ecosystem
The lecture organizes the stack into models, frameworks, inference engines, hardware, and deployment tools.

### Model families shown
- **EfficientNet-Lite**
- **MobileNetV3 / V4**
- **EdgeNeXt**
- **YOLO family**
- **MobileViT / EdgeViT**
- **MobileSAM / FastSAM**

### Frameworks
- **PyTorch** — mainstream for training
- **TensorFlow 2.x / JAX**
- **ONNX** — important for compatibility

### Inference engines / runtime stack
- **ONNX Runtime**
- **TensorRT**
- **OpenVINO**
- **TFLite**
- **MIGraphX**

### CPU / GPU optimisation support
- CPU side: **OpenMP, NEON**
- NVIDIA GPU side: **cuDNN, cuBLAS, cuFFT**
- AMD GPU side: **MIOpen, rocBLAS, rocFFT, rocRAND**

### Hardware examples
- CPUs: **ARM Cortex-A/M**, **RISC-V**
- GPUs: **Jetson Orin**, **Mali-G710+**, **Imagination GPUs**
- NPUs/accelerators: **Apple Neural Engine**, **Hailo**, **Google TPU 2.0**
- SoCs: **Snapdragon AI**, **Dimensity 9000**, **RK3588**

### Deployment tools
- **DeepStreamEdge**
- **TPU Compiler**
- **TFLite Converter**
- **OpenVINO suite**

## 7. Real-world deployment problems
The lecture stresses that lab demos and field deployments are different.

### Practical problems highlighted
- **Waterproofing requirements**
- **Thermal challenges**
- Waterproofing and ventilation may conflict
- Inside-box temperature can exceed **65°C** during daytime without ventilation
- CPU temperature can exceed **85°C**

This matters because edge systems often run outdoors in hot, enclosed spaces.

## 8. Optimisation: two levels
The lecture compares two optimisation layers.

### Model-centric optimisation
Focus:
- Neural network architecture and parameters

Goal:
- Reduce model size, complexity, and computation

Examples:
- **Pruning**
- **Quantization**
- **Knowledge distillation**
- **NAS**

### System-level optimisation
Focus:
- Hardware, OS, memory, data movement

Goal:
- Maximize hardware utilization and overall efficiency

Examples:
- **I/O optimisation**
- **Memory management**
- **Scheduling**

## 9. Deployment optimisation workflow
The lecture presents a four-part loop:
1. **Profiling**
2. **Analysing**
3. **Scheduling**
4. **Optimisation**

### What each part tries to do
- **Profiling**: identify performance patterns and bottlenecks
- **Analysing**: understand what is limiting the system
- **Scheduling**: assign tasks across resources better
- **Optimisation**: improve hardware use, computation, and energy efficiency

## 10. Advanced profiling techniques
The lecture lists several profiling categories.

### Model performance analysis
Purpose:
- Layer-wise execution time
- Function / operation bottlenecks

Tools shown:
- **TensorBoard Profiler**
- **NVIDIA Nsight Systems**

### Memory analysis
Purpose:
- Detect cache misses and inefficient data movement

Tools shown:
- **Valgrind Cachegrind**
- **ARM Streamline**

### Power profiling
Purpose:
- Find energy-intensive operations

Tools shown:
- **ARM Energy Probe**
- **Intel Power Gadget**

### System-level profiling
Purpose:
- Cycles, instructions, cache misses, branch mispredictions

Tool shown:
- **perf**

### Network profiling
Purpose:
- Latency, bandwidth limits, packet loss

Tools shown:
- **tcpdump**
- **Wireshark**

## 11. Analysing bottlenecks
The lecture groups bottlenecks into three main areas.

### CPU/GPU utilization
Problem:
- Under-utilization or overloading of compute units

Approaches:
- **Load balancing between CPU and GPU**
- Use **heterogeneous computing**, e.g. NPUs

### Memory and I/O
Problem:
- Delays caused by memory access and transfers

Approaches:
- **Memory pooling**
- **Layer fusion**
- Better data access patterns
- Faster storage / buffering

### OS-level issues
Problem:
- Scheduling overhead, resource allocation, context switching

Approaches:
- **Real-time scheduling**
- Prioritise important ML tasks
- Use lightweight runtimes where possible

Examples shown:
- **Zephyr RTOS**
- **TensorFlow Lite Micro**

## 12. Dynamic and adaptive scheduling
### Dynamic task scheduling
Idea:
- Assign work to CPU / GPU / NPU based on available resources

Examples from the slide:
- **Work stealing**
- **Priority-based scheduling**
- **Preemptive scheduling**

### Adaptive model partitioning
Idea:
- Split the model across edge and cloud depending on current conditions such as latency

### Adaptive resource allocation
Idea:
- Give more CPU / memory to high-priority tasks during peak load
- Reduce resources for background tasks when appropriate

## 13. Advanced optimisation techniques
### Hardware acceleration
Examples:
- **Operator fusion**
- **Domain-specific accelerators**
- **Google Edge TPU**
- **NVIDIA TensorRT**

### Code optimisation
Examples:
- **Kernel optimisation**
- **Loop unrolling and tiling**
- **Vectorization (SIMD)**
- **LLVM / GCC**
- **XLA**

### Graph-level optimisation
Examples:
- **Common subexpression elimination**
- **Constant folding**
- **TensorFlow XLA / TVM**

## 14. Case study: smart camera object detection
The lecture describes a smart camera doing real-time object detection and alerting.

### Problem
- High latency when sending results to the cloud
- Limited bandwidth
- Intermittent connectivity

### Tools used to investigate
- **tcpdump**
- **Wireshark**

### Solutions suggested
- **Edge-cloud partitioning**: keep critical detection logic on the edge
- Send only **summarised alerts** to the cloud
- **Protocol optimisation**: switch from **HTTP to MQTT**

## 15. Case study: face recognition camera
This example explains adaptive model partitioning in simpler terms.

### Challenge
- Running complex face recognition fully on the edge causes:
  - High latency
  - Higher power consumption

### Solution
- Keep fast / critical work **local**
- Offload heavier recognition layers to the **cloud** when connection quality and load allow it

### Benefits listed
- **Fast response**
- **Saves energy**
- **Reduces cost**

## 16. IoT vs IIoT
The lecture gives several distinctions for **Industrial IoT (IIoT)**:
- Different **application domains**
- **Accuracy and precision** are top priorities
- **Cyber security** is critical
- **Reliability and robustness** matter strongly

This implies that industrial deployments are often less tolerant of mistakes than consumer IoT.

## 17. Smart factory examples
The lecture gives future factory examples such as:
- **Asset tagging and locating**
- **Supply chain management**
- **Just-in-time asset management**
- **Smart inventory management** using sensors on containers
- Automatic low-stock alerts and even re-ordering
- **RFID-based quality control** to tag defective products
- Automatic alarms when failure rate crosses a threshold

## 18. Cloud computing basics
The lecture cites the NIST-style definition:
- Convenient, **on-demand network access**
- Shared pool of configurable computing resources
- Rapid provisioning and release
- Minimal management effort / provider interaction

### Key characteristics listed
- **On-demand self-service**
- **Scalable**
- **Shared**
- **Ubiquitous access**
- **Rapid provisioning / release**
- **Minimal management**

### Cloud for IoT
The lecture shows cloud-for-IoT platforms such as:
- **AWS IoT**
- **ThingSpeak**
- **Microsoft Azure IoT Platform**
- **IBM Watson IoT**
- **ARTIK Cloud**

## 19. Barriers to cloud adoption
The lecture lists several adoption barriers.

### Security
Questions raised include:
- Is the data secure?
- How do we audit security?
- Is deleted data really erased?

### Compliance
- Privacy / security / risk-law concerns

### Interoperability
- Moving workloads between providers is **not easy**

### Service level management
- Billing accuracy
- Failure handling
- Capacity questions

### Tools
- Need automation for provisioning, monitoring, and management

## 20. Fog computing
Fog computing is presented as a layer between edge devices and large cloud data centers.

### Properties listed
- **Location aware and location sensitive**
- **Low latency**
- **Computing in micro clouds**
- **Computing in the edge / everywhere**
- **Geographically distributed**
- **Large scale**
- **Mobility**
- **Real-time**

## 21. Key exam-style takeaways
- **AIoT depends heavily on edge computing**
- Edge computing is mainly motivated by **latency, bandwidth, and reliability**
- Know the difference between **network latency** and **application processing latency**
- Understand the difference between **model-centric** and **system-level** optimisation
- Remember the **profiling → analysing → scheduling → optimisation** loop
- Be able to explain **edge-cloud partitioning**
- Know why **IIoT** emphasizes **accuracy, cyber security, reliability, and robustness**
- Understand cloud basics, cloud barriers, and the role of **fog computing**
