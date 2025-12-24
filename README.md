# 📊 SSA Denoising Pipeline

*Paper Title*: **Denoising Stock Index Time Series via Truncated Singular Value Decomposition on Hankel Matrices**

*Description*: Implementation of **Singular Spectrum Analysis (SSA)** with integrated mean-centering for denoising financial time series. Paper for IF2123 Linear Algebra and Geometry - ITB (2025).

## 📌 Overview

This project implements a **complete SSA pipeline** for removing noise from financial time series using **Truncated SVD (TSVD)** and **Hankel matrix embedding**.

**Key Results (IHSG 2024):**
- 🎯 **86.3% MSE reduction** vs Simple Moving Average
- 🎯 **12.3x smoothness improvement** vs original noisy data
- 🎯 **R² = 0.1005** (non-linearity preserved)
- 🎯 **5 dominant components** with total 91.03% cumulative energy

**Theoretical Guarantee:** Eckart-Young-Mirsky theorem ensures optimal low-rank approximation in Frobenius norm.


## 🚀 Setup

### 1️⃣ Create Virtual Environment

**Windows:**
```bash
python -m venv venv
.\venv\Scripts\activate
```

**macOS / Linux:**
```bash
python3 -m venv venv
source venv/bin/activate
```

### 2️⃣ Install Dependencies

```bash
pip install --upgrade pip
pip install numpy scipy pandas matplotlib yfinance jupyter
```

Or use requirements.txt:
```bash
pip install -r requirements.txt
```

### 3️⃣ Run Notebook

```bash
jupyter notebook SSA_Pipeline.ipynb
```


## 🔬 SSA Pipeline Overview

### 6-Step Algorithm

```
Step 0: Mean-Centering
├─ Compute ȳ = (1/N) Σ yₜ
└─ Center: ỹₜ = yₜ - ȳ

Step 1: Embedding
├─ Create Hankel matrix X ∈ ℝ^(L×K)
└─ L = N/2 (window length), K = N - L + 1

Step 2: Decomposition
├─ Compute SVD: X = UΣVᵀ
└─ Extract singular values σ₁ ≥ σ₂ ≥ ... ≥ σᵣ

Step 3: Rank Selection
├─ Cumulative energy: Eₖ = Σ(σᵢ²) / Σ(σⱼ²)
└─ Select k = argmin{Eₖ ≥ τ}, τ = 0.90

Step 4: Reconstruction
├─ Form rank-k approximation: Xₖ = Σ(i=1 to k) σᵢ uᵢ vᵢᵀ
└─ Optimal in Frobenius norm (Eckart-Young theorem)

Step 5: Diagonal Averaging
├─ Project Xₖ back to Hankel matrix structure
└─ Recover univariate series: sₜ = (1/|Dₚ|) Σ Xₖ[i,j]

Step 6: De-centering
└─ Restore mean: ŝₜ = sₜ + ȳ
```

### Why This Works

**Signal vs Noise Separation:**
- **High-correlation signal** → concentrates in few large σᵢ values
- **Low-correlation noise** → spreads across many small σᵢ values
- **Truncation at rank k** → retains signal, discards noise

**Mean-Centering Advantage:**
- Without it: σ₁ captures DC offset (99.97%) → unrealistic k=1 selection
- With it: σ₁ captures oscillations → realistic k=5 selection → preserves non-linearity

**Optimality:**
The rank-k approximation Xₖ minimizes the Frobenius norm error over all rank-k matrices:

∥X - Xₖ∥_F = min_{rank(B)≤k} ∥X - B∥_F = √(Σ_{i=k+1}^r σᵢ²)

This guarantees mathematically optimal denoising with theoretical justification via the Eckart-Young-Mirsky theorem.


## 📁 Project Files

```
├── SSA_Pipeline.ipynb      ← Main implementation notebook
├── requirements.txt        ← Python package dependencies
└── README.md               ← This file
```


## 🎓 Educational Value

**Core Linear Algebra Concepts:**
- **Spectral Theorem** - Eigenvalue decomposition of symmetric matrices
- **Singular Value Decomposition (SVD)** - Matrix factorization into U, Σ, Vᵀ
- **Eckart-Young-Mirsky Theorem** - Optimal low-rank matrix approximation
- **Frobenius Norm** - Matrix error measurement and minimization
- **Hankel Matrix Structure** - Time-delay embedding for temporal correlation
- **Orthogonal Transformations** - Preservation of geometry through matrix operations

**Application Domain:**
- Financial time series denoising
- Signal processing with matrix decomposition
- Practical demonstration of linear algebra theory


## 📄 License

This implementation is provided for educational purposes as part of **IF2123 Aljabar Linier dan Geometri - ITB**.

**Author:** Samuelson Dharmawan Tanuraharja (13524001)