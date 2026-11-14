# Numerical infimum checks

This directory contains standalone Maple scripts for the numerical checks in the Brannan-Meijer paper. Run each file in full, beginning with its own `restart`. All function definitions and search settings are included in the file.

## Sampling near parameter boundaries

Several scripts place extra emphasis on values near the boundaries, using three different methods:

| Files | Sampling method |
| --- | --- |
| 01-04 | **Cosine-spaced grids:** points cover the alpha and beta intervals, with closer spacing near both ends. |
| 05-06 | **Hybrid grids:** evenly spaced points across each interval are combined with geometric edge samples, whose distances from each end are 0.01, 0.001, ..., 1e-14. This gives both interior coverage and close boundary sampling. |
| 11-13 | **Transformed coordinates:** logistic alpha-beta coordinates and exponential phi coordinates approach the boundaries closely. Each transformed domain is divided into cells for local optimization and vertex checks. |

For example, the hybrid alpha grids include points 1e-14 from zero and one. The beta grids use the same construction at the ends of their respective intervals. Duplicate points are removed. Here, “hybrid” refers specifically to the uniform-plus-geometric grids in files 05-06.

The main settings are:

| Files | Grid or cell settings | Working precision |
| --- | --- | ---: |
| 01-02 | 121 alpha points and 241 beta points; eps=1e-12 | 80 digits |
| 03 | 161 points per coordinate, plus three beta samples at and around 1/4; eps=1e-12 | 80 digits |
| 04 | 31 alpha points, 31 beta points and 21 rho points; eps=1e-10 | 50 digits |
| 05 | 61 uniform points per coordinate, plus geometric edge samples | 50 digits |
| 06 | 101 uniform points per coordinate, plus geometric edge samples | 50 digits |
| 11-13 | Four cells per transformed coordinate: 256 cells and 625 vertices per main search | 100 digits |

File 01 also refines in beta at alpha=1e-12, 1e-16 and 1e-20, using 120 digits. Files 05-06 use logarithmically spaced s values to bracket stationary points and print their combined alpha-beta grid sizes.

## Scripts

- `01_minimize_Hcal5.txt`: Hcal5star(alpha,beta), a lower bound for Hcal5(alpha,beta,zeta) valid uniformly for zeta in [0,81]. The expected infimum is approximately 0.0388137394068094, approached as alpha tends to zero with beta approximately 0.3390014599579164.
- `02_minimize_Qcal5.txt`: Qcal5star(alpha,beta), a lower bound for Qcal5(alpha,beta,zeta) valid uniformly for zeta in [0,81]. The expected infimum is approached at the corner (alpha,beta)=(0,1).
- `03_minimize_Ptilde.txt`: Ptilde(alpha,beta;phi), checking phi endpoints and bracketed stationary points for each alpha-beta pair. The expected boundary value is approximately 0.3043519050083174 at (alpha,beta,phi)=(1,1,0.061).
- `04_minimize_L0.txt`: L0(alpha,beta,rho;s), checking s endpoints and bracketed stationary points on 1e-8<=s<=10. The expected boundary value is approximately -0.0253581508670938 at (alpha,beta,rho,s)=(0,1,0,0.7149617952310417).
- `05_minimize_tildeL0_S.txt`: tildeL0(alpha,beta,phi;s) with the S normalization, beta>=1/4, for eps<=s<=10.
- `06_minimize_tildeL0_S0.txt`: tildeL0(alpha,beta,phi;s) with the S0 normalization, beta<1/4, for eps<=s<=10.
- `07_tildeL0_corner_limit.txt`: the one-variable limiting expression obtained by setting s=c*phi and letting phi tend to zero at alpha=beta=0.
- `08_tildeL0_corner_direct.txt`: the alpha=beta=0 corner expression with the S0 normalization, for phi in [0.03,0.061] and s in [eps,0.5].
- `09_tildeL0_corner_c_coords.txt`: the same corner expression in phi and c=s/phi, on a rectangle containing the file-08 domain.
- `10_tildeL0_small_box_c_coords.txt`: a four-variable search near alpha=beta=phi=s=0, using s=c*phi and 120-digit precision.
- `11_minimize_L0_tail.txt`: the s>=10 tail of L0(alpha,beta,rho;s), including a separate check at rho=0.
- `12_minimize_tildeL0_S_tail.txt`: the s>=10 tail with the S normalization, including a lower-bound check for the phi=0, beta=1 edge.
- `13_minimize_tildeL0_S0_tail.txt`: the s>=10 tail with the S0 normalization, including its one-sided closure at beta=1/4. The exact phi=0 limit is zero for fixed s>0.

In files 01-02, numerical Meijer evaluations are at zeta=81; values needed at zeta=0 are explicit gamma values. Files 05-06 on sample four phi values: eps, phi0/3, 2*phi0/3 and phi0, since phi0=0.061 is small. Files 07-10 provide additional checks near the joint corner.

The tail scripts use t=1/s in [0,0.1], setting the exact t=0 limit to zero. They apply Maple's Optimization:-NLPSolve in matrix form to each cell and check all vertices. Any failed solve or evaluation marks the scan incomplete. File 11 additionally checks 64 cells on the rho=0 face, and file 12 checks four cells for its boundary lower bound.

## Reported Maple runs

The fine-grid runs of files 01-03 completed with results consistent with their expected boundary values. Files 01 and 02 each returned 29,161 finite evaluations at 80-digit precision, with zero failed evaluations.

| File | Best grid/search value reported | Parameters | Difference from printed boundary reference |
| --- | ---: | --- | ---: |
| 01 | 0.038813759820009260645 | alpha=1e-12, beta=0.3392802673487406 | 2.04131999e-8 |
| 02 | 3.1274558648657449707 | alpha=1e-12, beta=1-1e-12 | 7.91133598e-12 |
| 03 | approximately 0.304351905009 | alpha=beta=1-1e-12, phi=0.061 | 1.936e-13 |

File 01 also completed all three beta refinements at 120-digit precision, with zero failed refinements. At alpha=1e-20 it returned Hcal5star(alpha,beta) approximately 0.03881373940680938519125116 at beta approximately 0.3390014599579164229135584.

The hybrid-grid runs of 05 and 06 returned approximately -0.02729555139436756 and -0.03728635132259653, respectively. Files 07 and 10 reproduced approximately -0.037710421382 with s/phi approximately 0.52813987365. Files 08 and 09 agreed on approximately -0.03707522427866 on their domains bounded away from phi=0.

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

These scripts provide numerical checks on the stated grids and domains. While successful local solves do not certify global minima, these scripts provide independent numerical checks on the stated grids and domains. Stationary points are located by bracketing sign changes of the derivative and applying a numerical root solver. Files 05-06 sample four phi values over the small phi interval; files 07-10 provide additional corner checks.

Independent high-precision evaluations have checked representative function values. Runtime depends on grid sizes, precision and root bracketing. Reduce grid parameters for a short execution check before a full run.
