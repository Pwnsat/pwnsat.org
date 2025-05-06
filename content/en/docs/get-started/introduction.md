---
title: "Understanding the Flatsat"
description: "Understanding the Flatsat: A hardware-based training platform designed to be vulnerable"
lead: "A hardware-based training platform designed to be vulnerable"
date: 2022-08-27T08:48:57+00:00
lastmod: 2022-08-27T08:48:57+00:00
draft: false
menu:
  docs:
    parent: "get-started"
weight: 1
toc: true
---

{{< alert icon="🚧" context="warning" text="This site is currently under construction. We're working hard to bring you something awesome—stay tuned!" />}}

[Flatsat](https://flatsat.org/) is a hardware-based training platform designed to be vulnerable — on purpose. It’s built for hackers, engineers, and space enthusiasts who want to dive deep into space-grade systems, learn cybersecurity concepts, and prototype their own payloads — all at an accessible, low cost to encourage learning and experimentation.

# Hardware Design

## The idea
![](/images/ft_components.png)

- Central Processor: 
	 - **ESP32-S3** is used as the central processor handling both TT&C (Telemetry, Tracking & Command) and AD&C (Attitude Determination & Control) functions due to its powerful dual-core architecture and compatibility with embedded development.
- Communications: 
	 - **SX1262** LoRa transceivers are used for both uplink and downlink — connected via **SPI** — offering low-power, long-range wireless communication ideal for simulating satellite radio links.
- Sensors:
	 - **MPU6050** provides inertial data (gyroscope and accelerometer) crucial for attitude control.
	 - **BME280** supplies environmental readings (temperature, pressure, and humidity)
	 - Both sensors communicate via **I2C** for simplicity and efficiency in data acquisition.

## Expansion and Modularity

Recognizing the complexity of modern aerospace systems, Flatsat includes **expansion headers** for modular growth. This design supports stacking additional boards (e.g., cameras or advanced sensors) following the **CubeSat architecture**, enabling development of more sophisticated mission payloads and subsystems. This modular approach promotes flexibility, rapid prototyping, and realistic system scalability.

![](/images/ft_rear.png)

# Flatsat Architecture

![](/images/ft_phy.png)

## CCSDS Standards implementation
Flatsat incorporates elements of the **CCSDS (Consultative Committee for Space Data Systems)** standards to ensure educational alignment with real-world protocols. While **LoRa** is not a CCSDS-approved modulation, it is used here as a practical layer due to hardware constraints, allowing us to simulate protocol behavior within a 128-byte transmission limit.

### Implemented Standards:
1. **CCSDS 133.0-B-2** – Space Packet Protocol
2. **CCSDS 232.0-B-4** – TC Space Data Link Protocol
3. **CCSDS 132.0-B-3** – TM Space Data Link Protocol

This enables payload encapsulation and parsing to reflect actual satellite data handling and link-layer behavior, offering users insight into how data is structured, validated, and processed in space systems.
## AX.25 Integration
**AX.25** is a well-known amateur radio protocol used extensively in satellite missions and by the amateur radio community. It’s compatible with a wide range of **ground station software** and **SDR (Software-Defined Radio)** tools.

Flatsat uses **AFSK 1200 baud modulation** to transmit AX.25 frames via the SX1262, enabling users to decode telemetry with low-cost SDRs—fostering hands-on experience in signal demodulation and protocol analysis.

## Firmware

![](/images/ft_firmware.png)

### Dual-Radio System
The system features **dual transceivers** to support flexible mission configurations. One radio can be set in **semi-full-duplex** mode using LoRa for both uplink and downlink commands, while the second handles **downlink-only AFSK/AX.25** telemetry. This separation mirrors real satellite architectures, ensuring reliable and structured communication flow.
### uC&DH: Micro Command & Data Handling
To manage interactions between subsystems, Flatsat implements a **micro Command and Data Handling (uC&DH)** module. This unit coordinates commands received from the ground station and routes telemetry from various subsystems to the communication stack—serving as a central bridge in the software-defined architecture.

## Software system
![](/images/ft_cfs.png)

Inspired by NASA's **core Flight System (cFS)**, Flatsat implements a lightweight version of the cFS architecture. This includes:

- **Serial UART** interface for communication with subsystems
- **Subprocess logic** for command parsing, file transfers, and data storage
- **Database logging** of commands and telemetry
- **Limited shell access** for ground station interaction

This modular design mimics the structure and logic of professional mission software systems while remaining accessible for training and experimentation.

# Ground Station
The ground segment uses the **SimpleC3 software**, offering an intuitive interface for users to:

- Send commands    
- View telemetry through graphical dashboards
- Log and replay data
- Simulate satellite-ground interactions

It supports operation with a **LoRa USB stick** or **SDR with Tx/Rx capabilities**, adapting to various levels of hardware availability.