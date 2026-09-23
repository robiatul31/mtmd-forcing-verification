# Verification code for *Mixed Triple Metric Dimension of Polyhedral Graphs: Local Forcing and Extremal Families*

by R. Adawiyah, Rinurwati and M. Nagaraj.

This repository contains the computational verification accompanying that paper.
Every row of Table 2 of the paper is reproduced by a single self-contained
program. There are eleven rows and eleven checks, in the same order.

## Contents

| File | Description |
|---|---|
| `mtmd_forcing_verify.pl` | the verification program (core Perl, no external dependencies) |
| `verify_output.txt` | transcript of a complete run |
| `README.md` | this file |
| `LICENSE` | CC0 1.0 (public domain dedication) |

## Requirements

Core Perl 5 only. No modules beyond the standard distribution, no external
libraries, and no floating-point arithmetic anywhere: every computation uses
exact integer arithmetic.

## Running

Full run, reproducing every range stated in Table 2:

    perl mtmd_forcing_verify.pl

Reduced-range smoke test (a few seconds):

    perl mtmd_forcing_verify.pl --quick

Single check by name:

    perl mtmd_forcing_verify.pl --only=forcing-theorem

List the available checks:

    perl mtmd_forcing_verify.pl --list

The program prints a verdict for each check and exits with status 0 if all pass,
nonzero otherwise. A complete run takes well under a minute on a desktop
machine.

## Method

Each graph is built from its combinatorial definition. All vertex distances are
obtained by breadth-first search, the element list `V(G) u E(G) u F(G)` is built
explicitly with each element stored as its set of incident vertices, and the
distance from a vertex to an element is the minimum distance to an incident
vertex.

A candidate set `S` is tested by hashing the code of every element, so that `S`
resolves precisely when no collision occurs. This costs `O(|S| . (|V|+|E|+|F|))`
per candidate rather than the quadratic cost of pairwise comparison, which is
what makes the exhaustive searches feasible. Minimum values are obtained by
testing subsets of increasing size and stopping at the first success, so every
reported minimum is a true minimum and not merely an upper bound.

Two further routines support the analysis rather than the verification: one
lists every vertex separating a fixed pair of elements, and so computes the
forced set `F(G)`; the other reports the first unresolved pair for a candidate
set.

## The eleven checks

The names below are the names the program prints, and they appear in the order
of the rows of Table 2.

| # | Check | What it establishes |
|---|---|---|
| 1 | `antiprism` | `MTMD(A_n) = 2n`, and each `b_i` is the unique separator of its pair, for `3 <= n <= 25` |
| 2 | `icosahedron` | `MTMD(I) = 9 < 12` and `F(I)` is empty, by exhaustive minimisation over one graph |
| 3 | `bipyramid` | `MTMD(B_n)`, equal to `\|V\|` for `n = 3,4` and to `\|V\|-1` for `n >= 5` |
| 4 | `bipyramid-theorem` | every internal step of the proof that `MTMD(B_n) = n+1`, re-derived independently |
| 5 | `forcing-theorem` | the forcing criterion against a direct separator count over all triangular edge–face pairs |
| 6 | `faces-complete` | Euler's formula and edge–face incidence, so that no face is missing or duplicated |
| 7 | `lower-bound-3` | no set of size 1 or 2 resolves a 2-connected plane graph |
| 8 | `A4-anomaly` | every step of the proof that `mmd(A_4) = 5`, against the parity formula in the literature |
| 9 | `delete-one` | `V` minus one vertex resolves if and only if that vertex is neither dominated nor critical |
| 10 | `face-dimension` | the `dim_f` column of Table 1, and the antiprism reduction |
| 11 | `mmd-values` | the `dim_m` values of Table 1, including the corrected `dim_m(A_n)` |

Checks 4, 5, 7 and 8 test *proofs* rather than only their conclusions. Checks 2
and 3 are exhaustive minimisations over single finite graphs, which on a fixed
finite graph is a proof rather than evidence.

## Scope

The paper treats the triangular-face regime, where the forcing criterion has
instances. A companion paper treats the complementary triangle-free regime, and
its exact values for the prisms, the stacked prisms `C_n x P_r` and the layered
family `L(n,r,sigma)` are **not** checked here.

The prism and stacked-prism builders are nevertheless retained, because checks
6, 7, 9, 10 and 11 run over a corpus of graphs that includes them. Check 9 is
the clearest case: the predicate "neither dominated nor critical" is compared
against a direct test of whether `V` minus that vertex resolves, and on a
triangle-free graph no vertex is critical, so those members of the corpus act as
controls rather than as results.

## Licence

Released into the public domain under CC0 1.0. See `LICENSE`.
