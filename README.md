# Quantum-Credit-Risk-Analysis-using-Iterative-Amplitude-Estimation
A complete end-to-end credit risk modeling system that integrates **classical machine learning** with **quantum amplitude estimation (IAE/QAE)** to estimate **Expected Loss (EL)** for credit portfolios.  
All code — including classical preprocessing, model training, quantum circuit construction, and IAE execution — is contained entirely in **one notebook/file**.

---

## 📘 Project Summary
Financial institutions rely heavily on Monte Carlo simulation to compute credit risk metrics such as:

- **Expected Loss (EL)**
- **Value-at-Risk (VaR)**
- **Conditional VaR (CVaR)**

However, Monte Carlo converges slowly (**O(1/ε²)**).  
Quantum Amplitude Estimation (QAE/IAE) provides a **quadratic speedup**, scaling as **O(1/ε)**.

This project demonstrates a **hybrid workflow**:
1. Use a classical ML model to compute **PD (Probability of Default)**.
2. Use classical LGD values from the dataset.
3. Build a **quantum circuit** that encodes both PD and LGD.
4. Use **Iterative Amplitude Estimation (IAE)** to compute the Expected Loss quantumly.
5. Compare classical EL vs quantum EL.

The final quantum model achieved **≈4% relative error**, validating the pipeline.

---

## 📂 Single-File Pipeline Overview
The entire workflow is implemented inside **one notebook/script** containing:

1. **Data preprocessing**  
   - Encoding, scaling, cleaning  
   - Train-test split  

2. **Classical ML model**  
   - Trains on credit data  
   - Predicts PD for each loan  
   - Computes classical Expected Loss (PD × LGD)

3. **Quantum Circuit Construction**
   - PDs encoded using **Ry rotations**
   - LGDs encoded using **WeightedAdder**
   - Loss mapped via **LinearAmplitudeFunction**
   - Qubits arranged as:
     - State qubits
     - Sum qubits
     - Carry qubits
     - Objective qubit
     - (Optional) Ancilla qubits

4. **Iterative Amplitude Estimation (IAE)**
   - Uses `qiskit_algorithms` and `SamplerV2`
   - Returns estimated amplitude → Expected Loss
   - Produces confidence interval

5. **Noise Model Integration**
   - Uses Aer Simulator with SamplerV2.
   - Runs IAE under Aer Simulator:
     ```python
     sampler = SamplerV2.from_backend(aer_sim, default_shots=2048, seed=123)
     ```

6. **Comparison & Metrics**
   - Classical EL vs Quantum EL
   - Relative and absolute error
   - 95% quantum confidence interval

---

## 🧠 Motivation
Credit risk analytics depend on massive simulations.  
Quantum Amplitude Estimation offers a **theoretical speedup** by reducing the number of samples required to estimate expectations.

This project explores whether:
- PD and LGD can be encoded efficiently into quantum states  
- Quantum methods can approximate Expected Loss  
- Simulated noisy hardware still produces stable results  

This serves as a practical, publishable demonstration of **quantum-enhanced financial modeling**.

---

## 🛠️ Installation

Install all required libraries:

```bash
pip install qiskit>=2.0.0
pip install qiskit-aer==0.13.3
pip install qiskit-algorithms
pip install qiskit-ibm-runtime
pip install qiskit-finance
pip install scikit-learn pandas numpy matplotlib
