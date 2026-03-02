# Stock Exchange Prototype 📈

[![Build Status](https://github.com/Rakshiiii/Stock_Exchange_Prototype/actions/workflows/build.yml/badge.svg)](https://github.com/Rakshiiii/Stock_Exchange_Prototype/actions)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![C++ Standard](https://img.shields.io/badge/C%2B%2B-20-blue.svg)](https://en.wikipedia.org/wiki/C%2B%2B20)

A high-performance, multi-process stock exchange engine prototype designed for low-latency financial trading systems. This project demonstrates advanced system programming techniques including high-concurrency networking, custom Inter-Process Communication (IPC) via shared memory, and FIX protocol processing.

---

## 🏗️ System Architecture

The project is built on a decoupled, microservices-style architecture that prioritizes throughput and reliability:

1.  **Gateway Process**: 
    *   Acts as the entry point for clients.
    *   Uses **Linux Epoll** for high-concurrency TCP connection management.
    *   Parses **FIX (Financial Information eXchange)** protocol messages.
    *   Enqueues orders into a high-speed shared memory IPC.

2.  **Sequencer / Process2**:
    *   Consumes messages from the shared memory queue.
    *   Designed for deterministic order sequencing and processing.
    *   Operates in a separate process to ensure isolation and performance.

3.  **Shared Memory IPC**:
    *   A custom, low-latency communication layer.
    *   Features session-based UUIDs and **crash recovery** mechanisms.
    *   Optimized for lock-free or minimal-lock performance.

---

## 🚀 Key Features

-   **High-Performance Networking**: Asynchronous I/O using `epoll` for efficient handling of thousands of concurrent client connections.
-   **Low-Latency IPC**: Custom shared memory implementation for near-zero latency communication between the Gateway and Sequencer processes.
-   **Protocol Parsing**: Native support for the FIX protocol, the industry standard for financial trading.
-   **Multi-Threaded Scheduler**: A sophisticated task-based scheduler with dedicated worker threads for network I/O and message dispatching.
-   **Robustness & Reliability**: Built-in crash recovery for IPC sessions and graceful shutdown handlers.
-   **Modern C++20**: Leverages the latest language features for cleaner, safer, and more performant code.

---

## 🛠️ Technical Stack

-   **Language**: C++20
-   **Networking**: TCP/IP, Linux Epoll
-   **IPC**: Shared Memory (shm_open, mmap)
-   **Configuration**: XML (via TinyXML-2)
-   **Build System**: CMake
-   **Protocol**: FIX (Financial Information eXchange)

---

## 📁 Project Structure

```text
.
├── Gateway/              # Entry point for network connections and FIX parsing
│   ├── Network/          # Epoll-based TCP listener implementation
│   └── Scheduler/        # Gateway-specific task scheduling logic
├── common/               # Shared utilities used across processes
│   ├── ipc/              # Custom Shared Memory IPC implementation
│   ├── Logger/           # Custom logging framework
│   └── xml/              # XML configuration utilities
├── process2/             # Sequencer/Processing engine
├── scripts/              # Automation scripts for deps and testing
├── tests/                # Integration and unit tests
└── config.xml            # System-wide configuration
```

---

## ⚙️ Installation

Get the project running on your local machine with these simple steps:

# 1. Install dependencies (Requires Linux/macOS)
```bash
chmod +x scripts/install-deps.sh
./scripts/install-deps.sh 
```

# 2. Build the project

```bash
mkdir build && cd build
cmake ..
cmake --build .
```

# 3. Launch the exchange processes
```bash
./process1 9001 & ./process2 9002 &
```

*(Note: Ensure you are running on a Linux environment for full Epoll support.)*

---

## 🧪 Testing

The project includes a suite of tests to ensure system stability and performance:

```bash
cd build
ctest --output-on-failure
```

-   **IPC Crash Recovery**: Verifies that the shared memory session can recover from process crashes.
-   **Gateway Network Tests**: Stress tests the FIX parser and concurrent connection handling.

---

## 🌟 Future Improvements

-   [ ] **Matching Engine**: Implement a full limit order book (LOB) for trade execution.
-   [ ] **Lock-Free Queues**: Replace mutex-based queues with SPSC/MPMC lock-free implementations for further latency reduction.
-   [ ] **Zero-Copy Serialization**: Implement flatbuffers or custom binary protocols for ultra-fast serialization.
-   [ ] **Web Dashboard**: Real-time monitoring of exchange health and order flow.

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
