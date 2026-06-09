# Numerical infimum checks

This directory contains Maple source files used for the numerical infimum checks in the Brannan-Meijer paper.

## Requirements

The routines were run in Maple 2025. Some files use Maple's built-in `MeijerG`; others use elementary functions together with `Optimization`, `fsolve`, or direct bracketing.

Endpoint offsets such as `eps` are numerical devices used to avoid removable singularities in Maple evaluation. They are not changes to the mathematical domains; endpoint values are interpreted by continuous extension in the paper.

## Files

- `01_minimize_Hcal5.txt`  
  Checks positivity of $ℋ_5(\alpha,\beta,\zeta)$ on $\alpha,\beta\in[0,1]$ and $\zeta\in[0,81]$.

- `02_minimize_Qcal5.txt`  
  Checks positivity of $𝒬_5(\alpha,\beta,\zeta)$ on $\alpha,\beta\in[0,1]$ and $\zeta\in[0,81]$.

- `03_minimize_Ptilde.txt`  
  Checks positivity of $\widetilde P(\alpha,\beta,\phi)$ on $\alpha,\beta\in[0,1]$ and $\phi\in[0,\phi_0]$.

- `04_minimize_L0.txt`  
  Checks the lower bound for $\mathsf L_0(\alpha,\beta,\rho;s)$ on on $\alpha,\beta\in[0,1]$, $\rho \in [0,2\sin(\phi_0/2)$ and $s\in[0,10]$.

- `05_minimize_tildeL0_S.txt`  
  Checks the lower bound for $\widetilde{\mathsf L}_0(\alpha,\beta,\phi;s)$ on the rectangle $S$, $\phi \in [0,\phi_0$ and $s\in[0,10]$.

- `06_minimize_tildeL0_S0.txt`  
  Checks the lower bound for $\widetilde{\mathsf L}_0(\alpha,\beta,\phi;s)$ on the rectangle $S_0$, $\phi \in [0,\phi_0$ and $s\in[0,10]$. This is the longest run.

- `07_tildeL0_corner_limit.txt`  
  Computes the limiting corner value of $\widetilde{\mathsf L}_0(\alpha,\beta,\phi;s)$ at $\alpha=\beta=0$, using the scaling $s=c\phi$.

- `08_tildeL0_corner_direct.txt`  
  Checks the $\alpha=\beta=0$ corner expression of $\widetilde{\mathsf L}_0(\alpha,\beta,\phi;s)$ when $\phi$ is bounded away from zero, using the direct variables $\phi$ and $s$.

- `09_tildeL0_corner_c_coords.txt`  
  Repeats the bounded-away-from-zero corner of $\widetilde{\mathsf L}_0(\alpha,\beta,\phi;s)$ check using the coordinate $c=s/\phi$.

- `10_tildeL0_small_box_c_coords.txt`  
  Performs a four-variable small-box minimisation of $\widetilde{\mathsf L}_0(\alpha,\beta,\phi;s)$ near $\alpha=\beta=0$, using the coordinate $c=s/\phi$.

## Runtime notes

The most expensive file is `06_minimize_tildeL0_S0.txt`; on a modern laptop it may take several hours. For quick syntax checks, reduce the grid parameters near the top of each file.
