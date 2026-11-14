# Numerical infimum checks

This directory contains standalone Maple source files for the numerical checks in the Brannan-Meijer paper. Run each file in full, beginning with its own `restart`. Each file includes its own function definitions and parameters.

## H and Q: two-dimensional checks

- `01_minimize_Hcal5.txt` evaluates Hcal5star(alpha,beta), a lower bound for Hcal5(alpha,beta,zeta) valid uniformly for zeta in [0,81]. The expected boundary infimum is approximately 0.0388137394068094, approached as alpha tends to zero at beta approximately 0.3390014599579164.
- `02_minimize_Qcal5.txt` evaluates Qcal5star(alpha,beta), a lower bound for Qcal5(alpha,beta,zeta) valid uniformly for zeta in [0,81]. Its expected infimum is approached as (alpha,beta) tends to (0,1). The script evaluates the explicit corner reference.

All numerical Meijer evaluations in these files are at zeta=81. Values needed at zeta=0 are explicit gamma values. Both files use endpoint-biased 121-by-241 grids at 80-digit precision, with eps=1e-12. Exact parameter endpoints are excluded from the interior formulas. At (alpha,beta)=(0,0), the starred functions have direction-dependent limits, whose lower limits exceed their expected minima.

File 01 also refines in beta at alpha=1e-12, 1e-16 and 1e-20, using 120-digit precision and a golden-section search. The beta interval is selected from the smallest-alpha grid row. Grid results, refined values, and stored boundary references are reported separately.

## Finite-s and corner checks

- `03_minimize_Ptilde.txt`: endpoint-biased alpha-beta grid with 161 points per coordinate, plus beta=1/4-eps, 1/4 and 1/4+eps. It uses 80 digits and eps=1e-12. For each pair, it checks phi endpoints and bracketed stationary points using NbinsPhi=120. The expected boundary value is approximately 0.3043519050083174 at (alpha,beta,phi)=(1,1,0.061).
- `04_minimize_L0.txt`: 31-by-31-by-21 endpoint-biased alpha-beta and uniform rho grid, with 50 digits and eps=1e-10. It checks s endpoints and bracketed stationary points on sLo<=s<=10, using sLo=1e-8 and NsBins=160. The expected boundary value is approximately -0.0253581508670938 at (alpha,beta,rho,s)=(0,1,0,0.7149617952310417).
- `05_minimize_tildeL0_S.txt`: the S normalization of tildeL0(alpha,beta,phi;s), for beta>=1/4, on hybrid alpha-beta grids, four phi samples, and eps<=s<=10.
- `06_minimize_tildeL0_S0.txt`: the S0 normalization of tildeL0(alpha,beta,phi;s), for beta<1/4, on hybrid alpha-beta grids, four phi samples, and eps<=s<=10.
- `07_tildeL0_corner_limit.txt`: the one-variable limiting expression obtained by setting s=c*phi and letting phi tend to zero at alpha=beta=0. Its minimum is approximately -0.0377104213819363 at c approximately 0.5281398736487735.
- `08_tildeL0_corner_direct.txt`: the alpha=beta=0 corner expression with the S0 normalization, on phi in [0.03,0.061] and s in [eps,0.5].
- `09_tildeL0_corner_c_coords.txt`: the same corner expression in phi and c=s/phi, on the rectangle stated in the file. Its domain contains the file-08 rectangle and additional points.
- `10_tildeL0_small_box_c_coords.txt`: a four-variable small-box search at 120 digits with s=c*phi and the constraints eps<=c*phi<=smax. Here eps=1e-20 and amax=bmax=smax=0.1.

The endpoint factor (1-alpha)/sin(Pi*alpha) has limit 1/Pi at alpha=1. Files 03, 05 and 06 use the exact endpoint value at alpha=1. The R^2 formula in files 05-06 and 08-10 is evaluated as (exp(s)-1)^2+4*sin(phi/2)^2*exp(s), reducing cancellation near s=phi=0.

## Hybrid alpha-beta grids: files 05-06

Each hybrid grid combines uniform samples with samples at distances 10^(-k), k=2..edgeDigits, from both physical ends of the parameter interval. The default edgeDigits=14 reaches within 1e-14 of the ends. Alpha has physical ends 0 and 1; beta has ends 1/4 and 1 on S, and 0 and 1/4 on S0. Exact alpha=0, alpha=1 and beta=1 are excluded. The S0 beta samples lie strictly between 0 and 1/4; the S grid includes beta=1/4.

The uniform-grid parameters are Nab=60 in file 05 and Nab=100 in file 06. The AddEdges helper combines and sorts the samples, removing exact duplicates, and the code prints the resulting grid sizes. Lower edgeDigits for fewer edge scales, or increase Nab for a finer uniform component.

Both files use Digits=50, four phi values [eps,phi0/3,2*phi0/3,phi0], and endpoint and bracketed stationary-point checks in s. Symbolic derivatives are computed once. CPU times are reported for derivative construction and the search.

## Tail searches: s>=10

- `11_minimize_L0_tail.txt`: L0(alpha,beta,rho,s), with rho=2*sin(phi/2), plus a separate search of the exact rho=0 face.
- `12_minimize_tildeL0_S_tail.txt`: the S normalization of tildeL0(alpha,beta,phi;s), plus a lower-bound check for the phi=0, beta=1 edge.
- `13_minimize_tildeL0_S0_tail.txt`: the S0 normalization of tildeL0(alpha,beta,phi;s), including its one-sided closure at beta=1/4. Its fixed-s phi=0 limit is zero.

These files use t=1/s in [0,0.1], alpha=1/(1+exp(-A)), beta=1/(1+exp(-B)), and phi=phi0*exp(-P). The intervals are A in [-30,30] and P in [0,30], with B in [-30,30] for file 11, [-ln(3),30] for file 12, and [-30,-ln(3)] for file 13. The transformed boxes approach alpha and beta endpoints exponentially.

The kernels are evaluated using bounded powers and decaying exponentials, with the exact t=0 value set to zero before any reciprocal is taken. Digits=100 and UseHardwareFloats=false are set explicitly.

Each domain is split into cells. Maple's Optimization:-NLPSolve is applied in matrix form through a Vector callback, starting at each cell centre and using numerical derivatives. All cell vertices are also checked. Ncells=[4,4,4,4] gives 256 cell solves and 625 vertices per four-dimensional search. File 11 additionally checks 64 cells and 125 vertices on the exact rho=0 face; file 12 checks four cells and five vertices for its special boundary lower bound.

Returned coordinates are checked against the cell bounds using boundCheckTol. Failed solves and failed vertex evaluations are reported, and any failure marks the scan incomplete. Numerical summaries are guarded by successful completion. Transformed points are printed with eight-digit formatting; evaluation precision remains 100 digits.

Matrix-form calling conventions follow [Maple's NLPSolve documentation](https://www.maplesoft.com/support/help/view.aspx?path=Optimization/NLPSolveMatrixForm).

## Reported Maple runs

The supplied file-01 output records 3,721 finite evaluations at 60 digits and zero failures. Its smallest sampled value was 0.038824881080913603865 at alpha=1e-10 and beta=0.345491502843428, approximately 1.11416741e-5 above the printed boundary reference. Completed runs of the fine-grid configurations supplied in files 01-03 have not yet been reported here.

Reported runs of files 02-10 were consistent with the expected values. The hybrid-grid runs of 05 and 06 returned approximately -0.02729555139436756 and -0.03728635132259653, respectively. Files 07 and 10 reproduced approximately -0.037710421382 with s/phi approximately 0.52813987365. Files 08 and 09 agreed on approximately -0.03707522427866 on their domains bounded away from phi=0.

The tail searches at 100 digits returned:

| File / search | Successful cells | Successful vertices | Smallest value found |
| --- | ---: | ---: | ---: |
| 11: L0 tail | 256 | 625 | -9.8096810787199284214e-6 |
| 11: exact rho=0 face | 64 | 125 | -9.8096810787199284214e-6 |
| 12: tildeL0 S tail | 256 | 625 | -5.7804985901604318432e-5 |
| 12: phi=0, beta=1 boundary lower bound | 4 | 5 | -2.8902492950897287496e-4 |
| 13: tildeL0 S0 tail | 256 | 625 | -1.0875851398898281206e-5 |

All listed tail runs reported zero failed cells and zero failed vertex evaluations. Their smallest values occurred at t=0.1, corresponding to s=10. The file-12 special boundary entry is a lower-bound function and agrees with -20/(Pi*exp(10)). The exact phi=0 boundary in file 13 contributes zero. No tail search found a value below its applicable finite-s reference.

## Scope and runtime

These scripts provide numerical checks on the stated grids and domains. Successful local solves do not certify global minima, and bracketed searches locate the roots they bracket. Files 05-06 only sample four phi values since the interval is very small; files 07-10 supplement the checks near the joint corner. The separate cell optimizations on s<=10 and those for Hcal5star(alpha,beta), Qcal5star(alpha,beta) and Ptilde(alpha,beta;phi) described in the paper are outside this collection.

Independent high-precision evaluations have checked representative function values. Runtime depends on grid sizes, precision and root bracketing. Reduce grid parameters for a short execution check before a full run.
