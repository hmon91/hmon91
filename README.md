# State-Space Control of a Sounding Rocket (First-Stage Ascent)

Full state-space analysis and controller design for the first launch phase of a
vertically ascending rocket — from launch to just before booster separation.

Course project, **Advanced Automatic Control**, Department of Aerospace
Engineering, Sharif University of Technology.

Everything is plain MATLAB: **no Simulink and no Control System Toolbox**.
Pole placement, observer design, Lyapunov solving and transfer-function
extraction are all implemented from the underlying formulae, which is the point
of the exercise. `ode45` and `expm` are the only heavy lifting borrowed from the
base product.

```matlab
run_all                  % all 14 items, figures into ./figures, log into ./results
run_all('items', 8:10)   % just the controller-design items
```

Each `scripts/Qxx_*.m` is standalone — it rebuilds the parameters, the linear
model and every gain from scratch, so any single item can be run on its own.

---

## The plant

Vertical flight with altitude-dependent gravity and density, and mass as a state:

$$m\ddot h = T - f(\dot h) - m g(h), \qquad T = K\dot m, \qquad
f(\dot h) = \tfrac12 C_D \rho(h) \dot h^2 S, \qquad
g(h) = \frac{k^2 g_0}{(k+h)^2}$$

with $\rho(h) = \rho_0 (1 - 7\times10^{-6} h)$.

| | state | input | output |
|---|---|---|---|
| | $x = [\,h,\ \dot h,\ m\,]^\top$ | $u = \dot m$ | $y = h$ |

| parameter | value | | parameter | value |
|---|---|---|---|---|
| $K$ | $-3000$ N·kg/s | | $g_0$ | $9.8$ m/s² |
| $C_D$ | $0.4$ | | $m_0$ | $4000$ kg |
| $d$ | $0.7$ m | | $m_f$ | $1200$ kg |
| $k$ | $6371$ km | | $u_0$ | $-26$ kg/s |

The constant input is $u_0 = -26$ kg/s, so the phase lasts
$(m_0 - m_f)/|u_0| = 107.69$ s. At burnout the vehicle reaches
**58.6 km at 1004 m/s**.

---

## Results at a glance

Linearising on the nominal trajectory at $t_{op} = 45$ s, where
$x_{op} = [10979\ \text{m},\ 492.7\ \text{m/s},\ 2830\ \text{kg}]$:

$$A = \begin{bmatrix} 0 & 1 & 0 \\ 5.968\times10^{-5} & -3.031\times10^{-2} & -7.101\times10^{-3} \\ 0 & 0 & 0\end{bmatrix},
\quad B = \begin{bmatrix} 0 \\ -1.0601 \\ 1 \end{bmatrix},
\quad C = \begin{bmatrix}1 & 0 & 0\end{bmatrix}$$

**Open-loop modes** — $\lambda = \{+1.855\times10^{-3},\ -3.217\times10^{-2},\ 0\}$:

- $\lambda = 0$ — the mass channel; $\dot m = u$ is a pure integrator.
- $\lambda > 0$ — **unstable**. Climbing thins the air *and* weakens gravity, so
  extra altitude buys extra acceleration. Doubling time 374 s.
- $\lambda < 0$ — aerodynamic damping on $\dot h$, $\tau = 31$ s.

Unstable in every sense (Lyapunov, asymptotic, BIBO); no pole–zero
cancellation hides the unstable mode from the output. Controllable, observable,
output controllable, and output-functionally controllable with relative degree 2
($CB = 0$, $CAB = -1.06$).

**Designed gains**

| | value | closed-loop poles |
|---|---|---|
| $K$ | $[-0.0590,\ -0.3487,\ +0.0067]$ | $-0.0067,\ -0.2 \pm 0.15i$ |
| $L_r$ | $-0.0590$ | (DC gain from $r$ to $y$ = 1) |
| $K_1,\ K_i$ | $[-0.0967,\ -0.4431,\ +0.0067]$, $-5.896\times10^{-3}$ | above $+\ -0.1$ |
| $L$ | $[2.370,\ 1.808,\ -67.60]^\top$ | $-0.6,\ -0.8,\ -1.0$ |
| $L_{red}$ | $[1.370,\ -67.60]^\top$ | $-0.6,\ -0.8$ |

**Verified behaviour** (from the run log)

| item | result |
|---|---|
| Q03 linear vs nonlinear | 4.0 m altitude discrepancy over 20 s; 33.4 m over the full 62.7 s |
| Q08 regulation | $\delta x \to 0$; $u \in [-26.9,\ -18.4]$ kg/s; $m_{min} = 1206$ kg |
| Q09 feed-forward tracking | error $+0.22\%$ nonlinear, $0.00\%$ linear |
| Q10 integral tracking | error $-0.15\%$ nonlinear; $u \in [-26.8,\ -25.9]$ kg/s |
| Q11 full observer | altitude estimate inside 1 m after 10.7 s |
| Q12 reduced observer | $\dot h$ estimate inside 0.5 m/s after 7.3 s; $h$ error exactly zero |
| Q13/Q14 separation principle | eigenvalue residual $2.5\times10^{-14}$ |

---

## Figures

All 39 figures are produced by `run_all` and written to `figures/`. Delete the
folder and re-run to regenerate every one of them.

### The design argument in one plot

![Pole set comparison](figures/Q07_pole_set_comparison.png)

Closed-loop step response for four candidate pole sets. The original report's
`-2 ± 2.64i` pair looks attractive on paper but needs several hundred kg/s of mass
flow — the vehicle would have to *gain* propellant. Only the two slow designs are
flyable. `Q07` prints the accompanying table with the control-effort column.

### Open-loop ascent and why it is unstable

| | |
|---|---|
| ![Open-loop states](figures/Q01_states.png) | ![Open-loop eigenvalues](figures/Q02_eigenvalues.png) |

Left: 107.7 s of first-stage burn, reaching 58.6 km at 1004 m/s. Right: one
eigenvalue sits in the right half-plane — climbing thins the air and weakens
gravity, so extra altitude buys extra acceleration.

### How far the linearisation can be trusted

![Linear vs nonlinear](figures/Q03_states_du1.png)

Deviation from the nominal trajectory under a `-1` kg/s step, nonlinear against
frozen-coefficient linear. The gap is 4 m over 20 s and 33 m over the full 62.7 s
remaining burn — the model degrades as the nominal carries the state away from the
point where the Jacobians were taken.

### Integral action versus static feed-forward

![Tracking error comparison](figures/Q10_error_comparison.png)

The static pre-compensator of `Q09` leaves a bias because its accuracy is limited
by the model; the integrator of `Q10` removes it. Final error `+0.22 %` against
`-0.15 %` on the nonlinear plant.

### What each estimator costs

![Three feedback variants](figures/Q14_comparison.png)

Feedback from the true state, from a full-order observer, and from a
reduced-order observer. Peak deviation from the true-state command is 1.74 kg/s
for the reduced observer. The full-order observer is comparable *only* once it is
initialised from the measured altitude — see the note in `Q13`.

<details>
<summary><strong>Full figure index (39 files)</strong></summary>

| item | figures |
|---|---|
| Q01 | `Q01_states`, `Q01_forces` |
| Q02 | `Q02_operating_point`, `Q02_eigenvalues` |
| Q03 | `Q03_states_du1`, `Q03_input_du1`, `Q03_states_du2`, `Q03_input_du2` |
| Q04 | `Q04_step_states`, `Q04_modal_states`, `Q04_step_output` |
| Q05 | *(console output only)* |
| Q06 | `Q06_free_response`, `Q06_eig_compare` |
| Q07 | `Q07_pole_set_comparison`, `Q07_admissible_designs` |
| Q08 | `Q08_states`, `Q08_deviations`, `Q08_input` |
| Q09 | `Q09_states`, `Q09_deviations`, `Q09_input`, `Q09_error` |
| Q10 | `Q10_states`, `Q10_deviations`, `Q10_input`, `Q10_error_comparison` |
| Q11 | `Q11_nonlinear`, `Q11_linear`, `Q11_error` |
| Q12 | `Q12_nonlinear`, `Q12_linear`, `Q12_error` |
| Q13 | `Q13_nonlinear`, `Q13_linear`, `Q13_input` |
| Q14 | `Q14_nonlinear`, `Q14_linear`, `Q14_comparison`, `Q14_input` |

All are `.png` at 150 dpi. State plots stack altitude, vertical speed and mass on
shared time axes; `_deviations` plots show the same quantities relative to the
nominal trajectory, which is where the small signals are actually visible.

</details>

---

## Three design decisions worth reading

### 1. Deviations are taken from the nominal *trajectory*, not from a frozen point

This plant has **no equilibrium**: $\dot m = u \neq 0$ always, so $f(x,u)$ never
vanishes and $f_{op} = [492.7,\ 10.33,\ -26]^\top$.

Freezing the operating point gives the affine model
$\dot{\delta x} = f_{op} + A\,\delta x + B\,\delta u$, and a regulator driving
$\delta x \to 0$ then asks the vehicle to *hold a fixed altitude while its engine
burns*. The required gain is enormous and the commanded mass flow comes out at
$u \approx +434$ kg/s — the rocket **gaining** propellant. The nonlinear
simulation diverges outright.

Taking $\delta x = x - x_{nom}(t)$ instead gives
$\dot{\delta x} = A\,\delta x + B\,\delta u$ with no constant term, and
regulation means *staying on the planned trajectory*. Gains are modest and $u$
stays negative. `simulateRocket` integrates the nominal alongside the plant so
no interpolation error enters.

### 2. A slow transmission zero caps the achievable bandwidth

$$G(s) = \frac{-1.0601\,(s + 0.0067)}{s\,(s - 0.001855)(s + 0.03217)}$$

That zero at $-0.0067$ sits between two poles and limits how fast the loop can
be made. Q07 tabulates the consequences:

| pole set | max\|K\| | peak \|Δu\| | overshoot | $t_s$(2%) | admissible? |
|---|---|---|---|---|---|
| $-0.005,\ -2 \pm 2.64i$ | 85.3 | 460 | — | never | **no** |
| $-0.0067,\ -2 \pm 2.64i$ | 10.3 | 1054 | 9.3 % | 1.8 s | **no** |
| $-0.0067,\ -0.2 \pm 0.15i$ | **0.35** | **7.7** | **1.5 %** | **15 s** | **yes** |
| $-0.0067,\ -0.12 \pm 0.09i$ | 0.20 | 3.1 | 1.5 % | 25 s | yes |

Placing one pole *on* the zero near-cancels it and removes the long tail; the
dominant pair then sets the response. The third row is the default
(`p.des.dominantPair`), giving $\zeta = 0.80$, $\omega_n = 0.25$ rad/s.

The first row is the set used in the original coursework report. It is retained
as `p.des.polesOriginalReport` and evaluated by Q07 rather than deleted, because
the comparison is the interesting part. **`simulateRocket` warns explicitly if
any design drives $u \geq 0$.**

### 3. The mass is only weakly observable, and it shows

$m$ reaches the altitude channel only through $A_{23} \approx -7\times10^{-3}$,
so $L_3$ dwarfs $L_1$ and $L_2$. Moving the observer poles from $-1$ to
$-2$ rad/s raises $\|L\|$ from 68 to over 2000 — a straight gain-versus-noise
trade-off, so the poles are kept moderate.

This also produces a nice contrast between Q13 and Q14. Initialising the
full-order observer at zero leaves a 100 m output error which, multiplied by
$L_3$, swings the mass estimate by ~2000 kg and briefly drives $u$ positive.
Initialising from the *measured* altitude — which a real implementation would do
— keeps the peak deviation from the true-state design under 2 kg/s. The
reduced-order observer never estimates $h$ at all, so it cannot suffer that
transient in the first place.

---

## Layout

```
├── run_all.m                     master runner with diary logging
├── src/
│   ├── rocketParams.m            every constant and design choice
│   ├── rocketDynamics.m          nonlinear ẋ = f(x,u)
│   ├── rocketLinearModel.m       nominal trajectory, op point, analytic
│   │                             Jacobians + numerical cross-check, zeros
│   ├── assembleDesiredPoles.m    builds [slowPole, dominantPair]
│   ├── placePoles.m              Bass-Gura, cross-checked by Ackermann
│   ├── designControllers.m       K, Lr, K1/Ki, L, reduced-observer F/G/H
│   ├── simulateRocket.m          one simulator for every configuration
│   ├── systemAnalysis.m          controllability/observability, PBH tests
│   ├── stabilityAnalysis.m       Lyapunov / asymptotic / BIBO + direct method
│   ├── stepMetrics.m             overshoot, rise, settling, steady-state error
│   ├── plotStates.m              h/ḣ/m comparison figures
│   ├── plotSignal.m              scalar-signal figures
│   ├── saveFigure.m              PNG export with a pre-R2020a fallback
│   ├── printHeader.m             console section headings
│   └── yesNo.m
├── scripts/                      Q01 … Q14, one per assignment item
├── legacy/                       original 2018 Simulink models (archive only)
├── figures/                      generated PNGs        (created on first run)
└── results/                      generated run logs    (created on first run)
```

`legacy/` holds the Simulink `ModelInformation` XML exports from the original
submission. They are a readable record of how the diagrams were wired but
**cannot be reopened in Simulink**, and 8 of the 14 models did not survive at
all. See `legacy/README.md`. Nothing in `src/` or `scripts/` depends on them.

| item | script | topic |
|---|---|---|
| 1 | `Q01_open_loop_nonlinear` | nonlinear simulation, force balance |
| 2 | `Q02_linearisation` | operating point, $A/B/C$, modal interpretation |
| 3 | `Q03_linear_vs_nonlinear` | validity range of the linearisation |
| 4 | `Q04_diagonalisation` | modal form, $\Phi(t)$, unit-step response |
| 5 | `Q05_structural_properties` | controllability, observability, PBH tests |
| 6 | `Q06_stability` | Lyapunov / asymptotic / BIBO, direct method |
| 7 | `Q07_desired_poles` | specification → desired poles, effort comparison |
| 8 | `Q08_state_feedback` | pole-placement regulator |
| 9 | `Q09_tracking_feedforward` | static pre-compensator tracker |
| 10 | `Q10_tracking_integral` | integral tracker |
| 11 | `Q11_full_order_observer` | Luenberger observer |
| 12 | `Q12_reduced_order_observer` | Gopinath observer |
| 13 | `Q13_observer_state_feedback` | full observer + feedback, separation principle |
| 14 | `Q14_reduced_observer_feedback` | reduced observer + feedback |

## Implementation notes

- **Pole placement** is computed by Bass-Gura *and* Ackermann, and the two are
  compared against each other and against the achieved eigenvalues, so a silent
  numerical failure cannot pass. Observer gains use the same routine on the dual
  pair $(A^\top, C^\top)$.
- **Jacobians** are derived analytically and validated against a
  central-difference Jacobian at run time.
- **`A` is singular** (the mass integrator), so the usual
  $A^{-1}(e^{At} - I)B$ step-response formula is unusable. The augmented
  identity $\exp\!\big(\begin{bmatrix} A & B \\ 0 & 0\end{bmatrix}t\big)$ is
  used instead, which is exact regardless.
- **Lyapunov equations** are solved through the Kronecker form
  $(I \otimes A^\top + A^\top \otimes I)\,\mathrm{vec}(P) = -\mathrm{vec}(Q)$.
- **Transfer functions** come from
  $C(sI-A)^{-1}B = \det(sI - A + BC)/\det(sI-A) - 1$, so
  `num = poly(A-B*C) - poly(A)` and `den = poly(A)`.
- **Stabilisability and detectability** use PBH rank tests on the eigenvalues
  with $\mathrm{Re}\,\lambda \geq 0$ rather than being inferred from
  controllability and observability, so the code stays correct for plants that
  are only partially controllable.

Tested on MATLAB R2021b and later. `exportgraphics` is used when available and
falls back to `print` on older releases.
