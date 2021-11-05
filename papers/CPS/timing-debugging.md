# Timing Debugging for Cyber-Physical Systems

## Ideas

- Currently studied
  - schedulability analysis
  - scheduling algorithms
- *Not* studied
  - What to change when unable to schedule?

How to assess the change to the system dynamics? In this paper it is defined as *minimal shifts* to the system poles. There are other possible metrics, e.g. peak overshoot, settling time, root sum square difference etc.

## Structure
1. Mathematical model
2. Relation between physical dynamics and poles.
3. Metrics for debugging
4. Optimization algorithm
5. Case study.

## Methods

The **plant model** is linear and time-invariant (LTI).
$$\dot{x}=Ax(t)+Bu(t)$$
Where the deadline for control input is same as its sampling period, and control input is applied *at the deadline*. Therfore
$$x[k+1]=\Phi x[k]+\Gamma u[k-1]$$
where
$$\Phi = e^{Ah}, \Gamma = \int_0^h e^{As}ds$$

**Control law** (and control gain $K$) is considered a known priori. The equation with augmented state vector now becomes
$$z[k+1]=(\Phi_a -\Gamma_a K)z[k]=\Phi_{cl}z[k] $$
where $\Phi_{cl}$ is the closed-loop state-transition matrix and eigenvalues of $\Phi_{cl}$ are the poles of the system.

**Performance measures** include rise time, settling time, overshoot and input energy.

## Example
Second-order DC motor as example. Define the displacement $D=|\rho_1-\rho_1'|+|\rho_2-\rho_2'|+|\rho_3-\rho_3'|$ and $D^*=||\rho_1|-|\rho_1'||$ (furthest pole).

Slow controller varied insignificantly with increasing sampling period while fast controller deviated greatly.

## Heuristics
### Defining optimization objectives.
- $F=\Sigma_{\tau_i \in T}D_i^*$
- $F=\Sigma_{\tau_i \in T}D_i$
- $F=\Sigma_{\tau_i \in T}\hat{D_i}$ where $\hat{D_i}=(D_i^*|p_{i,1}|)^2$

### Algorithm
Using dynamic programming. Each iteration obtains task periods, processor utilization and objective $F$ by incrementing task period by $\delta p$ one task at a time.

### Result
$F=\Sigma_{\tau_i \in T}D_i^*$ and $F=\Sigma_{\tau_i \in T}D_i$ resulted in an identical outcome that is worse than naive approach (increase all task period by equal amount), while $F=\Sigma_{\tau_i \in T}\hat{D_i}$ showed superior results. (Equal to exhaustive search but uses 97.5% less time).

## Extensions

- (more ambitious) Behaviours such as overshoot, settling times instead of just poles (but no closed form solutions). Use linear temporal logic (LTL) formulae. Change periods and make sure the LTL formulae are still satisfied.
- (more basic) Additional questions to ask: (1) non-dominant poles movement; non-critical system can move more; generally more concret parameters. (2) directly use metrics such as settling time in heuristics (but harder to incorperate)