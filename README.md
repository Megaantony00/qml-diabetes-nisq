# ⚛️ Hamiltonian Quantum Machine Learning for Diabetes Diagnostics

> *Exploring the boundary between quantum physics and medical AI in the NISQ era.*

**Bachelor's Thesis** · University of Salerno · Department of Computer Science · A.Y. 2024/2025  
**Supervisor**: Prof. Christiancarmine Esposito  
**Author**: Antonio Del Vecchio

---

## 📌 Overview

This repository contains the full research work behind my Bachelor's thesis on **Quantum Machine Learning (QML)** applied to diabetes risk classification using the **Pima Indians Diabetes Dataset (PIDD)**.

The core thesis challenges the dominant "digital" gate-based QML paradigm in favor of a **pure analogic approach** based on **Hamiltonian simulation** — encoding clinical features directly into the physical dynamics of an Ising model, governed by the Schrödinger equation. The goal is not to claim immediate quantum supremacy, but to systematically explore where quantum architectures already show promise and where the current NISQ era still falls short.

---

## 🧠 Key Findings

| Result | Detail |
|---|---|
| 🏆 **Best quantum model** | Quantum Kernel SVM · ROC AUC = **0.730** (±0.025, 5-fold CV) |
| ⚡ **Most original finding** | **Noise-assisted learning**: HamVQC at t=0.1 improves from AUC 0.481 (barren plateau) to **0.768** under NISQ depolarizing noise |
| 📉 **Barren Plateau** | Empirically documented on VQC Prototype: flat training curve for 120 epochs, gradient ≈ 0 |
| 🛠️ **Error mitigation** | Zero-Noise Extrapolation (ZNE) recovers accuracy from 37.5% → 39.58% without hardware changes |
| ❌ **QEC limitation** | 14-qubit Repetition Code introduces more noise than it corrects under current NISQ gate error rates |
| 📊 **Classical baseline** | Logistic Regression · ROC AUC = **0.833** (reference ceiling) |

---

## 🔬 Models & Architectures

Three quantum architectures were designed, implemented and evaluated:

**1. Quantum Kernel SVM**  
Data mapped into an exponentially large Hilbert space via a quantum feature map. Classification performed by a classical SVM on the resulting kernel matrix. Most stable architecture in noise-free conditions.

**2. HamVQC (Hamiltonian Variational Quantum Classifier)**  
Clinical features injected directly into the coefficients of a Transverse-Field Ising Hamiltonian. Time evolution simulated via `qml.ApproxTimeEvolution` (Suzuki-Trotter decomposition). Key parameter: evolution time `t`, which controls entanglement depth and noise interaction.

$$\hat{H}(x) = \sum_{i} x_i Z_i + \alpha \sum_{i \lt j} x_i x_j Z_i Z_j$$

**3. VQC Prototype**  
Standard angle-encoding variational circuit. Used as a reference to empirically demonstrate the Barren Plateau phenomenon.

---

## 📊 Full Results Summary

| Model | Condition | ROC AUC | Notes |
|---|---|---|---|
| Logistic Regression | Classical baseline | 0.833 | Reference |
| Quantum Kernel SVM | No noise, 5-fold CV | 0.730 ± 0.025 | Most stable |
| HamVQC (t=0.5) | No noise, 5-fold CV | 0.549 ± 0.053 | |
| HamVQC (t=0.1) | No noise | 0.481 | Barren plateau |
| VQC Prototype | No noise, 5-fold CV | 0.585 ± 0.081 | High variance |
| **HamVQC (t=0.1)** | **NISQ noise** | **0.768** | **Best quantum result** |
| HamVQC (t=0.5) | NISQ noise | 0.661 | |
| HamVQC (t=1.0) | NISQ noise | 0.621 | |
| VQC Prototype | NISQ noise | ~0.51 | Flat, barren plateau |
| ZNE Mitigated | NISQ noise | 0.572 | +0.002 over noisy |
| Repetition Code | NISQ noise | 0.533 | Neutral/negative |

---

## 🛠️ Tech Stack

- **Language**: Python 3
- **QML Framework**: [PennyLane](https://pennylane.ai/) + `lightning.gpu` backend
- **Classical ML**: scikit-learn
- **Environment**: Google Colab → Jupyter on remote GPU server
- **Dataset**: [Pima Indians Diabetes Dataset](https://www.kaggle.com/datasets/uciml/pima-indians-diabetes-database)

---

## 📁 Repository Structure

```

📦 qml-diabetes-nisq/
├── 📄 .gitignore         <- Excludes temporary files and local simulation artifacts.
├── 📄 README.md          <- Project overview and summary of key research findings.
├── 📄 requirements.txt   <- Python dependencies (PennyLane, Scikit-Learn, etc.).
├── 📄 thesis.pdf         <- Full Bachelor's Thesis in Computer Science (Italian).
│
├── 📁 notebooks/ 	<- Core experimental Jupyter notebooks.
│ ├── 📄 01_qml_comparison_angle_ham_kernel_cv.ipynb               <- Comparison of 4, 6, and 8-qubit scaling across different embeddings.
│ ├── 📄 02_hamvqc_noisefree.ipynb                                 <- Analysis of the Hamiltonian VQC in ideal, noise-free conditions.
│ ├── 📄 03_vqc_prototype_noisefree.ipynb                          <- Evaluation of the standard VQC baseline without hardware noise.
│ ├── 📄 04_hamvqc_noisy_zne_repetition.ipynb                      <- Testing HamVQC under NISQ noise with ZNE mitigation and Repetition Code.
│ ├── 📄 05_hamvqc_noisy_zne_repetition_full_eval.ipynb            <- Final performance analysis and generalization assessment for HamVQC.
│ ├── 📄 06_vqc_prototype_noisy_zne_repetition.ipynb               <- Testing the VQC Prototype under realistic NISQ noise conditions.
│ └── 📄 07_vqc_prototype_noisy_zne_repetition_full_eval.ipynb     <- Final metrics and comparative evaluation for the VQC Prototype.
│
└── 📁 results/		<- Detailed simulation outputs and visualizations.
├── 📁 Result_4Qubits_CV                            <- Cross-validation results for 4-qubit scalability analysis.
├── 📁 Result_6Qubits_CV                            <- Cross-validation results for 6-qubit scalability analysis.
├── 📁 Result_8Qubits_CV                            <- Cross-validation results for 8-qubit scalability analysis.
├── 📁 Result_Ham_4Qubits                           <- Performance logs for 4-qubit noise-free Hamiltonian models.
├── 📁 Result_Ham_6Qubits                           <- Performance logs for 6-qubit noise-free Hamiltonian models.
├── 📁 Result_Ham_8Qubits                           <- Performance logs for 8-qubit noise-free Hamiltonian models.
├── 📁 Result_Ham_8Qubits_noisy                     <- Data documenting the noise-assisted learning effect (ROC AUC 0.768).
├── 📁 Result_Ham_8Qubits_noisy_mitigated_re...     <- Evaluation of ZNE mitigation and Repetition Code on the HamVQC model.
├── 📁 Result_Prototype_4Qubits                     <- Experimental evidence of Barren Plateaus in 4-qubit standard VQCs.
├── 📁 Result_Prototype_6Qubits                     <- Experimental evidence of Barren Plateaus in 6-qubit standard VQCs.
├── 📁 Result_Prototype_8Qubits                     <- Experimental evidence of Barren Plateaus in 8-qubit standard VQCs.
├── 📁 Result_Prototype_8Qubits_noisy               <- VQC Prototype performance degradation under depolarizing noise.
└── 📁 Result_Prototype_8Qubits_noisy_mitigate...   <- Impact of ZNE and error correction strategies on the baseline model.
```

---

## 🚀 Getting Started

### 📓 Notebooks
Explore the research experiments directly on Google Colab:

- **01 Comparison**:
- [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Megaantony00/qml-diabetes-nisq/blob/main/notebooks/01_qml_comparison_angle_ham_kernel_cv.ipynb)
- **02 HamVQC Noise-free**:
- [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Megaantony00/qml-diabetes-nisq/blob/main/notebooks/02_hamvqc_noisefree.ipynb)
- **03 VQC Prototype Noise-free**:
- [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Megaantony00/qml-diabetes-nisq/blob/main/notebooks/03_vqc_prototype_noisefree.ipynb)
- **04 HamVQC Noisy/ZNE**:
- [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Megaantony00/qml-diabetes-nisq/blob/main/notebooks/04_hamvqc_noisy_zne_repetition.ipynb)
- **05 HamVQC Full Evaluation**:
- [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Megaantony00/qml-diabetes-nisq/blob/main/notebooks/05_hamvqc_noisy_zne_repetition_full_eval.ipynb)
- **06 VQC Prototype Noisy/ZNE**:
- [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Megaantony00/qml-diabetes-nisq/blob/main/notebooks/06_vqc_prototype_noisy_zne_repetition.ipynb)
- **07 VQC Prototype Full Evaluation**:
- [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Megaantony00/qml-diabetes-nisq/blob/main/notebooks/07_vqc_prototype_noisy_zne_repetition_full_eval.ipynb)

### ☁️ Run on Google Colab
1. **Open** any notebook via the badges above.
2. **Enable GPU**: Go to `Runtime > Change runtime type` and select **T4 GPU** (required for Hamiltonian simulations).
3. **Setup**: Execute the first cell in the notebook to automatically install all required libraries.
4. **Kaggle Auth**: You might need to provide your Kaggle credentials when `kagglehub` prompts for them to download the PIMA dataset.

### 💻 Run on Local or Remote Server
Recommended for long training sessions or high-qubit simulations.

```bash
# Clone the repository
git clone https://github.com/Megaantony00/qml-diabetes-nisq.git
cd qml-diabetes-nisq

# Install dependencies
pip install -r requirements.txt

# Launch Jupyter
jupyter notebook
```

---

## 💡 The Noise-Assisted Learning Effect

The most unexpected finding of this work: under a **depolarizing noise model** (realistic NISQ simulation), the HamVQC at `t=0.1` went from **AUC 0.481** (stuck in a barren plateau in ideal conditions) to **AUC 0.768** — the best quantum result of the entire study.

The hypothesis is that depolarizing noise acts as an **implicit stochastic regularizer**, breaking the symmetry in the optimization landscape and allowing the algorithm to escape the plateau. In other words, in certain architectural configurations, **NISQ hardware noise can be a computational resource rather than just a source of degradation**.

---

## 📖 Abstract

This thesis explores the intersection of Quantum Machine Learning (QML) and medical diagnostics, focusing on diabetes risk classification via the Pima Indians Diabetes Dataset. The central goal is to move beyond the "digital" gate-based quantum paradigm — often limited in the NISQ era by Trotterization errors and decoherence — toward a "pure QML" approach based on analogic Hamiltonian simulation.

Clinical features are not simply encoded as static rotation angles, but injected into the control parameters of a Transverse-Field Ising model, allowing the natural dynamics governed by the Schrödinger equation to map data into a high-dimensional Hilbert space. Validation goes beyond standard metrics (AUC-ROC, F1-score) and includes advanced quantum indicators such as Kernel Target Alignment (KTA), effective dimension, and the Meyer-Wallach entanglement measure.

---

## 📚 Citation

If you find this work useful, please cite:

```bibtex
@thesis{delvecchio2025qml,
  author    = {Del Vecchio, Antonio},
  title     = {Quantum Machine Learning Hamiltoniano per la Diagnostica del Diabete},
  school    = {Università degli Studi di Salerno},
  year      = {2025},
  type      = {Bachelor's Thesis}
}
```

---

## 📬 Contact

**Antonio Del Vecchio**  
📧 antoniodv29@gmail.com  
🔗 [LinkedIn](https://linkedin.com/in/antonio-del-vecchio) · [GitHub](https://github.com/Megaantony00)
