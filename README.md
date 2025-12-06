# Hardware-Software Co-Design for LLM Quantization

**CECS 528: Advanced Computer Architecture - Term Project**  
**California State University, Long Beach**  
**Author: Gyanendra Pandey**

## Research Question

> Can hardware-software co-design for quantization outperform software-only approaches in LLM inference?

## Key Findings

| Approach | Performance | Energy Efficiency | Accuracy Loss |
|----------|-------------|-------------------|---------------|
| Software (bitsandbytes) | 0.65× (slower) | Baseline | <1% |
| Software (ONNX Runtime) | 1.7× faster | Baseline | <1% |
| **Oltron (ASIC)** | **1.9× faster** | **1.6× better** | **<1%** |
| **FlightLLM (FPGA)** | **1.2× vs A100** | **6× better** | **<1%** |

## Key Numbers

- **LLaMA-70B**: 140 GB memory requirement
- **NVIDIA A100**: 80 GB memory, $15,000, 300W power
- **Xilinx U280 FPGA**: 8 GB HBM, $5,000, 75W power
- **ChatGPT inference**: ~$700K/day operational cost

## Papers Analyzed

1. **Oltron** (DAC 2024) - ASIC with adaptive quantization and reconfigurable processing elements
2. **FlightLLM** (FPGA 2024) - FPGA with configurable sparse DSP and always-on-chip KV cache

## Repository Structure

```
├── README.md
├── docs/
│   ├── LLM_Quantization_Paper.docx    # 2-page research paper
│   └── Presenters_Guide.pdf            # Speaker notes
├── presentation/
│   ├── LLM_Quantization_Complete.pptx  # 19-slide presentation
│   └── LLM_Quantization_Complete.pdf   # PDF version
└── resources/
    ├── Potential_Questions.pdf         # Q&A preparation (31 questions)
    ├── LLM_Quantization_Quiz.docx      # MCQ quiz (5 questions)
    └── LLM_Quantization_Quiz_Answers.docx  # Answer key
```

## Presentation Outline (19 Slides)

1. Title Slide
2. The Problem: LLMs Are Huge
3. Why This Matters
4. What is Quantization?
5. Research Question
6. Software Quantization Tools
7. Software Quantization Results
8. The Software Ceiling
9. Hardware Solution #1: Oltron
10. Oltron: Technical Deep Dive
11. Oltron: Results
12. Hardware Solution #2: FlightLLM
13. FlightLLM: Technical Deep Dive
14. FlightLLM: Results
15. Head-to-Head Comparison
16. Key Takeaways
17. Course Connections
18. Conclusion
19. References

## Course Connections

- **Memory Hierarchy**: HBM vs SRAM tradeoffs, KV cache placement
- **Dataflow Architectures**: Reconfigurable processing elements
- **Amdahl's Law**: Memory bandwidth as the bottleneck
- **Energy Efficiency**: Performance-per-watt as key metric

## References

1. NVIDIA. (2024). A100 Tensor Core GPU Architecture
2. Oltron: An Adaptive Accelerator for LLM Quantization (DAC 2024)
3. FlightLLM: Efficient Large Language Model Inference with a Complete Mapping Flow on FPGAs (FPGA 2024)
4. CipherCore Benchmarks: bitsandbytes vs ONNX Runtime Performance Analysis
