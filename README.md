<div align="center">

# Hamidreza Montazeri Hedesh

### PhD Student · Control Theory and Safe Learning-Based Control

Northeastern University · Siami Lab

[![Website](https://img.shields.io/badge/Website-111111?style=flat&logo=googlechrome&logoColor=white)](https://hmon91.github.io/)
[![Google Scholar](https://img.shields.io/badge/Google%20Scholar-4285F4?style=flat&logo=googlescholar&logoColor=white)](https://scholar.google.com/citations?user=5k1HFTkAAAAJ&hl=en)
[![ORCID](https://img.shields.io/badge/ORCID-A6CE39?style=flat&logo=orcid&logoColor=white)](https://orcid.org/0009-0007-0041-830X)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=flat&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/hamidreza-montazeri/)
[![Email](https://img.shields.io/badge/Email-D14836?style=flat&logo=gmail&logoColor=white)](mailto:montazerihedesh.h@northeastern.edu)

</div>

---

### About

I'm a PhD student in Electrical and Computer Engineering at [Northeastern University](https://www.northeastern.edu/), advised by **Prof. Milad Siami**. My research builds **provable stability and safety certificates for systems controlled by neural networks** — combining positive-systems theory, absolute stability, and sector-bounded analysis so that learned controllers come with formal guarantees rather than empirical hope.

The recurring theme is **cheap certificates**: replacing semidefinite-programming analysis of neural feedback loops with elementwise sector bounds plus Metzler/Hurwitz conditions. This turns a solver call into a matrix test that runs orders of magnitude faster than SDP-based IQC methods — and certifies regimes those methods cannot reach.

Always glad to talk about safe learning-based control, positive systems, or potential collaborations.

---

### Research Interests

- **Certified neural-network control** — stability and safety guarantees for NN-in-the-loop systems
- **Positive systems** — Metzler / Hurwitz conditions and positive Aizerman-type results
- **Absolute stability** — sector-bounded nonlinearities, integral quadratic constraints, Lyapunov certificates
- **Region of attraction and verification** — local sector bounds via layer-wise linear relaxation
- **Robustness under delay and uncertainty** — delay-independent certificates, interval matrices, stability radius
- **Learning-based control** — imitating optimal or expert controllers while preserving guarantees

---

### Software

**[nnsector](https://github.com/hmon91/nnsector)**: sector bounds for feedforward neural networks, in Python and MATLAB.

[![tests](https://github.com/hmon91/nnsector/actions/workflows/tests.yml/badge.svg)](https://github.com/hmon91/nnsector/actions/workflows/tests.yml)
[![Python 3.10+](https://img.shields.io/badge/Python-3.10%2B-3776AB?style=flat&logo=python&logoColor=white)](https://github.com/hmon91/nnsector)
[![MATLAB R2016b+](https://img.shields.io/badge/MATLAB-R2016b%2B-0076A8?style=flat&logo=mathworks&logoColor=white)](https://github.com/hmon91/nnsector/tree/main/matlab)
[![License: BSD-3-Clause](https://img.shields.io/badge/License-BSD--3--Clause-blue?style=flat)](https://github.com/hmon91/nnsector/blob/main/LICENSE)

Give it a trained network, a point `y*` and a box around it. It returns two slopes `gamma1 <= gamma2`, and on the box the network stays between the two lines through `(y*, pi(y*))` with these slopes. This is the form that absolute-stability tests need (positive Aizerman, circle criterion, Lur'e systems), so you can use it to certify a feedback loop with a neural network controller, observer or sensor.

- **Local** sector bounds on a box around any point *(CDC 2025)*, and **global** bounds that hold everywhere *(L-CSS 2024)*
- Works with or without biases, for tanh, sigmoid, ReLU, leaky ReLU and softplus activations
- Reads PyTorch `nn.Sequential` models
- The same functions in Python and MATLAB (the MATLAB version also runs in GNU Octave)

```bash
pip install git+https://github.com/hmon91/nnsector.git
```

```python
import nnsector as ns

net = ns.demo_network()            # small tanh network that ships with the package
sb = ns.local_bound(net, -1, 1)    # box [-1, 1] around y* = 0
print(sb.gamma1, sb.gamma2)        # about -2.786 and -1.200
```

---

### Publications

**Journal**

- **Ensuring Both Positivity and Stability Using Sector-Bounded Nonlinearity for Systems with Neural Network Controllers**
  H. Montazeri Hedesh, M. Siami
  *IEEE Control Systems Letters (L-CSS)*, vol. 8, pp. 1685–1690, 2024
  [IEEE Xplore](https://ieeexplore.ieee.org/document/10561541) · [arXiv:2406.12744](https://arxiv.org/abs/2406.12744) · [code](https://github.com/hmon91/Global-stability-of-NN-controlled-systems) · [library](https://github.com/hmon91/nnsector)

**Conference**

- **Delay-Independent Safe Control with Neural Networks: Positive Lur'e Certificates for Risk-Aware Autonomy**
  H. Montazeri Hedesh, M. Siami
  *American Control Conference (ACC)*, New Orleans, LA, USA, 2026, pp. 134-141
  [IEEE Xplore](https://ieeexplore.ieee.org/document/11616300) · [arXiv:2510.06661](https://arxiv.org/abs/2510.06661) · [code](https://github.com/hmon91/Delay-independant-safety-of-NN-controllers)

- **Robust Stability Analysis of Positive Lur'e System with Neural Network Feedback**
  H. Montazeri Hedesh, M. K. Wafi, B. Shafai, M. Siami
  *IEEE Conference on Control Technology and Applications (CCTA)*, San Diego, CA, pp. 976–981, 2025
  [IEEE Xplore](https://ieeexplore.ieee.org/document/11151493) · [arXiv:2505.18912](https://arxiv.org/abs/2505.18912) · [code](https://github.com/hmon91/Robustness-of-NN-controlled-systems)

- **Local Stability and Region of Attraction Analysis for Neural Network Feedback Systems under Positivity Constraints**
  H. Montazeri Hedesh, M. K. Wafi, M. Siami
  *64th IEEE Conference on Decision and Control (CDC)*, Rio de Janeiro, Brazil, 2025
  [IEEE Xplore](https://ieeexplore.ieee.org/document/11312171) · [arXiv:2505.22889](https://arxiv.org/abs/2505.22889) · [code](https://github.com/hmon91/Local-stability-of-NN-controlled-systems) · [library](https://github.com/hmon91/nnsector)
  <!-- TODO: add page range once you have it from Xplore -->

- **Predator-Swarm-Guide Dynamics: A Hybrid Approach to Crowd Modeling and Guidance in Mass Shooting Scenarios**
  A. Darabi, H. Montazeri Hedesh, M. Siami, M. Sznaier
  *American Control Conference (ACC)*, Toronto, ON, Canada, 2024, pp. 4402–4408
  [IEEE Xplore](https://ieeexplore.ieee.org/abstract/document/10645036) 
  [code](https://github.com/hmon91/PSGModel)

<sub>Full, up-to-date list on my <a href="https://scholar.google.com/citations?user=5k1HFTkAAAAJ&hl=en">Google Scholar</a>.</sub>

---

### Research Code

Each repository reproduces the figures and tables of its paper. The set traces one
research program: from global stability outward to local, robust, and delay-tolerant settings.

| Repository | Paper | Stack |
| :--- | :--- | :--- |
| **[Global stability of NN-controlled systems](https://github.com/hmon91/Global-stability-of-NN-controlled-systems)** | Closed-form sector bound + Metzler/Hurwitz test for global exponential stability, with an IQC run-time baseline. *(L-CSS 2024)* | `MATLAB` `Python` |
| **[Local stability of NN-controlled systems](https://github.com/hmon91/Local-stability-of-NN-controlled-systems)** | Localized Aizerman conditions and layer-wise linear relaxations for region-of-attraction estimation. *(CDC 2025)* | `Jupyter` |
| **[Robustness of NN-controlled systems](https://github.com/hmon91/Robustness-of-NN-controlled-systems)** | Closed-form stability radius for positive Lur'e systems under structured uncertainty, plus sector-bound refinement. *(CCTA 2025)* | `MATLAB` |
| **[Delay-independent safety of NN controllers](https://github.com/hmon91/Delay-independant-safety-of-NN-controllers)** | Certificates holding across state/input delays and interval-matrix uncertainty. *(ACC 2026)* | `MATLAB` |
| **[PSGModel](https://github.com/hmon91/PSGModel)** | Predator–Swarm–Guide dynamics for crowd modeling and guidance. *(ACC 2024)* | `MATLAB` |

---

### Toolbox

[![MATLAB](https://img.shields.io/badge/MATLAB-0076A8?style=flat&logo=mathworks&logoColor=white)](#)
[![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)](#)
[![TensorFlow](https://img.shields.io/badge/TensorFlow-FF6F00?style=flat&logo=tensorflow&logoColor=white)](#)
[![LaTeX](https://img.shields.io/badge/LaTeX-008080?style=flat&logo=latex&logoColor=white)](#)
[![Git](https://img.shields.io/badge/Git-F05032?style=flat&logo=git&logoColor=white)](#)
[![Linux](https://img.shields.io/badge/Linux-FCC624?style=flat&logo=linux&logoColor=black)](#)

<sub>Also: CVX · MOSEK · YALMIP for the SDP baselines.</sub>
