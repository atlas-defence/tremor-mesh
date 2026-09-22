# tremor-mesh

**tremor-mesh** is a decentralized, ultra-low latency seismic and acoustic ground-intrusion detection network. Designed to complement aerial and RF monitoring systems, it covers the physical ground approach by detecting and classifying the specific vibrational signatures of footsteps, vehicles, and heavy equipment.

## Core Architecture

Built with a strict focus on bare-metal performance and zero-dependency environments, `tremor-mesh` operates completely off-the-grid, ensuring a self-healing, air-gapped monitoring perimeter.

* **Native Edge Processing:** The core detection and signature-matching engine is written purely in Rust. It runs entirely on the edge, analyzing seismic anomalies locally to eliminate network latency and prevent bandwidth saturation.
* **Dark-Mesh Routing:** Sensor nodes communicate exclusively via highly efficient ZigBee relays. This creates an isolated, jam-resistant tactical mesh that does not rely on vulnerable Wi-Fi or cellular infrastructure.
* **Zero-Allocation Data Pipeline:** Memory is strictly managed with a custom arena allocator, ensuring continuous uptime on micro-computing hardware without garbage collection spikes.

## Hardware Requirements

To deploy a standard `tremor-mesh` cluster, each node requires:

* **Compute:** ESP32-S3 microcontroller
* **Network:** ZigBee relay module (e.g., CC2652 or native ESP32-H2 companion)
* **Sensors:** High-sensitivity geophones or analog piezo-electric vibration sensors
* **Power:** 18650 battery pack with PoE dongles for optional hardwired base stations

## Installation & Build

Ensure you have the Rust toolchain installed, alongside the `esp-rs` targets for building to the ESP32-S3.

```bash
# Clone the repository
git clone https://github.com/atlas-defence/tremor-mesh.git
cd tremor-mesh

# Build the firmware for the ESP32-S3 nodes
cargo build --target xtensa-esp32s3-none-elf --release

# Flash to the connected device
cargo espflash flash --release --monitor

```

## The Command Center (TUI)

The central monitoring dashboard rejects heavy web frameworks in favor of a highly optimized, retro-styled Terminal User Interface (TUI). This ensures the command center can run natively on low-power field laptops or direct serial connections with zero hydration overhead.

### Launching the Dashboard

```bash
cd cmd-center
cargo run --release -- --port /dev/ttyUSB0 --baud 115200

```

### TUI Features

* **Live Topography Grid:** Real-time ASCII visualization of activated nodes.
* **Signature Classification:** Instant readouts categorizing disturbances (e.g., `[INFANTRY]`, `[VEHICLE]`, `[WILDLIFE]`).
* **Node Health:** Continuous monitoring of battery levels and ZigBee link strength across the mesh.

## System Integration

`tremor-mesh` is designed to be highly modular. By default, the base station outputs a clean JSON stream over serial or local UDP, which can be piped directly into `openperimeter` or `osint-grid` for unified situational awareness.

```bash
# Example: Piping tremor events to openperimeter
./tremor-cli stream | openperimeter-agent ingest --source tremor-mesh

```

## Maintainers

* **Mehmet T. AKALIN** – *Lead Architect & Systems Programming*
