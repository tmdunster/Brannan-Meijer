# Numerical infimum checks

This directory contains standalone Maple source files for the numerical checks in the Brannan-Meijer paper. Run each file in full, beginning with its own `restart`. Each file includes its own function definitions and parameters.

## H and Q: two-dimensional checks

- `01_minimize_Hcal5.txt` evaluates the two-variable lower bound Hcal5star(alpha,beta). All numerical Meijer evaluations are at zeta=81. Its expected boundary infimum is 0.0388137394068094, approached at alpha=0, beta=0.3390014599579164. This lower bound is a different function from the direct Hcal5 search formerly used in this file.
- `02_minimize_Qcal5.txt` evaluates Qcal5star(alpha,beta). The positive-part penalty is zero for alpha>=beta. All numerical Meijer evaluations are at zeta=81. The expected boundary infimum is 3.1274558648578336 at (alpha,beta)=(0,1).

These two-dimensional lower bounds apply uniformly for zeta in [0,81]. Neither script searches for zeta stationary points or differentiates in zeta. Values needed at zeta=0 are explicit gamma values.

Both scripts use a configurable endpoint-biased grid and report failed evaluations. Exact parameter endpoints are excluded from the numeric Meijer formulas. At (alpha,beta)=(0,0), the starred functions have direction-dependent limits, whose lower limits exceed their expected minima. The finite grid minima and printed boundary references are distinct.

## Other scripts

- `03_minimize_Ptilde.txt`: alpha-beta grid plus endpoint and bracketed stationary-point checks in phi. Both sides of beta=1/4 are explicitly sampled. The expected boundary value is 0.3043519050083174.
- `04_minimize_L0.txt`: defining Meijer remainder kernels, on parameter grids and sLo<=s<=10. The expected boundary value is -0.0253581508670938 at (alpha,beta,rho,s)=(0,1,0,0.7149617952310417).
- `05_minimize_tildeL0_S.txt`: the S branch, beta>=1/4, on hybrid alpha-beta grids (original uniform points plus geometric edge samples), four phi values, and eps<=s<=10.
- `06_minimize_tildeL0_S0.txt`: the S0 branch, beta<1/4, on hybrid alpha-beta grids (original uniform points plus geometric edge samples), four phi values, and eps<=s<=10.
- `07_tildeL0_corner_limit.txt`: the one-variable limiting function with s=c*phi at alpha=beta=0. The minimum is -0.0377104213819363 at c=0.5281398736487735.
- `08_tildeL0_corner_direct.txt`: the alpha=beta=0 corner expression on phi in [0.03,0.061] and s in [eps,0.5].
- `09_tildeL0_corner_c_coords.txt`: the same expression on a broader rectangle in phi and c=s/phi. Its actual domain is stated in the file; it contains the file-08 rectangle and additional points.
- `10_tildeL0_small_box_c_coords.txt`: a four-variable small-box search with the exact transformed constraints eps<=c*phi<=smax.

The endpoint factor (1-alpha)/sin(Pi*alpha) has limit +1/Pi at alpha=1. Files 03, 05 and 06 now use the exact endpoint value without replacing nearby interior parameter values by their limiting values. The R^2 formula in files 05-06 and 08-10 is evaluated as (exp(s)-1)^2+4*sin(phi/2)^2*exp(s), reducing cancellation near s=phi=0.

## Hybrid alpha-beta grids: files 05-06

Files 05-06 keep every original uniform alpha-beta grid point and add samples at distances 10^(-k), k=2..edgeDigits, from both physical ends of each parameter interval. The default edgeDigits=14 reaches within 1e-14 of the edges; exact endpoints are excluded. Alpha has physical ends 0 and 1; beta has ends 1/4 and 1 on S, and 0 and 1/4 on S0. The S0 additions remain strictly below 1/4. A small AddEdges helper combines and sorts the points, removing exact duplicates. The code prints the resulting grid sizes.

The uniform-grid parameters remain Nab=60 in file 05 and Nab=100 in file 06. Each parameter receives at most 26 additional points. The parameter-pair counts are therefore at most 87^2 versus 61^2 in file 05, and 127^2 versus 101^2 in file 06: approximately 2.03 and 1.58 times the original numbers. These ratios count grid pairs, not measured runtime. Lower edgeDigits for fewer added edge scales, or increase Nab to refine the uniform interior grid.

The original Digits=50 setting, kernel formulas, symbolic derivatives, normalizations, four phi samples, s interval, stationary-point bracketing counts, and CPU-time output are unchanged. Independent checks confirm preservation of the uniform grids, valid added points, and sufficient evaluation precision at representative new near-edge points. The hybrid-grid versions of both scripts have completed Maple runs; their reported best values are listed below.

## Tail searches: s>=10

- `11_minimize_L0_tail.txt`: L0(alpha,beta,rho,s), with rho=2*sin(phi/2), plus a separate search of the exact rho=0 face.
- `12_minimize_tildeL0_S_tail.txt`: the S normalization, beta>=1/4, plus the explicit lower-bound check for the phi=0, beta=1 edge.
- `13_minimize_tildeL0_S0_tail.txt`: the S0 normalization, beta<1/4, including its one-sided closure at beta=1/4. Its fixed-s phi=0 limit is zero.

These standalone files use the t=1/s substitution, with t in [0,0.1]. They use alpha=1/(1+exp(-A)), beta=1/(1+exp(-B)), and phi=phi0*exp(-P), with A in [-30,30] and P in [0,30]. The B interval is [-30,30] for L0, [-ln(3),30] for S, and [-30,-ln(3)] for S0. Alpha and beta endpoint values are approached exponentially; exact parameter-edge limits are handled separately from these finite transformed boxes.

The functions are evaluated directly at high precision using exp(-1/t), with the exact t=0 value set to zero before any reciprocal is taken. The normalized kernels are written in terms of bounded powers and decaying exponentials. Digits controls the working precision; the substitutions preserve the original normalizations.

Each four-dimensional domain is split into bounded cells. Maple's Optimization:-NLPSolve is applied in matrix form through a Vector callback, starting at each cell centre and using numerical derivatives. All cell vertices are checked as well, including t=0 and t=0.1. The default Ncells=[4,4,4,4] gives 256 cell solves per file; file 11 also performs 64 solves on the rho=0 face. Set Ncells=[1,1,1,1] for a short first run and increase the counts for refinement. Digits=100 and UseHardwareFloats=false are set explicitly. Failed solves and failed vertex evaluations are counted and reported; inspect any failures before accepting the run. A successful local solve in every cell remains a numerical check.

The tail calls pass bounds and omit the rejected feasibilitytolerance option. boundCheckTol is used solely for checking returned coordinates. The first three cell or vertex exceptions are printed, and any failure marks the scan incomplete. Each result is initialized to FAIL, and numerical summaries are guarded, so a worksheet continuing after an error cannot print secondary arithmetic/printf errors or reuse an old result. Kernel formulas and search domains are unchanged. The full matrix-form searches in files 11-13 have completed Maple runs at 100 digits with no failed cell solves or vertex evaluations, including the separate boundary checks. Their reported values are listed below. These local numerical searches do not establish certified global minima.

Matrix-form calling conventions follow [Maple's NLPSolve documentation](https://www.maplesoft.com/support/help/view.aspx?path=Optimization/NLPSolveMatrixForm). The one-dimensional special boundary check in file 12 uses the same cell routine.

## Scope and runtime

These scripts are numerical checks with the stated finite grids and search domains. Files 04-06 check s<=10 and files 11-13 supply the tail cell searches. The separate cell optimizations on s<=10 and those for Hcal5star, Qcal5star and Ptilde described in the paper are still not included in this directory.

The grids in files 05-06 sample only four phi values; the corner scripts are supplemental checks, not replacements for a continuous-phi search over all alpha,beta. Bracketed root searches find the roots they bracket.

The runs recorded below were performed in Maple and supplied for comparison with these scripts. Independent high-precision evaluations were also used to check representative function values. Reduce grid parameters for a quick execution check before starting a full scan.


## Reported Maple runs

Files 02-10 have completed runs with results consistent with their expected values. The hybrid-grid runs of 05 and 06 returned best values approximately -0.02729555139436756 and -0.03728635132259653, respectively. Files 07 and 10 reproduced the limiting-corner value approximately -0.037710421382 with s/phi approximately 0.52813987365. Files 08 and 09 agreed on approximately -0.03707522427866 in their domains bounded away from phi=0.

The final tail searches returned the following results. Counts include the separate boundary searches where applicable.

| File / search | Successful cells | Successful vertices | Smallest value found |
| --- | ---: | ---: | ---: |
| 11: L0 tail | 256 | 625 | -9.8096810787199284214e-6 |
| 11: exact rho=0 face | 64 | 125 | -9.8096810787199284214e-6 |
| 12: tildeL0 S tail | 256 | 625 | -5.7804985901604318432e-5 |
| 12: phi=0, beta=1 boundary lower bound | 4 | 5 | -2.8902492950897287496e-4 |
| 13: tildeL0 S0 tail | 256 | 625 | -1.0875851398898281206e-5 |

Every tail run above reported zero failed cells and zero failed vertex evaluations. The listed tail values occurred at t=0.1, corresponding to s=10. The special boundary entry for file 12 is a lower-bound function, not the unbounded-domain kernel itself, and agrees with its explicit reference -20/(Pi*exp(10)). The exact phi=0 boundary in file 13 contributes the limiting value zero. None of these searches found a value below the applicable finite-s reference.

An earlier, coarser version of file 01 was run successfully. The longer refined version currently supplied in file 01 has not yet had a completed run reported here. Stored reference constants are distinguished from computed search outputs throughout.
