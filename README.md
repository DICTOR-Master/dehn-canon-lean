# The Dehn Canon: Hilbert's Third Problem, Machine-Checked (Lean 4 + Mathlib)

Author: DICTO.

## The theorems

The Dehn invariant `dehnSet P` is defined from the solid `P` as a set of points of 3-space, with no list of edges as
input. At a point of `P` the directions that stay in `P` form a cone; where the directions `v` with `v` and `-v` in the
cone span a line, the angle there is 2π times the area of the cone in the unit disc across that line, divided by the
area of the disc. `dehnSet P` adds, over the angle classes θ modulo rational multiples of π, the length of the points
with class θ, tensored with θ, in ℝ ⊗[ℚ] (ℝ/ℚπ). The Challenge proves:

1. **Hilbert's third problem in Hilbert's words** (`hilbert_third_problem`): two tetrahedra `A B C D` and `A B C D'` on
   one and the same base triangle, with both apexes at distance 1 from its plane and both volumes 1/6, are neither
   scissors congruent nor equicomplementable. The pair is Aigner and Ziegler's T₁ and T₂.
2. **The edge sum** (`dehnSet_eq_edge_sum`): for every convex polytope with interior points, `dehnSet P` is the sum over
   the edges of (length) ⊗ (interior dihedral angle), with the edges, the facets through an edge and the dihedral
   angle also defined from the set: Dehn's invariant in Jessen's tensor form.
3. **The bridge** (`dehnSet_eq_of_scissorsCongruent`): scissors-congruent convex polytopes have the same invariant, in
   every 3-dimensional real inner product space; and for finite unions of convex polytopes
   (`dehnSum_eq_of_scissors`) the total over the pieces does not depend on the dissection and is kept by scissors
   congruence.
4. **Dehn's theorem** (`similarTetra_not_scissorsCongruent_similarCube`): no similar copy of the regular tetrahedron is
   scissors congruent to a similar copy of the cube.

Pieces of a dissection are convex polytopes (bounded intersections of finitely many closed half-spaces), finitely many,
moved by isometries, reflections included. Sydler's converse is not stated.

- `Challenge.lean`: the definitions and the five statements, with every proof `sorry`. It imports only Mathlib. Every
  definition and statement is copied, character for character, from the development by a script.
- `Solution.lean`: the Challenge word for word, with proofs that transfer the theorems of the development.
- `DehnCanon/`: the development (32 modules, namespace `DehnCanon.Library`): the invariant of a set and its plane-cut identity, invariance under
  isometries, additivity over dissections, the sector-area angle, the model solids, scaling, the edge sum, and the
  volumes and altitudes of the pair.

Toolchain `leanprover/lean4:v4.35.0-rc2`, Mathlib `v4.35.0-rc2`. The five theorems depend only on the axioms
`propext`, `Classical.choice` and `Quot.sound`. Neither `sorry` nor `native_decide` is used. Every Lean file uses the
module system.

## How it was made

DICTO set the question and the order of the work. A Claude Code agent (Claude Opus, Anthropic) wrote the definitions,
the statements and the proofs, and ran the checks. `formalization.yaml` (section `automation`) records this in full.

## Related work

- FormalBook, chapter 10 (mo271/FormalBook, pull request #157): the first Lean proof of Hilbert's third problem, by the
  route of *Proofs from THE BOOK*, in an abstract model of segments and angles, with the same pair of tetrahedra. This
  entry was made independently, by Dehn's invariant, and proves the bridge from geometric dissections that the model
  leaves out.
- `formalization.yaml` lists the other formalizations found (with pinned commits) and every source checked.

## Checks

`scripts/check-lean-sources.py` and `scripts/verify-comparator.sh` come from Palomar's entry template (PalomarRegistry).
They check the source rules and run `lake comparator` the way Palomar does.

## How to cite and reuse

### How to cite

- **The paper.** DICTO, *The Dehn Canon: Hilbert's Third Problem, Machine-Checked — From the solids up, deciding every
  pair of the canon* (paper #113), Zenodo, 2026. DICTO DOI: [10.5281/zenodo.24383552](https://doi.org/10.5281/zenodo.24383552)
  (all versions).
- **The Palomar entry.** PALOMAR-2026-11-20-000003, version 1:
  https://palomar-registry.org/entry?id=PALOMAR-2026-11-20-000003&version=1
- **The repository commit.** github.com/DICTOR-Master/dehn-canon-lean, commit
  `9ea20f6dfd64fc3f10f64b1fb59a754a77e5cf57` (the commit registered on Palomar). Later commits change only metadata and
  documentation, not the Lean files.

`CITATION.cff` holds the same data in machine-readable form (GitHub shows it under "Cite this repository").

### What the library offers

The five theorems of the Challenge are listed above (namespace `DehnCanon`). The development in `DehnCanon/` (namespace
`DehnCanon.Library`) can be used on its own; every name below is in that namespace.

- The invariant. `ratPi`, `Angle` (ℝ/ℚπ), `DehnSpace` (ℝ ⊗[ℚ] Angle), `edgeTerm`, `dehn` (the sum over a list of
  edges) in `DehnCanon1.lean`; `tcone`, `linSpace`, `wedgeAngle`, `theta`, `edgeAngle` and `dehnSet` (the invariant of
  a set) in `Dehn2Local.lean`.
- Polytopes and dissections. `sol` (an intersection of finitely many closed half-spaces) and `IsPoly` (a bounded one) in
  `Dehn2Poly.lean`; `ScissorsCongruent` in `DehnCanon2.lean`.
- Properties of `dehnSet`. `dehnSet_cut` (the plane-cut identity, `Dehn2Cut.lean`); `dehnSet_image` (invariance under
  affine isometries, `Dehn2Motion.lean`); `dissection_additive` (a set function additive under every plane cut of a
  convex polytope is additive over every dissection into convex polytopes, `Dehn2Poly.lean`); `dehnSet_dissection`,
  `dehnSet_eq_of_scissorsCongruent`, `not_scissorsCongruent_of_dehnSet_ne`, `dehnSum_eq_of_scissors`
  (`DehnCanon2.lean`); `dehnSet_smul_set`, `IsSimilarCopy`, `dehnSet_similarCopy` (`Dehn4Similar.lean`).
- The edge sum. `edges`, `facetNormals`, `dihedral` and `dehnSet_eq_edge_sum` in `Dehn10Edges.lean`.
- Angles and volumes. `tetDihedral_not_ratPi` (arccos(1/3) is not a rational multiple of π, from Mathlib's `niven`,
  `DehnCanon1.lean`); `volume_sector` and `wedgeAngle_two_planes` (`Dehn3Sector.lean`); `volume_iUnion_poly` and
  `volume_eq_of_scissorsCongruent` (`Dehn9Vol.lean`).

### How to import it

The library builds with `leanprover/lean4:v4.35.0-rc2` and Mathlib `v4.35.0-rc2`; a project that requires it should use
the same toolchain and Mathlib. In `lakefile.toml`:

```toml
[[require]]
name = "DehnCanon"
git = "https://github.com/DICTOR-Master/dehn-canon-lean.git"
rev = "9ea20f6dfd64fc3f10f64b1fb59a754a77e5cf57"
```

Then import the modules you need, for example:

```lean
import DehnCanon.DehnCanon2
import DehnCanon.Dehn10Edges
```

### Licence

The Lean code: Apache-2.0 (see `LICENSE`). Copyright 2026 DICTO. The paper #113: CC BY-SA 4.0.

## Other work by DICTO

- The paper: DICTO, *The Dehn Canon: Hilbert's Third Problem, Machine-Checked. From the solids up, deciding every pair
  of the canon*, Zenodo, 2026. DICTO DOI: 10.5281/zenodo.24383552
- ORCID (all works): https://orcid.org/0009-0001-2620-2036
- Website: https://dictospheres.com
