# ROS2 DDS Architecture Notes

A simple explanation of the ROS2 communication architecture.

---

# ROS2 Communication Stack

```text
+-------------------+
| Application layer |
|    Client code    |
+-------------------+
        ↓
+-------------------+
| ROS2 cllent layer |
|      rclcpp       |
| ROS2 C++ API      |
+-------------------+
        ↓
+-------------------+
|DDS interface layer|
|        RMW        |
| ROS Middleware    |
| DDS abstraction   |
+-------------------+
        ↓
+-------------------+
| DDS Implementation|
| FastDDS / Cyclone |
| Connext DDS       |
+-------------------+
        ↓
+-------------------+
|       RTPS        |
| Real-Time PubSub  |
+-------------------+
        ↓
+-------------------+
|        UDP        |
| Network transport |
+-------------------+
```

---

# 1. rclcpp

`rclcpp` is the ROS2 C++ API layer.

Example:

```cpp
publisher_->publish(msg);
```

```Python：
import math
math.sin(1)
```

Purpose:

```text
Your C++ code ↔ ROS2 system
```

Main responsibilities:

- publishers
- subscribers
- nodes
- timers
- executors

---

# 2. RMW (ROS Middleware)

RMW = middleware abstraction layer.

Purpose:

ROS2 can switch DDS implementations
without changing application code.

Examples:

```bash
rmw_fastrtps_cpp
rmw_cyclonedds_cpp
```

Same ROS2 code,
different DDS backend.

---

# 3. DDS Implementation

DDS = real communication engine.

Common DDS systems:

- Fast DDS
- Cyclone DDS
- Connext DDS

DDS handles:

- node discovery
- publish/subscribe
- QoS
- serialization
- real-time communication

This is the actual networking core.

---

# 4. RTPS

RTPS = Real-Time Publish Subscribe protocol.

Usually runs on:

```text
UDP
```

RTPS is the low-level protocol used by DDS.

---

# DDS Discovery

DDS automatically discovers nodes.

Example discovery messages:

```text
"Who publishes this topic?"
"Who subscribes this topic?"
```

Nodes connect automatically.

No ROS Master is required.

---

# QoS (Quality of Service)

QoS controls communication behavior.

Examples:

- Reliability
- Latency
- Queue depth
- Durability

---

# Reliable vs Best Effort

## Reliable

Guarantees delivery.

Good for:

- robot control
- commands
- critical data

Disadvantages:

- slower
- retransmission overhead

---

## Best Effort

Fast communication but packets may be lost.

Good for:

- camera
- lidar
- high-frequency sensors

---

# Why Cyclone DDS is Low Latency

Reasons:

- lightweight design
- efficient discovery
- lower overhead
- optimized RTPS behavior

Often preferred in robotics research.

---

# Why ROS2 is Good for Real-Time Systems

DDS supports:

- QoS
- UDP/RTPS
- low latency
- distributed systems
- deterministic communication

Applications:

- robotics
- autonomous vehicles
- industrial automation
- multi-robot systems

---

# One-Line Summary

```text
rclcpp = ROS2 coding API
RMW     = DDS switching layer
DDS     = communication engine
RTPS    = network protocol
UDP     = transport layer
```

---

# Common Interview Questions

- Why ROS2 does not need ROS Master?
- How DDS discovery works?
- What is QoS?
- Reliable vs Best Effort?
- Why Cyclone DDS has low latency?
- Why ROS2 is suitable for real-time systems?

These are all related to DDS implementation knowledge.
