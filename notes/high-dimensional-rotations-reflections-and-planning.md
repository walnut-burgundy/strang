# High-dimensional rotations, reflections, and planning before lowering

This note records the current computational question behind the rotation/reflection work and keeps it in the numerical-analysis repository rather than letting it live only in compiler issues.

The motivating semantic operation is:

```text
d = x - y
choose orthogonal Q
Q d = ||d|| e₁
```

Optionally require:

```text
det Q = +1
```

and optionally require the same `Q` to be applied to accompanying data.

The important point is that the semantic goal does **not** uniquely determine the algorithm or even a unique matrix `Q`. The planner should therefore preserve the goal long enough to compare several mathematically correct realizations rather than hard-code one numerical routine in advance.

## Candidate realizations

### Householder reflector

For nonzero `v`, a Householder reflector is

```text
H = I - 2 vvᵀ / (vᵀv).
```

It is orthogonal and has determinant `-1`. With a suitable `v`, one reflector can send `d` to a signed multiple of `e₁`.

For a dense high-dimensional vector this is attractive because the operation is regular:

1. one dot product / reduction;
2. one scalar coefficient;
3. one coordinatewise rank-one update.

That can replace a long sequence of coordinate-by-coordinate decisions, but it is not automatically best for sparse/local data or every target.

### Two reflections for a proper rotation

A product of two reflections has determinant `+1`. Therefore a requirement that the final transform be a proper rotation does **not** imply that the implementation must be a Givens chain.

A Householder-style alignment followed or preceded by an appropriate second reflection is a serious proper-rotation candidate.

### Direct rotation in the data-defined 2-plane

If the only geometric requirement is to carry one distinguished direction to `e₁`, the nontrivial action can be restricted to

```text
span(d, e₁)
```

while the orthogonal complement is fixed.

This gives another proper-rotation family. Degenerate cases such as an already-aligned or antipodal direction need an explicit convention; that is part of the planner contract rather than a reason to pretend the construction is globally unique.

### Serial Givens chain

A Givens rotation acts in one coordinate plane and fixes all other coordinates. A standard serial elimination can zero coordinates one at a time until

```text
d -> ||d|| e₁.
```

This uses `n - 1` plane rotations and is easy to inspect, but the naïve order has a long dependency chain and may expose branch/divergence issues if the order or zero-skipping policy depends on the data.

The small 2D Givens kernel remains an excellent conformance test. It should not be confused with a claim that a long serial Givens chain is the preferred high-dimensional algorithm.

### Balanced Givens tree

The Givens option does not have to mean the serial order

```text
(1,2), (1,3), (1,4), ...
```

A planner can instead generate disjoint plane rotations in layers.

For eight coordinates, first reduce independent pairs:

```text
(1,2)  (3,4)  (5,6)  (7,8)
```

so each pair becomes

```text
(a,b) -> (sqrt(a²+b²), 0).
```

Then combine surviving coordinates:

```text
(1,3)  (5,7)
```

and finally:

```text
(1,5).
```

The total number of rotations is still `n - 1`, but for power-of-two `n` the dependency depth is `log₂ n`. For `n = 32`, that is 31 rotations arranged in 5 layers rather than a 31-step serial chain.

Every factor is a proper plane rotation, so their product remains in `SO(n)`.

This is a useful example of why the planner should generate **factor graphs**, not merely choose a library routine name.

## Represent a factorization as a dependency graph

A useful transform plan should retain more than an ordered list of factors. Candidate metadata includes:

```text
factor
subspace acted on
commutes_with
requires_after
can_share_layer_with
reusable_across_batch
internal reduction shape
precision requirement
cost evidence by target
```

That lets a backend derive execution layers, vectorize independent work, exploit commuting/disjoint factors, and compare sequential application with partial matrix composition where appropriate.

For reflections, this is especially useful because one reflector application itself contains parallel structure:

```text
H x = x - 2 v (vᵀx) / (vᵀv)
```

The dot product is a reduction, then all coordinate updates can proceed independently. Across a batch, many vectors can traverse the same reflector/factor graph independently.

## Cheap mathematical facts should prune candidates before benchmarking

The planner should consume mathematical facts rather than rediscover them at instruction-selection time.

Useful cheap facts include:

```text
dimension
known shape / block structure
orientation requirement
determinant sign
known fixed directions/subspaces
whether one transform is reused across a batch
whether the full transform must be retained
```

For an already specified orthogonal matrix `Q`, reflection theory gives further exact information:

```text
det(Q) = (-1)^k
```

for any factorization into `k` reflections, so orientation fixes reflection-count parity.

A sharper Euclidean reflection-length fact is:

```text
minimal number of hyperplane reflections = rank(I - Q)
```

or equivalently the codimension of the fixed subspace. Do not compute an expensive rank merely to rediscover information already known from the semantics or symbolic structure.

For the present problem, however, the semantic goal `Qd = ||d||e₁` does not determine a unique `Q`, so these facts apply after or during construction of a specific candidate transform, not as a substitute for choosing the family.

## The topology question is about coherent families, not one isolated vector

For one isolated nonzero `d`, ordinary numerical linear algebra is enough to construct a valid alignment. There is no reason to run a cohomology calculation every time a vector is subtracted.

Topology becomes relevant when the planner asks for **one coherent rule over a whole family of directions**.

The standard bundle is

```text
SO(n-1) -> SO(n) -> S^(n-1).
```

A rule that continuously chooses, for every direction `u`, a complete proper rotation carrying `u` to a fixed axis is asking for a global section / continuously chosen complementary frame. That is a stronger problem than factoring one individual matrix.

The practical planner distinction is therefore:

```text
Can each individual transform be constructed?
```

versus

```text
Can the same formula/convention choose those transforms continuously over the whole domain?
```

If a candidate silently assumes one global smooth choice, the planner should require a theorem/certificate supporting that assumption or introduce local charts/case splits instead.

The classical parallelizable-sphere theorem is relevant to the strongest version of this question: only `S⁰`, `S¹`, `S³`, and `S⁷` are parallelizable. This should be treated as a theorem with provenance when used by a planner, not as an undocumented optimizer heuristic.

References for that theorem include:

- Raoul Bott and John Milnor, “On the Parallelizability of the Spheres,” *Bulletin of the American Mathematical Society* 64 (1958), 87–89.
- J. F. Adams, “Vector Fields on Spheres,” *Annals of Mathematics* 75 (1962), 603–632.

## What cohomology can and cannot do here

Cohomology is **not** a scheduler that outputs

```text
rotate coordinates (i,j) next.
```

Its plausible computational value is more global:

- detect or explain obstructions to globally continuous choices;
- warn that one global parametrization/factorization convention cannot be assumed;
- distinguish topological structure that coordinate changes cannot remove;
- expose projective / `Z/2` phenomena connected with reflections;
- classify or compare families of choices;
- provide a certificate that a proposed global simplification is impossible or needs multiple cases.

The useful downstream object is therefore not “the whole cohomology ring in every kernel.” It is a compact derived fact such as:

```text
GlobalConstraint:
    statement: no single global trivialization assumed
    scope: coherent family of rotations over S^(n-1)
    provenance: topological theorem / checked source
    planning effect:
        permit local charts / case splits
        reject a globally smooth selector unless separately proved
```

For a single local `d`, this constraint is normally `not_applicable`.

## Hatcher's reflection construction is unusually close to the numerical problem

Allen Hatcher's Section 3D treatment of `SO(n)` uses reflections directly. If `r(v)` is reflection in the hyperplane perpendicular to `v`, then

```text
rho(v) = r(v) r(e₁)
```

has determinant `+1` and lies in `SO(n)`.

Hatcher also uses the recursive viewpoint

```text
put one direction in place
freeze it
solve the remaining lower-dimensional rotation problem
```

through the tower

```text
SO(n-1) -> SO(n) -> S^(n-1).
```

That is conceptually close enough to Householder alignment and Givens elimination that it belongs in the same notebook, while still serving a different topological purpose.

Primary source:

- Allen Hatcher, *Algebraic Topology*, Section 3D, “The Cohomology of `SO(n)`”: https://pi.math.cornell.edu/~hatcher/AT/ATch3.4.pdf
- Hatcher's separate `SO(n)` page: https://pi.math.cornell.edu/~hatcher/SO/SO.html

Hatcher's separate `SO(n)` material includes computer-generated Bockstein/cohomology diagrams for small `SO(n)`. The source credits M. A. Agosto and J. J. Perez for the Mathematica-generated pictures and Hatcher for commentary.

## CAS status: references exist, executable consultation does not yet

The current Computer Science notes mention mature symbolic systems, but the rotation planner is not presently calling them.

### Macaulay2

Macaulay2 is relevant as a mature algebraic/homological system and as part of the broader rule “ask existing mathematics/software before reimplementing symbolic machinery.” The Coxeter repository separately surveys Macaulay2, Sage, CHEVIE, GAP, OSCAR, and related systems as behavior/oracle sources.

There is currently **no Macaulay2 adapter in the rotation planner** and no test proving that a Macaulay2 result affected algorithm selection.

### cohomCalg

`cohomCalg` is not a general singular-cohomology engine for `SO(n)`. It is specialized to sheaf cohomology of line bundles / toric divisors on toric varieties. It should not be wired into this problem merely because the word “cohomology” appears in its name.

### Current honest status

At present there is:

- no executable Computer Science chooser for rotations;
- no CAS invocation in the rotation-selection path;
- no theorem/provenance gate in CI for the rotation planner;
- no check that “cohomology was consulted” before a local transform;
- no evidence that running a large cohomology computation would improve a local Householder/Givens decision.

That is preferable to pretending the architecture is already executable.

## What the first real planner trace should look like

A first manual/executable selection trace for the semantic request should be something like:

```text
request:
    d = x - y
    align d with e₁
    preserve norm
    proper rotation required? yes/no
    carry same transform to other values? yes/no
    local one-off or coherent family?

derive exact facts:
    dimension
    orientation constraint
    known zero/aligned/antipodal cases
    known sparsity/block structure if actually observed
    batch/reuse information

generate candidate factor graphs:
    Householder
    Householder + second reflection
    direct 2-plane rotation
    serial Givens
    balanced-tree Givens
    later structured/blocked variants

apply mathematical pruning:
    determinant/orientation
    fixed-subspace information
    local-vs-global topology constraint
    known factorization bounds / identities

estimate before measuring:
    operation count
    reduction depth
    dependency depth
    memory traffic
    code size
    register pressure
    branch/divergence risk
    numerical error/stability

benchmark surviving candidates on the actual target

record:
    chosen plan
    rejected plans and reasons
    theorem/symbolic provenance
    measurements
    unresolved assumptions
```

This is substantially more informative than a source-level command such as `use_givens()` or `use_householder()`.

## A useful future theorem/provenance check

The right CI check is not simply:

```text
did a cohomology program run? yes/no
```

That would reward ceremony rather than useful mathematics.

A better rule is claim-dependent:

```text
if request is a single local vector:
    do not require topology/cohomology consultation

if candidate claims one global continuous selector over a domain:
    require a theorem/certificate supporting that claim
    otherwise reject it or introduce explicit charts/case splits
```

A mathematical consultation record should preserve:

```text
claim
source / provenance
scope
status: theorem | exact symbolic result | heuristic | measurement
planning effect
what would invalidate the use of the claim
```

Then the numerical planner can consume the **consequence** of mathematics without rerunning all of mathematics inside a fragment shader or CPU hot loop.

## Relation to the Computer Science repository

Current related records:

- Computer Science #42 — the repository is presently a contemporaneous decision journal, not yet an automatic optimizer.
- Computer Science #51 — genuine 2D Givens rotation as a cross-target conformance kernel.
- Computer Science #53 — choose high-dimensional transform algorithms from evidence, including branch behavior.
- Computer Science #54 — ask mathematics before registers; use symbolic facts to prune before lowering.
- `rotations-and-reflections` branch — numerical linear algebra, topology/cohomology, cheap invariants, factorization parallelism, and LLM-advisory notes.
- Coxeter #1 — survey existing CAS/reflection-group machinery before reimplementing it.
- Coxeter #5 — Householder reflections for high-dimensional alignment.

The Computer Science planner README currently states explicitly that the planner has **not been implemented**. Its intended policy is nevertheless already useful: analytic pruning and cached failures should precede expensive benchmarking, and a manual selection trace should come before implementing only the planner operations the trace proves are needed.

## Numerical-linear-algebra reading trail

Trefethen & Bau is the central reference already present in this repository:

- Lloyd N. Trefethen and David Bau III, *Numerical Linear Algebra*. Philadelphia: SIAM, 1997. ISBN 978-0-89871-361-9.
- Twenty-Fifth Anniversary Edition. Philadelphia: SIAM, 2022. DOI: 10.1137/1.9781611977165.

Relevant parts include orthogonal matrices, QR factorization, Householder triangularization, stability of Householder triangularization, and comparisons with Givens rotations.

Cross-reference:

- [`trefethen-bau-numerical-linear-algebra.md`](trefethen-bau-numerical-linear-algebra.md)

## Working conclusion

There is now enough structure to define the **first real planning experiment**, but not enough implementation to claim an automatic rotation planner exists.

The concrete next computational object is not one pre-decided algorithm. It is a small planner that:

1. accepts the semantic transform request;
2. generates multiple legal factor graphs;
3. consumes cheap mathematical/symbolic certificates;
4. distinguishes local problems from coherent-family/topology problems;
5. estimates dependency and resource shape;
6. benchmarks only the surviving candidates;
7. records why the selected plan won.

That is the bridge from numerical linear algebra and topology to actual CPU/GPU lowering.