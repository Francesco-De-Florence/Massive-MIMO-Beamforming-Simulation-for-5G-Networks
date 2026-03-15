# Massive MIMO Beamforming Simulation for 5G Networks

> A complete Python simulation of a **Massive MIMO** wireless communication system — modelling large antenna arrays, multi-user beamforming, and spatial multiplexing for 5G/6G research.

---

## 🎯 Overview

Massive MIMO (Multiple-Input Multiple-Output) is a foundational technology in 5G and next-generation wireless networks. By deploying hundreds of antennas at the base station, it achieves dramatic gains in spectral efficiency, link reliability, and spatial reuse.

This project simulates the full signal chain — from channel modelling through precoder design to performance evaluation — using only standard Python scientific libraries.

---

## 📐 System Architecture

```
┌─────────────────────────────────────────────────────────┐
│                   Base Station (BS)                     │
│           M = 4 … 256 antennas (ULA)                    │
└────────────────────┬────────────────────────────────────┘
                     │  Precoded signal  W · s
                     ▼
        ┌────────────────────────┐
        │      Channel H(M×K)    │
        │  Rayleigh / Rician /   │
        │  Spatially Correlated  │
        └────────────────────────┘
                     │
        ┌────────────▼───────────┐
        │   K = 8 User Devices   │
        │   SINR → BER → Rate    │
        └────────────────────────┘
```

---

## ✨ Features

- **Three channel models** — i.i.d. Rayleigh, Rician (LoS + scatter), and spatially correlated (3GPP-style Jakes/Clarke)
- **Three beamforming algorithms**
  - MRT (Maximum Ratio Transmission) — low complexity, maximises per-user SNR
  - ZF (Zero-Forcing) — eliminates inter-user interference, requires M > K
  - MMSE (Regularised ZF) — optimal linear precoder, balances interference and noise
- **Spectral capacity analysis** — ergodic sum-rate via Monte Carlo, Shannon upper bound
- **BER performance** — QPSK modulation, full SNR sweep, theoretical baselines
- **Spatial multiplexing** — capacity vs. user count, per-user SINR CDF
- **Channel hardening demo** — variance of ‖h‖² → 0 as M → ∞
- **SVD analysis** — spatial degrees of freedom, channel conditioning vs. M
- **Full dashboard** — 6-panel summary of all results

---

## 📊 Key Results

| Metric | MRT | ZF | MMSE |
|--------|-----|----|------|
| Capacity @ M=256, SNR=10dB | ~28 bits/s/Hz | ~35 bits/s/Hz | ~37 bits/s/Hz |
| BER @ SNR=10dB (M=64) | ~0.01 | ~0.003 | ~0.001 |
| Complexity | O(MK) | O(MK²+K³) | O(MK²+K³) |
| Null inter-user interference | ✗ | ✓ | ✓ |

> Capacity scales as **log(M)** — confirmed across all algorithms. MMSE achieves the best trade-off at all SNR levels.

---

## 🚀 Getting Started

### Run in Google Colab (recommended)

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/)

Upload `Massive_MIMO_Beamforming_5G_Simulation.ipynb` to Google Drive and open with Colab — no local setup required.

### Run locally

```bash
git clone https://github.com/francescodeFlorence/massive-mimo-simulation.git
cd massive-mimo-simulation
pip install numpy scipy matplotlib seaborn tqdm
jupyter notebook Massive_MIMO_Beamforming_5G_Simulation.ipynb
```

---

## 📂 Project Structure

```
massive-mimo-simulation/
│
├── Massive_MIMO_Beamforming_5G_Simulation.ipynb   # Main simulation notebook
│
├── outputs/                   # Generated figures (auto-saved on run)
│   ├── channel_characteristics.png
│   ├── beampatterns.png
│   ├── capacity_vs_antennas.png
│   ├── ber_performance.png
│   ├── spatial_multiplexing.png
│   ├── svd_analysis.png
│   └── dashboard.png
│
└── README.md
```

---

## ⚙️ Configuration

All parameters are centralised in the `MIMOConfig` class (Section 2 of the notebook):

```python
class MIMOConfig:
    M_values       = [4, 8, 16, 32, 64, 128, 256]  # Antenna array sweep
    M_default      = 64          # Default BS antennas
    K              = 8           # Simultaneous users
    channel_model  = 'rayleigh'  # 'rayleigh' | 'rician' | 'spatial'
    modulation     = 'QPSK'
    snr_dB_fixed   = 10          # SNR for capacity analysis
    N_real         = 500         # Monte Carlo realisations
```

---

## 📖 Notebook Structure

| Section | Content |
|---------|---------|
| 1 | Setup, imports, global config |
| 2 | `MIMOConfig` — all parameters in one place |
| 3 | Channel models + visualisation |
| 4 | Beamforming algorithms + beampattern plots |
| 5 | Capacity vs. antenna count (primary result) |
| 6 | BER vs. SNR curves |
| 7 | Spatial multiplexing + SINR CDF |
| 8 | Complete 6-panel dashboard |
| 9 | Quantitative results summary |
| 10 | Extended analysis: channel hardening, SVD, experiments |

---

## 🧪 Experiment Ideas

Change `cfg.channel_model` or other parameters in Section 2 and re-run:

```python
cfg.channel_model  = 'rician'   # Add LoS component
cfg.channel_model  = 'spatial'  # Enable antenna correlation
cfg.K              = 16         # Overloaded system (K close to M)
cfg.angular_spread = 5          # Narrower spatial spread
cfg.M_default      = 128        # Larger array
```

---

## 📚 References

1. T. L. Marzetta — *Noncooperative cellular wireless with unlimited numbers of base station antennas*, IEEE Trans. Wireless Comm., 2010
2. F. Rusek et al. — *Scaling Up MIMO*, IEEE Signal Process. Mag., 2013
3. E. Björnson, J. Hoydis, L. Sanguinetti — *Massive MIMO Networks*, 2017 — [massivemimobook.com](https://massivemimobook.com)
4. 3GPP TR 38.901 — Study on channel model for frequencies from 0.5 to 100 GHz
5. D. Tse & P. Viswanath — *Fundamentals of Wireless Communication*, Cambridge University Press

---

## 👤 Author

**Francesco de Florence**

Feel free to open an issue or submit a pull request for improvements, bug fixes, or new features.

---

## 📄 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.
