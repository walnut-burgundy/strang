# Complex values at the machine boundary

This is an evidence ledger for complex-number representation work that already exists across the compiler, CPU backends, Android application work, and shader backends.

The purpose is not to choose one representation. It is to prevent later work from forgetting experiments that have already been implemented, tested, rejected, or deliberately kept provisional.

## Keep five layers separate

```text
mathematical semantics
    ↓
numerical coordinates
    ↓
precision
    ↓
ABI / register or shader representation
    ↓
target instructions and execution evidence
```

The source-level complex type is not defined by any one machine layout. Cartesian `(real, imaginary)`, polar `(modulus, argument)`, two scalar registers, a shader `vec2`, or a pair of F32 words are lowering choices.

The reverse warning matters too: changing representation is not a free cast. Cartesian↔polar conversion changes numerical work, exceptional cases, branch/phase conventions, and error behavior.

## Current implementation hierarchy

For the current Idriç complex/projective subsystem, the merged ai-ci policy records:

```text
canonical mathematical semantics
    -> direct x86-64 executable leader / CPU oracle
    -> shared numerical/projective corpus
    -> Thumb-2, shader/GPU, application, and other followers
```

General backend development can still be Thumb-led. This is a narrow exception for complex/projective arithmetic.

The AArch64 ICK/GCC work below is important representation evidence, but it is a separate compiler experiment and Android application path rather than the canonical Idriç complex/projective leader.

## Evidence ledger

| Line | Role/status | Physical or target representation | Boundary | Evidence actually present |
| --- | --- | --- | --- | --- |
| Idriç complex/projective semantics | semantic source, still draft/stacked in PR #81 | deliberately unspecified | `ComplexCoordinates complex n`, projective quotient types, shared F32 corpus | typecheck/executable structural tests; no machine layout mandated |
| x86-64 direct backend | current complex/projective executable leader, PR #22 draft | Cartesian F32 pair; scalar SSE | internal machine-code convention | direct ELF64 generation, native execution, shared numerical/projective corpus, thin Debian, deterministic headless render |
| ARMv7 Thumb-2 Idriç backend | `PROVISIONAL_DISPOSABLE` follower, PR #53 draft | temporary polar F32 pair `(magnitude, phaseTurns)` | softfp one-word argument ABI | lowering + Thumb assembly + QEMU complex multiplication PASS; full Cartesian/projective corpus and physical device SKIP |
| AArch64 ICK/GCC | merged experimental compiler implementation, `rhs` PR #1 | floating `_Complex` physical storage `(modulus, argument)` | `_Complex` must remain inside ICK-compiled unit; scalar/array Cartesian boundary outside | static polar bytes, multiplication/division, focused tests under qemu at `-O0/-O2`, Android NDK link; known remaining representation limits |
| Wegert Android arm64 consumer | merged application integration, Wegert PR #13 | ICK polar `_Complex` inside isolated AArch64 object; Cartesian outside | exported scalar/array Cartesian API | object checksum/ELF checks, APK arm64 link; app keeps armeabi-v7a and x86_64 Cartesian fallbacks and GLES Cartesian upload |
| GLSL ES complex/projective follower | shader follower, PR #36 draft | Cartesian `SVec 2` → GLSL ES `vec2` | shader IR / GLSL | typed IR, generated GLSL, validation, program link PASS; driver load/GPU execution/framebuffer/vendor device SKIP in this lane |
| PowerVR / other shader target work | target precision and hardware-followup work | target-dependent; no proven physical complex register assignment | GLSL/driver/device | precision policy and separate device receipt machinery; do not infer vendor registers from `vec2` |

This table is the first thing a future machine should consult before inventing another representation.

## x86-64: current executable complex/projective leader

The x86-64 candidate uses direct machine code and two scalar Float32 Cartesian components.

The working convention in the complex arithmetic generator is:

```text
xmm0 = left.real
xmm1 = left.imag
xmm2 = right.real
xmm3 = right.imag
```

Complex multiply/divide use `xmm4`–`xmm7` as scalar scratch and restore the result to `xmm0,xmm1`.

This is real register-level implementation evidence, not just an ABI proposal. Python emits the instruction bytes and ELF64 image; candidate arithmetic does not pass through C, an external assembler/linker, libc, libm, RefC, or LLVM.

The branch consumes the shared F32 complex/projective corpus and covers addition, multiplication, reciprocal/division, conjugation, magnitude, polynomial/rational evaluation, bounded complex exponential, Cartesian/polar observation, projective rescaling/non-equivalence, and affine/CP¹ chart behavior.

Polar operations here are observational. x87 `FPATAN`, `FSIN`, and `FCOS` are used for the polar/phase round trip, while the evolving complex arithmetic remains Cartesian scalar-SSE.

So the concrete current x86 choice is:

```text
core complex arithmetic: Cartesian F32 pair
working registers: xmm0/xmm1
polar conversion: explicit observational path
```

This implementation leads the current subsystem, but it does not redefine mathematical `Complex` as an SSE pair.

## ARMv7 Android Thumb-2: useful but explicitly provisional

The older ARM slice is real executable work, but its current policy status is important: **provisional and disposable**.

The temporary complex representation is:

```text
(magnitude, phaseTurns)
```

with two F32 words per logical complex value. `phaseTurns = 1` is one revolution, so dyadic turns such as `1/2`, `1/4`, and `1/8` are exactly representable at the phase-coordinate level in binary32.

For two complex operands, the tested softfp argument layout is:

```text
r0 = left.magnitude
r1 = left.phaseTurns
r2 = right.magnitude
r3 = right.phaseTurns
```

The self-test genuinely executes under QEMU:

```text
(2, 1/8 turn) × (3, 1/4 turn)
    =
(6, 3/8 turn)
```

The current emitter gives scalar locals stack homes and uses VFP scalar registers such as `s0` and `s1` for active F32 operations. Therefore the evidence is **not** a global pair-aware VFP register allocator.

There is also no general two-word complex return ABI. The experiment exposes magnitude and phase results as two scalar exported leaves.

PR #53 deliberately prevents this temporary design from becoming accidental architecture. Its receipt records:

```text
complex multiplication lowering      PASS
Thumb-2 assembly                     PASS
QEMU complex multiplication          PASS
shared Cartesian numerical corpus    SKIP
projective corpus                     SKIP
headless render                       SKIP
physical ARM device                   SKIP
```

This is the right interpretation of the ARM experiment: preserve the executable result and register grouping as evidence, but do not let it constrain later ARM, x86, GPU, or source-language design.

## AArch64 ICK/GCC: the major representation experiment that must not be lost

The AArch64 ICK work goes substantially deeper than the Thumb follower: it modifies GCC-family `_Complex` representation itself.

For floating `_Complex`, the implemented physical storage is:

```text
(modulus, argument)
```

while C semantic access through real/imaginary components is reconstructed where required.

Complex multiplication and division operate directly on the physical polar slots. Focused GCC tests cover polar layout, component stores/direct returns, and radial floor/ceil behavior.

The qualified compiler line builds an `aarch64-linux-gnu` cross compiler and checks real AArch64 objects. The focused work verifies static polar object bytes and complex multiplication and runs under qemu at `-O0` and `-O2`. The same isolated PIC object can be linked into an Android API 26 shared library with the NDK.

### Boundary with ordinary Android code

The safe consumption contract is intentionally narrow:

```text
inside ICK translation unit:
    _Complex may use ICK polar physical representation

boundary to Clang/NDK:
    ordinary scalar and pointer/array values only
```

Do **not** pass `_Complex` across the ICK/Clang boundary. Clang's Cartesian `_Complex` ABI is not assumed compatible with ICK's physical polar representation.

The representative bridge accepts Cartesian scalar inputs, constructs `_Complex` internally, performs complex operations in ICK, and returns Cartesian scalar/array outputs.

### Known representation limits

The ICK source explicitly records boundaries that matter numerically and semantically:

- partial or volatile bytewise views of complex storage are not generally qualified;
- arbitrary external functions accepting/returning `_Complex` are not qualified;
- the two-word polar representation does not preserve every ISO C `_Complex` distinction involving infinities, NaNs, or signed-zero quadrants;
- historical qualification exposed an `-O2` constant-representation/folding problem before later source consolidation.

These limitations are part of the result, not noise to omit from Strang.

## Wegert: actual Android consumption of the AArch64 experiment

Wegert merged an isolated ICK AArch64 complex-math object into the Android application path.

Its ABI split is informative:

```text
arm64-v8a:
    ICK-compiled internal _Complex arithmetic
    polar physical representation inside object
    Cartesian scalar/array API outside

armeabi-v7a:
    ordinary Cartesian fallback

x86_64:
    ordinary Cartesian fallback

GLES upload/storage:
    Cartesian
```

The application uses the ICK object for coefficient expansion while keeping `_Complex` entirely behind the object boundary. CI verifies the pinned object and AArch64 machine type and builds all supported Android ABIs.

A later open Wegert PR adds a stronger source-built qualification lane: rebuild ICK from the qualified source commit, rebuild the actual complex object, require its hash to match, execute the math boundary under qemu-aarch64, then build the APK using the freshly generated object.

This application evidence is different from the Idriç x86-leader hierarchy, but it is precisely the kind of backend/representation knowledge that Strang should preserve.

## GLSL ES: Cartesian logical pair, physical registers unknown

The complex/projective shader follower uses:

```text
SVec 2
```

as one Cartesian complex scalar, lowering naturally to GLSL ES `vec2`.

That proves a logical target representation. It does **not** prove a PowerVR USC register pair, Mali register allocation, Adreno packing, or any other vendor physical layout. The driver compiler can scalarize, pack, fuse, spill, or rearrange it.

The evidence chain is intentionally staged:

```text
typed IR generated          PASS
GLSL ES generated           PASS
shader validated            PASS
program linked              PASS
shader loaded by driver     SKIP in generic follower lane
GPU executed                SKIP
framebuffer captured        SKIP
vendor/device receipt       SKIP
```

A later target can strengthen these stages without changing complex semantics.

## Precision is independent of complex coordinate form

The shader work separately tracks numeric width:

```text
F32 -> highp float / highp vecN
F16 -> distinct semantic width
```

F32→F16 demotion is not implicit. Portable GLSL ES `mediump` is a precision/range guarantee, not a universal promise of exact binary16 storage. PowerVR-specific F16 claims need target evidence.

The earlier Float16 integration work also exposed a real compiler/API boundary: source `Float16` support can succeed in the compiler while still fail later because the shader source/signature/lowering layer only accepts the older scalar profile. That distinction belongs in the evidence trail rather than being summarized as “FP16 unsupported.”

## Cartesian versus polar is an algorithmic choice, not merely storage syntax

The existing work now gives at least three useful cases:

```text
x86 Idriç leader          Cartesian F32
Thumb provisional slice   polar F32 turns
AArch64 ICK/GCC           polar physical _Complex storage
shader follower           Cartesian vec2
```

Polar form makes multiplication/division and roots structurally attractive:

```text
(ρ₁, θ₁)(ρ₂, θ₂) = (ρ₁ρ₂, θ₁+θ₂)
(ρ₁, θ₁)/(ρ₂, θ₂) = (ρ₁/ρ₂, θ₁−θ₂)
```

Cartesian form makes addition/subtraction and polynomial evaluation direct.

Conversion is real numerical work:

```text
Cartesian -> Polar
    magnitude = hypot(real, imag)
    phase = atan2(imag, real)

Polar -> Cartesian
    real = magnitude cos(phase)
    imag = magnitude sin(phase)
```

A later representation planner should therefore consider operation mix, conversion count, precision, exceptional-value semantics, phase conventions, register pressure, spilling, vector packing, and target instructions.

## What the compiler should preserve long enough to exploit

The useful lowering metadata is not simply `Complex = pair`.

A machine-level complex value may need to retain:

```text
semantic complex identity
coordinate form: Cartesian | Polar | other
scalar width
component order
component locations / register class
calling convention
memory layout
conversion provenance
exceptional-value contract
projective / holomorphic context when relevant
```

For register allocation, pair identity can matter even when the two components ultimately occupy separate scalar registers: keep/spill/reload decisions, scratch selection, vector packing, and partial-component uses should not need to rediscover that the values belong together.

## Claims that are not currently justified

Do not promote any of these beyond the evidence:

- Thumb polar F32 is **not** the chosen general ARM complex representation; current policy calls it provisional/disposable.
- Thumb has no proven global pair-aware VFP allocator and no general complex return ABI.
- The x86 SSE pair is the current executable leader for the Idriç complex/projective subsystem, not a universal representation decision.
- ICK AArch64 polar `_Complex` is real implemented compiler work, but it has explicit ABI and exceptional-value limits and is not the canonical Idriç representation.
- A GLSL `vec2` is not evidence of a physical GPU register layout.
- `mediump` is not automatically binary16.
- shader compilation/link is not GPU execution.
- none of these representations defines ℂ itself.

## Source trail

### Canonical semantics / policy

- `isomorphisms/Idric` PR #81 — complex/projective semantic boundary and shared F32 corpus
- `isomorphisms/ai-ci` PR #79 — merged implementation hierarchy and fail-closed receipt policy

### x86-64 leader

- `isomorphisms/idric-x86-aggressive-backend` PR #22
- branch `x86/complex-projective-leader`
- head `71322966574d2589bcf07ac012cea878595b887c`
- `backend/complex_projective.py`

### ARMv7 Thumb follower

- `isomorphisms/idric-arm-thumb` executable base branch `polar-complex-first`
- base head `e0f23d1a5b3977f1c55da85b530049118cb4f7f6`
- `isomorphisms/idric-arm-thumb` PR #53 — current provisional/disposable follower status
- follower head `cb08fd55dec00bfbc3a6020218a7a7d78cc63682`

### AArch64 ICK/GCC

- `isomorphisms/rhs` PR #1 — merged **Build ICK directly for AArch64**
- implementation head `7458b3c29fe535eb7dda3b1c756b362cee5c889d`
- merge `5fe6f6d1259b0b4ae9adf99d354e49e2a01afbf9`

### Android application consumption

- `isomorphismes/wegert` PR #13 — merged ICK arm64 complex-math integration with Cartesian fallbacks
- `isomorphismes/wegert` PR #31 — open source-built ICK/APK qualification lane

### GLSL ES / GPU follower

- `isomorphisms/idris-shader-backend` PR #36
- branch `complex-projective/follower`
- head `4360f29d3bbdc09b4cc513e4a6be647f290ffede`

### Precision / target evidence

- `isomorphisms/idris-shader-backend`, `docs/float-semantics.md`
- PowerVR target branch `target/powervr-ge8322-gles`
- `isomorphisms/ai-ci` PR #45 and `isomorphismes/wegert` PR #29 for the staged Float16/shader-API integration boundary

## Related Strang notes

- [`type-directed-numerical-linear-algebra.md`](type-directed-numerical-linear-algebra.md)
- [`complex-projective-type-system.md`](complex-projective-type-system.md)
- [`geometry-conditioning-and-factorization-choice.md`](geometry-conditioning-and-factorization-choice.md)
- [`trefethen-bau-complex-change-of-basis.md`](trefethen-bau-complex-change-of-basis.md)
