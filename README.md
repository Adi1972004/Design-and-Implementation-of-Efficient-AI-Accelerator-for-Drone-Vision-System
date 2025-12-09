# 🚁 Efficient AI Accelerator for Drone Vision System
### Hardware–Software Co-Design for Monocular Depth Estimation & Path Planning

This project implements a **real-time embedded vision system** for drones, combining:

- Lightweight **Monocular Depth Estimation Model**
- Custom **FPGA-based Convolution Accelerator**
- Hardware **Path Planning FSM**
- **Quantized model deployment** workflow for STM32N6 Neural-ART Accelerator
- Full **bit-accurate Python verification** pipeline

The system enables **low-latency obstacle avoidance** and **efficient on-device inference** for UAVs and edge AI platforms.

---

## 📌 Overview

Drones require fast, power-efficient depth estimation for safe navigation.  
This project designs a **complete hardware–accelerated depth estimation + path planning system**, optimized for:

- FPGA deployment  
- Ultra-low-power microcontrollers (STM32N6)  
- Resource-constrained embedded environments  

---

## ✨ Features

### ✔ Monocular Depth Estimation
- MobileNetV3-Small encoder  
- ULWA lightweight decoder (Channel + Spatial Attention)  
- PixelShuffle upsampling  
- INT8-friendly architecture  
- Quantization-Aware Training (QAT) support  

### ✔ FPGA Convolution Accelerator
- Supports **1×1, 3×3, 5×5, 7×7** kernels  
- Unified **8×8 fixed-function MAC array**  
- 1 pixel/clock throughput  
- Q8.8 fixed-point arithmetic  
- Sliding-window line buffer design  
- Bit-accurate to Python model (<0.0022 mismatch)  

### ✔ Hardware Path Planning FSM
- 91-sector FOV segmentation  
- Median-based clearance estimation  
- Valley detection logic  
- 7-state FSM  
- Outputs yaw angle (Q1.15 fixed-point)  
- Emergency/no-path detection  

### ✔ STM32N6 NPU Integration
- Neural-ART Accelerator exploration  
- PTQ (INT8) & QAT for regression models  
- X-CUBE-AI memory mapping & operator analysis  
- External Flash Loader workflow  

---

## 🧱 System Architecture

Camera → Depth Model → FPGA Convolution Accelerator
→ Depth Map → Path Planning FSM → Yaw Angle


Supports both FPGA-only and STM32N6 hybrid deployments.

---

## 🔍 Depth Estimation Pipeline

- **Encoder:** MobileNetV3-Small  
- **Decoder:** ULWA module  
- **Upsampling:** PixelShuffle  
- Produces **224×224 full-resolution depth map**  
- Designed for fixed-point hardware execution  

---

## 🔧 FPGA Convolution Accelerator

### Highlights

| Property | Value |
|----------|-------|
| LUTs | 209 |
| Registers | 203 |
| Kernel Support | 1×1, 3×3, 5×5, 7×7 |
| Throughput | 1 px/clk |
| Output Precision | 32-bit accumulated |
| Mismatch vs Python | **0.00213%** |

### Architecture Summary
- 7×7 sliding window generator  
- Multi-stage pipelined MAC tree  
- Parameterized kernel controller  
- FSM for START → PROCESS → DONE  

---

## 🧭 Hardware Path Planning Module

The module converts depth maps into a safe yaw angle.

### 7-State FSM
1. IDLE  
2. INIT_SECTORS  
3. SCAN_IMAGE  
4. MARK_VALID  
5. FIND_VALLEYS  
6. SELECT_BEST  
7. COMPUTE_YAW  

### Outputs
- Best yaw angle (Q1.15)  
- Selected navigation sector  
- Forward clearance  
- Emergency flag  

Error vs Python reference: **<0.05%**.

---

## ⚡ STM32N6 NPU Deployment

### Explored Features
- Neural-ART Accelerator (600 GOPS INT8)  
- X-CUBE-AI operator optimization  
- INT8 PTQ challenges for depth regression  
- QAT improving accuracy  
- Flash Loader + UART output testing  
- Secure Boot & Application execution model  

---

## 📊 Results

### Convolution Accelerator
- Ultra-low resource footprint  
- Perfect timing closure at typical FPGA speeds  
- 1-cycle/px streaming pipeline  
- Dynamic power dominates (ideal for drones)

### Path Planning
- Correctly identifies narrow and wide safe valleys  
- Deterministic hardware latency  
- Robust yaw estimation  

### STM32N6
- QAT model closely matches FP32  
- NPU execution flow validated end-to-end  

---

## 🛠 Tools & Technologies

### Hardware
- Xilinx Zybo Z7 FPGA  
- STM32N657X0-Q Microcontroller  

### Software
- Verilog (Vivado)  
- PyTorch / ONNX  
- STM32CubeIDE / X-CUBE-AI  
- NumPy, Matplotlib, OpenCV  

---

## 📜 License
This project is released under the **MIT License**.
