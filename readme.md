# Comparative Performance Analysis of Workload on Enterprise GPUs with Consumer Platforms Accelerated by CUDA Graphs

This repository contains the experimental data and methodology used in the paper **"Comparative Performance Analysis of Workload on Enterprise GPUs with Consumer Platforms Accelerated by CUDA Graphs"**.

## 🔗 Source Code

The experiments in this study were conducted using the **NAS Parallel Benchmarks (NPB)** implementation with **CUDA Graphs** support.

**The source code used for these benchmarks is available at:**
👉 **[https://github.com/odiakun/NPB-GPU-CUDA-Graphs](https://github.com/odiakun/NPB-GPU-CUDA-Graphs)**

We gratefully acknowledge the work of Diakun et al. in providing the implementation that made this comparative analysis possible.

## 📄 Abstract

This work investigates the feasibility of reproducing benchmarks originally run on datacenter GPUs (NVIDIA A100, RTX 8000) using consumer-grade graphics cards (RTX 3050, GTX 1060). We evaluated selected NPB algorithms (BT, LU, SP, EP, IS, MG, and CG) across different problem classes using CUDA Graphs to quantify performance gains and overheads.

## 🛠️ Experimental Environment

To reproduce the results presented in our paper, the following environment and hardware configurations were utilized:

### Hardware
* **Consumer GPUs:**
    * NVIDIA GeForce **RTX 3050** (Ampere Architecture)
    * NVIDIA GeForce **GTX 1060** (Pascal Architecture, 3GB VRAM)
* **Enterprise GPUs (Baseline):**
    * NVIDIA **A100** (Ampere Architecture)
    * NVIDIA **RTX 8000** (Turing Architecture)

### Software Stack
* **Operating System:** Linux Ubuntu 22.04 LTS
* **CUDA Toolkit:** Version 13.0.2
* **Compiler:** GCC / NVCC

## 🧪 Methodology & Protocol

To ensure statistical reliability and minimize the influence of OS jitter and cold-start overheads, we strictly adhered to the following experimental protocol for every Algorithm/Class combination:

1.  **Total Executions:** 13 consecutive runs were performed for each configuration.
2.  **Warm-up Phase:** The first **3 executions** were discarded. This phase allows the GPU clock frequencies to stabilize (boost states) and instruction caches to be populated.
3.  **Data Collection:** The remaining **10 executions** were recorded.
4.  **Metric:** The final performance metric reported is the arithmetic mean of the 10 valid runs (Wall-clock time in seconds).

### Benchmarks Evaluated
* **BT, LU, SP:** Fluid dynamics and iterative solvers.
* **EP:** Embarrassingly Parallel.
* **IS:** Integer Sort (Memory bound).
* **MG, CG:** Multi-Grid and Conjugate Gradient.

## 📊 Summary of Results

The table below highlights the performance gap (execution time in seconds) for **Class C** benchmarks between consumer and enterprise hardware.

| Benchmark | GTX 1060 | RTX 3050 | A100 |
| :--- | :--- | :--- | :--- |
| **BT.C** | *Failed (VRAM)* | 58.59s | 7.60s |
| **SP.C** | 31.11s | 28.79s | 2.51s |
| **LU.C** | 52.05s | 43.46s | 3.34s |
| **IS.C** | *N/A* | 1.44s | 0.03s |

*Note: The GTX 1060 failed to execute BT.C and MG.C due to VRAM limitations (3GB).*

## 📝 Citation

If you reference this study or data, please cite our paper:

```bibtex
@inproceedings{retzlaff2025comparative,
  author    = {Retzlaff, Leandro L. and Pereira, Calebe C. and Veltri, Helena P. and Faganello, Murilo and Klein, William T. and Roque, Alexandre S.},
  title     = {Comparative Performance Analysis of Workload on Enterprise GPUs with Consumer Platforms Accelerated by CUDA Graphs},
  booktitle = {Proceedings of the [Conference Name]},
  year      = {2025},
  institution = {Universidade Regional Integrada do Alto Uruguai e das Missões (URI)}
}
