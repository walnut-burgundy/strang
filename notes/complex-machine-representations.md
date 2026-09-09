# Complex values at the machine boundary

This note records implementation work that should remain visible to a future compiler or model reading Strang.

The mathematical type of a complex value is deliberately independent of its machine representation. Several backend experiments already exploit that freedom differently. Those choices, their evidence, and their unresolved conflicts are useful numerical/compiler data and should not be flattened into one vague statement such as “complex is two floats.”

## Semantic rule

Keep these layers distinct:

```text
mathematical complex value
    ↓
chosen numerical coordinates
    ↓
precision width
    ↓
ABI / register representation
    ↓
physical target instructions
```

A backend may choose Cartesian or polar coordinates, scalar registers or a packed vector, F16 or F32, and a particular calling convention without redefining the mathematical field ℂ.

Conversely, changing any of those machine choices is not automatically semantics-preserving. The backend must preserve the shared arithmetic/projective contracts and record the conversion/error behavior.

## Current evidence snapshot

As of 2026-09-09 the most useful implementation lines are:

| Target line | Branch / revision | Complex machine coordinates | Precision | Machine representation |
| --- | --- | --- | --- | --- |
| Android ARMv7 Thumb-2 experiment | `isomorphisms/idric-arm-thumb`, `polar-complex-first`, `e0f23d1a5b3977f1c55da85b530049118cb4f7f6` | polar `(magnitude, phaseTurns)` | F32 | two one-word components; two complex arguments occupy `r0,r1` and `r2,r3` at the softfp boundary; VFP scalar arithmetic inside |
| x86-64 complex/projective leader | `isomorphisms/idric-x86-aggressive-backend`, `x86/complex-projective-leader`, `71322966574d2589bcf07ac012cea878595b887c` | Cartesian `(real, imag)` | F32 | scalar SSE components; working complex pair in `xmm0,xmm1`, RHS in `xmm2,xmm3`, scratch in `xmm4`–`xmm7` |
| GLSL ES complex/projective follower | `isomorphisms/idris-shader-backend`, `complex-projective/follower`, `4360f29d3bbdc09b4cc513e4a6be647f290ffede` | Cartesian `(real, imag)` | F32/highp currently | typed `SVec 2` lowered to GLSL ES `vec2`; physical vendor register allocation remains the driver/compiler's job |
| PowerVR GLES target work | `isomorphisms/idris-shader-backend`, `target/powervr-ge8322-gles`, `4f807b8561bd4704c85ce63af01c277d74c93769` | follows shader coordinate choice | F32 today; explicit F16 work separate | `highp` for F32; `mediump`/F16 only under target evidence, not by global narrowing |

These are implementation records, not a declaration that all backends must converge on the same physical layout.

## Android ARMv7: polar pair experiment

The direct Android CPU backend targets:

```text
Android armeabi-v7a
ARMv7-A
Thumb-2
VFPv3-D16
softfp procedure-call boundary
runtime-free numerical leaves
```

The ordinary scalar ABI sends up to four one-word Float32 arguments through:

```text
r0  r1  r2  r3
```

and returns one Float32 as raw bits in `r0`. Hardware VFP arithmetic is used inside the leaf.

The polar-complex experiment deliberately maps one logical complex value to:

```text
(magnitude, phaseTurns)
```

with both fields F32. `phaseTurns` measures angle in turns:

```text
1       = one full revolution
1/2     = half turn
1/4     = quarter turn
1/8     = eighth turn
```

That choice makes dyadic roots of unity exactly representable in binary32 at the phase-coordinate level.

### Register layout for two complex arguments

The tested four-word layout is:

```text
r0 = left.magnitude
r1 = left.phaseTurns
r2 = right.magnitude
r3 = right.phaseTurns
```

The acceptance fixture explicitly checks:

```text
(2, 1/8 turn) × (3, 1/4 turn)
    =
(6, 3/8 turn)
```

Magnitude multiplication consumes `r0` and `r2`; phase addition consumes `r1` and `r3`.

This is not merely a prose ABI sketch: the ARM self-test loads those exact binary32 words into the four argument registers and checks the returned component bit patterns.

### Internal scalar execution

The current Thumb emitter is correctness-first rather than a first-class complex register allocator. One-word arguments are given stack homes, then Float32 operations use VFP scalar registers such as:

```text
s0
s1
```

for the active operation before writing the result back to its local home.

Thus the current implementation proves the *logical pair ABI* at the function boundary, but it does not yet prove that one logical complex value remains permanently resident as an allocated VFP register pair through an arbitrary expression.

That distinction matters. A future allocator could keep the pair live in VFP registers, but the present evidence should not be rewritten as though that work already exists.

### Current return limitation

The branch explicitly lacks a two-word complex return convention. Therefore one logical polar result is presently exposed by two scalar exported leaves:

```text
polar_multiply_magnitude(...)
polar_multiply_phase(...)
```

Both consume the same four-word pair layout; each returns one F32 component through `r0`.

A future complex ABI should remove this scalar-leaf artifact rather than treating it as the mathematical API.

### Why polar was worth testing

For multiplication and division, polar coordinates expose the operations directly:

```text
(ρ₁, θ₁)(ρ₂, θ₂) = (ρ₁ρ₂, θ₁+θ₂)
(ρ₁, θ₁)/(ρ₂, θ₂) = (ρ₁/ρ₂, θ₁-θ₂)
```

Roots also have a direct phase interpretation. This makes polar representation a plausible target choice for workloads dominated by products, roots, magnitude, and phase.

But addition is not cheap in polar form. Therefore the ARM experiment is evidence for a representation strategy, not proof that polar must be the universal backend representation.

## x86-64: Cartesian scalar-SSE pair

The current complex/projective x86 implementation takes the opposite baseline: one ordinary complex value is two scalar F32 Cartesian components.

The machine-code generator uses the working convention:

```text
xmm0 = left.real
xmm1 = left.imag
xmm2 = right.real
xmm3 = right.imag
```

For complex multiplication it uses `xmm4`–`xmm7` as scalar scratch registers, then restores the result to:

```text
xmm0 = result.real
xmm1 = result.imag
```

Complex division follows the same pair convention.

This is direct machine code: the candidate path does not route arithmetic through C, an assembler, a linker, libc, libm, RefC, or LLVM.

### Why Cartesian is useful here

Cartesian representation makes the current arithmetic corpus straightforward:

- add/subtract componentwise;
- multiply/divide with scalar SSE operations;
- polynomial/rational evaluation without repeated coordinate conversion;
- bounded complex exponential directly in complex arithmetic.

The x86 branch uses x87 `FPATAN`, `FSIN`, and `FCOS` only for observational Cartesian/polar conversion. Those phase/magnitude observations do not feed back into the holomorphic `q → exp(q)` evolution.

This is a useful compiler boundary:

```text
core evolving representation: Cartesian F32 pair
observational polar conversion: separate path
```

### Projective values

A projective point is not lowered as a special runtime quotient object. It remains a sequence of homogeneous complex coordinates using the same underlying complex component representation.

Projective equality is checked through common rescaling or invariant wedges, not by raw component equality. There is no normalization after every arithmetic operation.

## GLSL ES: typed `vec2`, not “the definition of complex”

The shader follower lowers one complex scalar to:

```text
SVec 2
```

which becomes a GLSL ES `vec2` carrying Cartesian arithmetic coordinates.

This is a natural target representation because GPU arithmetic already has two-lane vector syntax. It still must not leak upward into the source semantics:

```text
Complex ≠ arbitrary vec2
C^n ≠ arbitrary vec(2n)
```

The generated complex/projective fixture performs Cartesian complex arithmetic and keeps projective CP¹ checks in terms of the invariant relation.

### Register language versus physical GPU registers

On the CPU backends we control concrete architectural registers such as `r0` or `xmm0`.

For GLSL ES, `vec2` is a compiler-level target value, **not evidence of a particular physical PowerVR/Mali/Adreno register assignment**. The vendor driver may scalarize, pack, fuse, spill, or otherwise allocate it.

Therefore Strang should record:

```text
logical GPU representation: vec2 of declared precision
physical GPU register allocation: unknown until target-specific compiler/disassembly evidence exists
```

Do not invent USC/register facts merely because the shader IR has a `vec2`.

## Precision is an independent axis

The shader work deliberately separates value shape from float width.

Current policy:

```text
F32 -> highp float / highp vecN
F16 -> distinct semantic width
```

F32→F16 demotion must be explicit. F16 and F32 may coexist in one shader.

Portable GLSL ES `mediump` is not automatically claimed to mean exact IEEE binary16 on every GPU. On PowerVR, target documentation and real-device evidence may justify an F16 profile, but that is a target profile rather than a global textual replacement of `highp` with `mediump`.

This is especially important for complex values: the two components must carry the same declared width unless a deliberately mixed representation is introduced and justified.

## PowerVR Android phone work

The PowerVR lane adds a real-device acceptance boundary around generated GLES code:

```text
Idriç / typed shader IR
    -> generated GLSL ES
    -> Android NDK runner
    -> real phone EGL/GLES driver
    -> framebuffer readback
```

The acceptance record is intended to preserve the exact source commit, phone ABI, Android version, EGL/GLES/GLSL strings, renderer/vendor, framebuffer verdicts, and timing data without recording unique device identifiers.

At the recorded branch state, software/Mesa acceptance existed but the architecture-specific PowerVR phone gate still required a real-device PASS record. That status should remain explicit rather than being upgraded by inference.

## The representation disagreement is useful

The ARM polar experiment and x86/GPU Cartesian implementations should coexist in the notes because they expose the real compiler question:

```text
Which representation is cheapest and most accurate for this operation graph
on this target?
```

Possible planner facts include:

```text
coordinate_form: Cartesian | Polar
precision: F16 | F32 | ...
component_count
argument ABI
return ABI
register class
packing / vector width
conversion cost
operation mix
range / phase behavior
branch-cut obligations
spill cost
hardware evidence level
```

Then a backend can choose representation from workload and target evidence rather than baking one storage convention into the mathematical type.

## Conversion should be explicit

If a computation crosses representation families, record the conversion:

```text
Cartesian -> Polar
    magnitude = hypot(real, imag)
    phase = atan2(imag, real)

Polar -> Cartesian
    real = magnitude cos(phase)
    imag = magnitude sin(phase)
```

Those are numerical operations with cost, accuracy, signed-zero, quadrant, zero-magnitude, and branch/phase conventions. A compiler must not treat them as free type casts.

This is also why the realification result in the type notes is a different issue. Realification changes the scalar field representation while preserving a complex structure `J`; Cartesian↔polar conversion changes coordinates *within each complex scalar*.

## What a future register-aware complex value should carry

A useful lowering object is conceptually closer to:

```text
MachineComplex:
    semantic_space
    coordinate_form
    scalar_width
    component_locations
    abi_class
    conversion_provenance
```

For example:

```text
ARMPolarF32:
    coordinate_form = PolarTurns
    scalar_width = F32
    incoming = (r0,r1) for first value

X86CartesianF32:
    coordinate_form = Cartesian
    scalar_width = F32
    working_pair = (xmm0,xmm1)
```

These are backend representations, not new mathematical complex-number types.

A register allocator should preserve pair identity long enough to make good decisions about:

- keeping both components live together;
- spilling/reloading them coherently;
- choosing scratch registers without unnecessary shuffles;
- exploiting vector packing where profitable;
- recognizing operations that only need one component;
- avoiding gratuitous Cartesian↔polar round trips.

## What not to claim yet

Do not infer more than the current evidence shows:

- the ARM branch does not yet have a general two-word complex return ABI;
- the ARM branch does not yet prove a pair-aware global VFP register allocator;
- x86 Cartesian SSE is the current complex/projective leader implementation, not proof that Cartesian must win on every target;
- the shader `vec2` does not identify physical vendor registers;
- portable `mediump` is not automatically exact binary16;
- the PowerVR hardware gate remains separate from software shader validation;
- none of these target layouts defines ℂ itself.

## Source trail

### Android ARMv7 / polar experiment

- `isomorphisms/idric-arm-thumb`
- branch `polar-complex-first`
- revision `e0f23d1a5b3977f1c55da85b530049118cb4f7f6`
- `examples/PolarComplex.idric`
- `src/Backend/ARMThumb/Emit.idr`
- `tests/arm/backend_selftest.S`
- `Makefile`

### x86-64 Cartesian leader

- `isomorphisms/idric-x86-aggressive-backend`
- branch `x86/complex-projective-leader`
- revision `71322966574d2589bcf07ac012cea878595b887c`
- `docs/complex-projective-x86-leader.md`
- `backend/complex_projective.py`

### GLSL ES / GPU follower

- `isomorphisms/idris-shader-backend`
- branch `complex-projective/follower`
- revision `4360f29d3bbdc09b4cc513e4a6be647f290ffede`
- `docs/complex-projective-follower.md`
- `src/Example/ComplexProjectiveFollower.idr`

### PowerVR precision/device work

- `isomorphisms/idris-shader-backend`
- branch `target/powervr-ge8322-gles`
- revision `4f807b8561bd4704c85ce63af01c277d74c93769`
- `docs/float-semantics.md`
- `docs/powervr-phone-acceptance.md`

## Related Strang notes

- [`type-directed-numerical-linear-algebra.md`](type-directed-numerical-linear-algebra.md)
- [`complex-projective-type-system.md`](complex-projective-type-system.md)
- [`geometry-conditioning-and-factorization-choice.md`](geometry-conditioning-and-factorization-choice.md)
- [`trefethen-bau-complex-change-of-basis.md`](trefethen-bau-complex-change-of-basis.md)
