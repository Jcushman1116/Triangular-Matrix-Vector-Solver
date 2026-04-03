# Matrix and Vector Product Algorithms

## Overview

Implements and evaluates four column-oriented algorithms for computing matrix-vector
and matrix-matrix products, exploiting the structure of triangular and banded matrices
for computational efficiency. Each algorithm is validated against MATLAB's built-in
operations by comparing absolute and relative error across dimensions n = 30 to n = 100.

## Algorithms

**Subroutine 1 — Unit lower triangular matrix-vector product** (`Lvmult_col`)
Computes z = Lv where L is a unit lower triangular matrix. Complexity: O(n²)

**Subroutine 2 — Compressed unit lower triangular matrix-vector product** (`Lvmult_col_compressed`)
Computes the same product as Subroutine 1 but stores only the nonzero sub-diagonal
elements of L in a 1-D array to reduce memory usage. Complexity: O(n²)

**Subroutine 3 — Banded unit lower triangular matrix-vector product** (`Lvmult_col_banded`)
Computes $z = L_B * v$ where $L_B$ has bandwidth 2, storing only the two sub-diagonals.
Uses the formula:

$$
z_i = \gamma_i + L_{i,i-1}\gamma_{i-1} + L_{i,i-2}\gamma_{i-2}
$$

with the conditions $i-1 \geq 1$ and $i-2 \geq 1$. Complexity: O(n)

**Subroutine 4 — LU matrix-matrix product** (`LUmult`)
Given a matrix A, decomposes it into a unit lower triangular matrix L and upper
triangular matrix U, then computes M = LU using the middle product:

$$
M_{ij} = \sum_{k=1}^{\min(i,j)} L_{ik} U_{kj}
$$

The upper limit is reduced to min(i, j) to skip zero elements. Complexity: O(n³)

## Methodology

Each algorithm is column-oriented, looping over columns j before rows i. Nonzero
entries of all input matrices and vectors are drawn from a normal distribution with
mean 0 and standard deviation 500. Results are compared against MATLAB built-ins
using absolute and relative error:

$$
\text{Absolute error} = \|x_{\text{comp}} - x_{\text{true}}\| \qquad
\text{Relative error} = \frac{\|x_{\text{comp}} - x_{\text{true}}\|}{\|x_{\text{true}}\|}
$$

Relative error for all four algorithms at n = 100 falls in the range of $10^{-16}$,
consistent with floating point machine precision. Error grows with n at a rate
proportional to each algorithm's complexity, with Subroutine 3 (O(n)) producing
the lowest error and Subroutine 4 (O(n³)) the highest growth rate.

## Language

MATLAB

## How to Run

1. Ensure all five `.m` files are in the same directory
2. Open the driver file in MATLAB
3. Follow the instructions at the top of the driver file to run each subroutine
4. The driver will test each algorithm across dimensions n = 30 to n = 100 and
   plot the mean and maximum relative error for each
