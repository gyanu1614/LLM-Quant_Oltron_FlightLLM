# LLM Quantization: Technical Presentation Guide

**CECS 530: Advanced Computer Architecture - Term Fall 2025**
**California State University, Long Beach**

---

## Document Purpose

This guide accompanies the LLM Quantization presentation, providing technical context for hardware-software co-design approaches in large language model inference optimization.

---

## 1. Problem Definition (Slides 1-4)

### Scale Constraints
LLaMA-70B requires 140 GB memory for inference. NVIDIA A100 provides 80 GB capacity at $15,000 and 300W power consumption. Current LLM deployment costs approach $700K/day for services like ChatGPT.

### Quantization Overview
Precision reduction (FP32 → INT8) decreases memory and computation requirements. Target: maintain <1% accuracy loss while achieving 4× compression.

---

## 2. Software Quantization (Slides 5-8)

### Evaluated Tools

**bitsandbytes**: GPU-based software quantization library
- Performance: 0.65× baseline (65% slower)
- Accuracy: <1% degradation
- Constraint: No hardware-level optimization

**ONNX Runtime**: Cross-platform inference framework
- Performance: 1.7× baseline speedup
- Accuracy: <1% degradation
- Constraint: Limited by GPU architecture

### Performance Limitations
Software approaches encounter fundamental bottlenecks: memory bandwidth saturation, fixed GPU dataflow, inability to exploit custom architectures, and energy efficiency limits at architectural boundaries.

---

## 3. Oltron ASIC Architecture (Slides 9-11)

### Design Specifications
Application-Specific Integrated Circuit targeting adaptive LLM quantization with reconfigurable processing elements for quantized matrix operations.

### Technical Components
- **Adaptive Quantization Engine**: Per-layer precision adjustment
- **Reconfigurable PE Array**: Model-specific dataflow optimization
- **Custom Memory Hierarchy**: Minimized data movement overhead
- **On-Chip Weight Cache**: Reduced external bandwidth dependency

### Results
- Throughput: 1.9× vs NVIDIA A100
- Energy: 1.6× performance-per-watt improvement
- Accuracy: <1% FP32 baseline degradation
- Trade-off: ASIC inflexibility for specialized performance

### Architecture Principle
Demonstrates Amdahl's Law: targeting memory bandwidth bottleneck yields disproportionate overall speedup.

---

## 4. FlightLLM FPGA Architecture (Slides 12-14)

### Design Specifications
Xilinx U280 FPGA implementation with 8 GB HBM, $5,000 cost. Complete mapping flow for LLM inference leveraging reconfigurable logic.

### Technical Components
- **Configurable Sparse DSP**: Exploits model-specific sparsity patterns
- **On-Chip KV Cache**: Eliminates external memory for attention mechanism
- **Dynamic Precision Scaling**: Runtime bit-width adaptation
- **Custom Transformer Dataflow**: Optimized attention computation pipeline

### Results
- Throughput: 1.2× vs NVIDIA A100
- Energy: 6× performance-per-watt (75W vs 300W)
- Accuracy: <1% FP32 baseline degradation
- Cost: $5,000 (vs $15,000 A100)
- Trade-off: FPGA reconfigurability for ASIC peak throughput

### Architecture Principle
Demonstrates dataflow optimization and memory hierarchy design: HBM-SRAM-DRAM trade-offs for bandwidth-bound workloads.

---

## 5. Comparative Analysis (Slides 15-16)

| Metric | Software (ONNX) | Oltron (ASIC) | FlightLLM (FPGA) |
|--------|-----------------|---------------|------------------|
| Performance Gain | 1.7× | 1.9× | 1.2× |
| Energy Efficiency | Baseline | 1.6× | 6× |
| Cost | GPU-dependent | High NRE | $5,000 |
| Flexibility | High | Low | Medium |
| Deployment | Immediate | Long cycle | Moderate |

### Analysis
ASIC delivers maximum throughput. FPGA provides optimal performance-per-watt and cost-effectiveness for medium-scale deployment. Software enables rapid iteration but encounters architectural limitations.

---

## 6. Architecture Concepts (Slides 17-19)

### Memory Hierarchy
HBM-SRAM-DRAM trade-offs in custom accelerators. On-chip KV cache placement reduces external bandwidth requirements. Memory bandwidth identified as primary performance limiter.

### Dataflow Design
Reconfigurable processing elements enable model-specific optimization. Spatial vs temporal compute allocation. Custom pipelines for transformer attention patterns.

### Amdahl's Law
Memory bandwidth bottleneck dominates system performance. Accelerating compute without addressing bandwidth yields diminishing returns.

### Energy-Performance
Performance-per-watt as deployment metric. Static power (ASIC leakage) vs dynamic power (FPGA reconfiguration overhead) trade-offs.

---

## 7. Conclusions

Hardware-software co-design achieves:
- Performance: 1.9× speedup (Oltron ASIC)
- Efficiency: 6× energy improvement (FlightLLM FPGA)
- Accuracy: <1% degradation across all approaches

### Deployment Selection
- High-volume, stable workload: ASIC (maximum throughput)
- Cost-sensitive, evolving models: FPGA (flexibility + efficiency)
- Rapid prototyping: Software (immediate deployment)

---

## Technical Reference

**ASIC**: Application-Specific Integrated Circuit
**FPGA**: Field-Programmable Gate Array
**HBM**: High Bandwidth Memory
**KV Cache**: Key-Value cache for transformer attention
**DSP**: Digital Signal Processor
**NRE**: Non-Recurring Engineering cost

---

## Citations

1. NVIDIA A100 Architecture Whitepaper
2. Oltron: An Adaptive Accelerator for LLM Quantization (DAC 2024)
3. FlightLLM: Efficient Large Language Model Inference with a Complete Mapping Flow on FPGAs (FPGA 2024)
4. Online sources and class content
