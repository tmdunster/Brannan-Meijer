# Numerical infimum checks

This directory contains Maple source files used for the numerical infimum checks in the Brannan-Meijer paper.

The files are named by the function or parameter region being minimised, rather than by manuscript equation number, since equation numbers may change between versions.

## Requirements

The routines were run in Maple. Some files use Maple's built-in `MeijerG`; others use elementary functions together with `Optimization`, `fsolve`, or direct bracketing.

Endpoint offsets such as `eps` are numerical devices used to avoid removable singularities in Maple evaluation. They are not changes to the mathematical domains; endpoint values are interpreted by continuous extension in the paper.

## Files

- `01_minimize_Hcal5.txt`  
  Checks the lower bound for the normalised function $\mathcal H_5(\alpha,\beta,\zeta)$ on $\alpha,\beta\in[0,1]$ and $\zeta\in[0,81]$.

- `02_minimize_Qcal5.txt`  
  Checks positivity of the derivative-control function $\mathcal Q_5(\alpha,\beta,\zeta)$ on the same compact set.

- `03_minimize_Ptilde.txt`  
  Checks the main lower-bound function $\widetilde P(\alpha,\beta,\phi)$ on $\phi\in[0,\phi_0]$.

- `04_minimize_L0.txt`  
  Checks the lower bound for the normalised Meijer remainder function $\mathsf L_0(\alpha,\beta,\rho;s)$.

- `05_minimize_tildeL0_S.txt`  
  Checks the lower bound for $\widetilde{\mathsf L}_0(\alpha,\beta,\phi;s)$ on the rectangle $S$, where $\beta\ge 1/4$.

- `06_minimize_tildeL0_S0.txt`  
  Checks the lower bound for $\widetilde{\mathsf L}_0(\alpha,\beta,\phi;s)$ on the rectangle $S_0$, where $\beta<1/4$. This is the longest run.

- `07_tildeL0_corner_alpha_beta_zero.txt`  
  Computes the limiting corner value of $\widetilde{\mathsf L}_0(\alpha,\beta,\phi;s)$ at $\alpha=\beta=0$, using the scaling $s=c\phi$.

- `08_tildeL0_corner_phi_ge_0p03_direct.txt`  
  Checks the $\alpha=\beta=0$ corner expression when $\phi$ is bounded away from zero, using the direct variables $\phi$ and $s$.

- `09_tildeL0_corner_phi_ge_0p03_c_coordinates.txt`  
  Repeats the bounded-away-from-zero corner check using the coordinate $c=s/\phi$.

- `10_tildeL0_small_box_c_coordinates.txt`  
  Performs a four-variable small-box minimisation near $\alpha=\beta=0$, using the coordinate $c=s/\phi$.

## Runtime notes

The most expensive file is `06_minimize_tildeL0_S0.txt`; on a modern laptop it may take several hours. For quick syntax checks, reduce the grid parameters near the top of each file.
