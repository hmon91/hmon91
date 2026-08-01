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

The recurring theme is **cheap certificates**: replacing semidefinite-programming-based analysis of neural feedback loops with elementwise sector bounds plus Metzler/Hurwitz conditions, which turns a solver call into a matrix test that runs in microseconds and is independent of network depth.

Always glad to talk about safe learning-based control, positive systems, or potential collaborations.

---

### Research Interests

- **Certified neural-network control** — stability and safety guarantees for NN-in-the-loop systems
- **Positive systems** — Metzler / Hurwitz conditions and positive Aizerman-type results
- **Absolute stability** — sector-bounded nonlinearities, integral quadratic constraints, Lyapunov certificates
- **Reachability and verification** — set propagation and region-of-attraction analysis for learned controllers
- **Robustness under delay and uncertainty** — delay-independent safety, interval uncertainty, stability radius
- **Learning-based control** — imitating optimal or expert controllers while preserving guarantees

---

### Research Code

The repositories below trace one research program, from global stability outward to local, reachable, and delay-robust settings.

| Repository | What it does | Stack |
| :--- | :--- | :--- |
| **[Global stability of NN-controlled systems](https://github.com/hmon91/Global-stability-of-NN-controlled-systems)** | Closed-form sector bound + Metzler/Hurwitz test certifying global exponential stability of an LTI plant under an FFNN controller. Includes an IQC baseline for run-time comparison. Code for the L-CSS 2024 paper. | `MATLAB` `Python` |
| **[Local stability of NN-controlled systems](https://github.com/hmon91/Local-stability-of-NN-controlled-systems)** | Local stability and region-of-attraction analysis for neural feedback loops. | `Jupyter` |
| **[Delay-independent safety of NN controllers](https://github.com/hmon91/Delay-independant-safety-of-NN-controllers)** | Safety certificates that hold irrespective of feedback delay. | `MATLAB` |
| **[PSGModel](https://github.com/hmon91/PSGModel)** | Predator–Swarm–Guide dynamics for crowd modeling and guidance (Siami Lab collaboration). | `MATLAB` |
| **[Reachability of NN feedback loops](https://github.com/hmon91/Reachability-of-NN-feedback-loops)** | Reachable-set propagation through plant–network loops. | `Jupyter` |
| **[Neural network verification](https://github.com/hmon91/neural-network-verification)** | Verification experiments and tooling for learned controllers. | `Jupyter` |

---

### Selected Publications

| Year | Paper | Venue |
| :--: | :---- | :---- |
| 2024 | Ensuring Both Positivity and Stability Using Sector-Bounded Nonlinearity for Systems with Neural Network Controllers &nbsp;—&nbsp; [arXiv](https://arxiv.org/abs/2406.12744) · [IEEE Xplore](https://ieeexplore.ieee.org/document/10561541) · [code](https://github.com/hmon91/Global-stability-of-NN-controlled-systems) | IEEE L-CSS, vol. 8, pp. 1685–1690 |

<sub>Full, up-to-date list on my <a href="https://scholar.google.com/citations?user=5k1HFTkAAAAJ&hl=en">Google Scholar</a>.</sub>

<!-- ─────────────────────────────────────────────────────────────
     OPTIONAL — delete this block or fill it in.
     Add in-progress work only at whatever status you're
     comfortable stating publicly:

| — | Taming Neural Feedback Loops &nbsp;—&nbsp; [arXiv](#) | Under review, IEEE TAC |
| — | Safe reinforcement learning via an Intrinsically Metzler Canonical Form | In preparation |
     ───────────────────────────────────────────────────────────── -->

---

### Toolbox

[![MATLAB](https://img.shields.io/badge/MATLAB-0076A8?style=flat&logo=mathworks&logoColor=white)](#)
[![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)](#)
[![TensorFlow](https://img.shields.io/badge/TensorFlow-FF6F00?style=flat&logo=tensorflow&logoColor=white)](#)
[![LaTeX](https://img.shields.io/badge/LaTeX-008080?style=flat&logo=latex&logoColor=white)](#)
[![Git](https://img.shields.io/badge/Git-F05032?style=flat&logo=git&logoColor=white)](#)
[![Linux](https://img.shields.io/badge/Linux-FCC624?style=flat&logo=linux&logoColor=black)](#)

<sub>Also: CVX · MOSEK · YALMIP for the SDP baselines.</sub>
