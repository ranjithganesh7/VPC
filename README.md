# Hardware-Accelerated Video Processing Core on Xilinx PYNQ-Z2

A hardware-accelerated video processing system implemented on the **Xilinx PYNQ-Z2** platform using **Zynq-7000 programmable logic, HLS, AXI DMA, and PYNQ Python**.

The project evaluates the performance benefits of moving video-processing computation from a software-based pipeline to a dedicated hardware processing core implemented in FPGA programmable logic.

---

## 📌 Overview

Real-time video processing on embedded platforms requires efficient computation and data movement. This project implements a hardware-accelerated video processing pipeline on the **Xilinx PYNQ-Z2**, combining a Python-controlled software environment with an HLS-based processing core in programmable logic.

The system uses:

- **Zynq-7000 Processing System**
- **AXI Interconnect**
- **AXI DMA**
- **AXI4-Stream Data FIFO**
- **HLS-based video processing IP**
- **PYNQ Python**
- **Zero-copy memory buffers**
- **TCP socket communication**

The hardware and software implementations are evaluated using **1280 × 720** video frames based on average processing latency and frame rate.

---

## ✨ Key Features

- Hardware-accelerated video processing using FPGA programmable logic
- HLS-based custom processing IP
- AXI DMA for high-speed frame transfer
- AXI4-Stream communication between DMA and processing IP
- PYNQ Python control and hardware configuration
- Zero-copy input/output buffers
- TCP-based frame communication
- Software baseline for performance comparison
- FPGA resource utilization analysis
- Latency and FPS benchmarking

---

## 🏗️ System Architecture

```text
                    ┌──────────────────────┐
                    │   External Client    │
                    │   Video / Frames     │
                    └──────────┬───────────┘
                               │
                         TCP Socket
                               │
                               ▼
                    ┌──────────────────────┐
                    │  PYNQ Python App     │
                    │                      │
                    │ • Frame Reception    │
                    │ • Image Decoding     │
                    │ • Frame Preparation  │
                    │ • Hardware Control   │
                    │ • Output Handling    │
                    └──────────┬───────────┘
                               │
                     Zero-Copy Buffers
                               │
                               ▼
                    ┌──────────────────────┐
                    │       AXI DMA        │
                    └──────────┬───────────┘
                               │
                         AXI4-Stream
                               │
                               ▼
                    ┌──────────────────────┐
                    │   ISP HLS Processing │
                    │        Core          │
                    │                      │
                    │   isp_hls_top_0      │
                    └──────────┬───────────┘
                               │
                         AXI4-Stream
                               │
                               ▼
                    ┌──────────────────────┐
                    │      AXI DMA         │
                    │  Output Transfer     │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │  PYNQ Python App     │
                    │  Output Reconstruction│
                    └──────────┬───────────┘
                               │
                         TCP Socket
                               │
                               ▼
                    ┌──────────────────────┐
                    │   External Client    │
                    └──────────────────────┘
```

---

## 🔧 Hardware Design

The FPGA design is implemented using **Xilinx Vivado** and consists of the following major components:

| Component | Function |
|---|---|
| Zynq-7000 Processing System | Software execution and system control |
| AXI Interconnect | AXI communication between system components |
| AXI DMA | Transfers frame data between memory and programmable logic |
| AXI4-Stream Data FIFO | Streaming data buffering |
| `isp_hls_top_0` | HLS-based video processing core |
| DDR Memory | Frame-buffer storage |

The processing core is integrated into the programmable logic and controlled by the Processing System through the PYNQ Python environment.

---

## 💻 Software Pipeline

The software environment runs on the PYNQ-Z2 Processing System.

The pipeline performs:

1. Video/frame reception through TCP socket
2. Image decoding
3. Frame preparation
4. Hardware configuration
5. Transfer of frame data using AXI DMA
6. Processing through the HLS hardware core
7. Output reconstruction
8. Transmission of processed data

A separate software implementation is used as the baseline for performance comparison.

---

## 🧠 Hardware Acceleration Flow

```text
Input Frame
     │
     ▼
Image Decoding
     │
     ▼
Frame Preparation
     │
     ▼
Zero-Copy Input Buffer
     │
     ▼
AXI DMA
     │
     ▼
HLS Processing Core
     │
     ▼
AXI DMA
     │
     ▼
Zero-Copy Output Buffer
     │
     ▼
Output Reconstruction
     │
     ▼
TCP Transmission
```

---

## 🖥️ Platform

### Hardware

- **Board:** Xilinx PYNQ-Z2
- **SoC:** Zynq-7000
- **FPGA:** Programmable Logic + ARM Processing System
- **Resolution:** 1280 × 720

### Development Tools

- Xilinx Vivado 2023.1
- Vitis HLS / HLS-based IP development
- PYNQ
- Python

---

## 📂 Project Structure

```text
hardware-accelerated-video-processing-pynq-z2/
│
├── README.md
├── LICENSE
├── .gitignore
│
├── docs/
│   ├── paper/
│   │   └── Hardware-Accelerated-Video-Processing-Core.pdf
│   │
│   ├── architecture/
│   │   ├── system-architecture.png
│   │   └── data-flow.png
│   │
│   └── results/
│       ├── vivado-resource-utilization.png
│       └── performance-comparison.png
│
├── hardware/
│   ├── vivado/
│   │   ├── design_1/
│   │   └── README.md
│   │
│   ├── hls/
│   │   ├── isp_hls_top/
│   │   │   ├── src/
│   │   │   ├── include/
│   │   │   └── README.md
│   │   └── README.md
│   │
│   └── bitstream/
│       └── design_1_wrapper.bit
│
├── software/
│   ├── hardware_pipeline/
│   │   ├── server.py
│   │   └── README.md
│   │
│   ├── software_pipeline/
│   │   ├── server.py
│   │   └── README.md
│   │
│   └── common/
│       ├── config.py
│       └── utils.py
│
├── scripts/
│   ├── run_hardware.sh
│   ├── run_software.sh
│   └── benchmark.py
│
└── results/
    ├── hardware_results.csv
    ├── software_results.csv
    └── performance_summary.md
```

---

## ⚙️ Hardware Requirements

To reproduce the hardware implementation, the following are required:

- Xilinx PYNQ-Z2 board
- MicroSD card with PYNQ image
- USB/Ethernet connection
- Development computer
- Vivado 2023.1
- HLS development environment

---

## 📦 Software Requirements

The PYNQ environment requires Python and the corresponding PYNQ libraries.

The project uses:

```text
Python
PYNQ
NumPy
TCP/IP Socket
```

Additional dependencies can be added to the project as the software implementation is organized.

---

## 🚀 Running the Hardware Pipeline

After copying the project to the PYNQ-Z2:

### 1. Load the overlay

The generated FPGA bitstream is loaded using PYNQ.

```python
from pynq import Overlay

overlay = Overlay(
    "/home/xilinx/pynq/overlays/NiraGarage/ISP/design_1_wrapper.bit"
)
```

### 2. Initialize DMA and processing IP

```python
dma = overlay.axi_dma_0
isp_ip = overlay.isp_hls_top_0
```

The processing core is configured through its control registers.

Example configuration used in the experiment:

```python
isp_ip.write(0x10, 16)
```

### 3. Allocate frame buffers

The implementation uses PYNQ zero-copy buffers for input and output frame data.

The evaluated frame resolution is:

```text
1280 × 720
```

### 4. Start the processing pipeline

The input frame is transferred through AXI DMA to the HLS processing core. The processed frame is then transferred back to memory for output handling.

---

## 🧪 Software Baseline

A software-only implementation is included for comparison.

The software baseline executes the corresponding processing workload without the dedicated FPGA processing core.

The purpose of this baseline is to quantify the performance improvement achieved through hardware acceleration.

---

## 📊 Performance Results

The experiments were performed using **1280 × 720** frames.

| Metric | Hardware | Software |
|---|---:|---:|
| Resolution | 1280 × 720 | 1280 × 720 |
| Average Latency | **35.28 ms** | 230.63 ms |
| Average FPS | **28.34 FPS** | 4.34 FPS |
| Total Frames | 222 | 76 |

### Performance Improvement

The measured throughput ratio is:

\[
S_{\mathrm{FPS}} =
\frac{\mathrm{FPS}_{hardware}}
{\mathrm{FPS}_{software}}
\]

\[
S_{\mathrm{FPS}} =
\frac{28.34}{4.34}
\approx \mathbf{6.53\times}
\]

The latency reduction is:

\[
R_{\mathrm{latency}} =
\frac{L_{\mathrm{software}}-L_{\mathrm{hardware}}}
{L_{\mathrm{software}}}
\times100
\]

\[
R_{\mathrm{latency}}
=
\frac{230.63-35.28}{230.63}
\times100
\approx \mathbf{84.70\%}
\]

### Summary

> The hardware-accelerated implementation achieved approximately **6.53× higher processing throughput** and **84.70% lower average latency** than the software baseline under the evaluated configuration.

---

## 🧮 FPGA Resource Utilization

The implemented hardware design uses the following FPGA resources:

| Resource | Used | Utilization |
|---|---:|---:|
| Slice LUTs | 4,889 | 9.19% |
| Slice Registers | 6,675 | 6.27% |
| Block RAM Tiles | 4 | 2.86% |
| DSPs | 2 | 0.91% |
| BUFGCTRL | 1 | — |
| Bonded I/O | 130 | — |

The results indicate that the implemented processing architecture uses a relatively small portion of the available LUT, register, BRAM, and DSP resources on the PYNQ-Z2.

---

## 📈 Benchmark

The repository can be used to reproduce the comparison between:

```text
Software Processing
       vs.
FPGA Hardware Acceleration
```

The benchmark records:

- Total processed frames
- Average processing latency
- Average FPS
- Hardware/software throughput ratio
- Latency reduction
- FPGA resource utilization

---

## 🔬 Research Paper

The complete IEEE-style research paper is included in:

```text
docs/paper/
```

### Paper

**Hardware-Accelerated Video Processing Core on Xilinx PYNQ-Z2**

The paper describes the architecture, implementation methodology, experimental setup, performance evaluation, and FPGA resource utilization of the proposed system.

---

## 🎯 Project Objectives

The main objectives of this project are:

- Implement a video processing core using FPGA programmable logic.
- Integrate an HLS processing core with AXI-based infrastructure.
- Use AXI DMA for efficient frame-data movement.
- Control the hardware accelerator through PYNQ Python.
- Evaluate zero-copy buffer-based data transfer.
- Compare hardware acceleration against a software baseline.
- Measure processing latency and frame rate.
- Analyze FPGA resource utilization.

---

## 🔮 Future Work

Possible future improvements include:

- Pipeline optimization of the hardware processing core
- Evaluation at different video resolutions
- Further reduction of memory-transfer overhead
- Optimization of DMA and streaming data paths
- Integration of additional video-processing functions
- Evaluation of more complex real-time workloads

---

## 👥 Authors

**Navaneethan S**  
Department of Electronics and Communication Engineering  
Saveetha Engineering College, Chennai, Tamil Nadu, India

**Ranjith Ganesh B**  
Department of Electronics and Communication Engineering  
Saveetha Engineering College, Chennai, Tamil Nadu, India

**Royce Niran George**  
Department of Electronics and Communication Engineering  
Saveetha Engineering College, Chennai, Tamil Nadu, India

---

## 📚 Citation

If you use this project or build upon this work, please cite:

```text
Navaneethan S, Ranjith Ganesh B, and Royce Niran George,
"Hardware-Accelerated Video Processing Core on Xilinx PYNQ-Z2,"
2026.
```

---

## 📄 License

This project is intended for academic and research purposes.

See the `LICENSE` file for the applicable license terms.

---

## ⭐ Acknowledgment

This project was developed as an academic research project focusing on FPGA-based hardware acceleration, embedded video processing, and the integration of programmable logic with Python-controlled software environments.
