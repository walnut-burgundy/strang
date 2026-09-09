# David Mumford — Pattern Theory

Source notes for the mathematical ideas in David Mumford's pattern-theory work, with the bibliography preserved from the 2002 ICM paper.

## Primary sources

- David Mumford, **“Pattern Theory: the Mathematics of Perception”**, *Proceedings of the International Congress of Mathematicians, Beijing 2002*, pp. 401–422.
  - Brown archive PDF: https://www.dam.brown.edu/people/mumford/vision/papers/2002c--ICM02proceedings-IMU.pdf
  - Mumford's Pattern Theory archive page: https://www.dam.brown.edu/people/mumford/vision/pattern.html
- David Mumford and Agnès Desolneux, **Pattern Theory: The Stochastic Analysis of Real-World Signals**, A K Peters/CRC Press, 2010.

Accessed 2026-09-09.

Mumford's Brown archive describes Pattern Theory as an approach developed from Ulf Grenander's program: construct stochastic models of signals and situations, use Bayesian inference for analysis, and test the model by synthesizing samples from it. The archive also emphasizes graphical structure and non-Gaussian variability as central features of real signals.

## Main mathematical ideas from the 2002 notes

### 1. Perception is fundamentally statistical

Mumford contrasts logical/rule-based approaches with statistical inference. Real sensory data are incomplete, noisy, cluttered, and ambiguous. The aim is not to infer from perfect measurements but to combine uncertain evidence under a stochastic model.

A recurring point is that probability does not merely represent irreducible physical randomness. It is also a compact way to model enormous numbers of unobserved or irrelevant variables.

### 2. Bayesian setup

Split the variables into observed and hidden parts:

- `x_o`: observed signal;
- `x_h`: hidden causes, objects, states, labels, etc.;
- `θ`: model parameters.

The model factors as

```text
Pr(x | θ) = Pr(x_o | x_h, θ) Pr(x_h | θ)
```

where the first factor is the observation/imaging model and the second is the prior.

The three broad problems are:

1. choose/craft a class of stochastic models;
2. estimate the parameters from data;
3. infer hidden variables from observations by the posterior

```text
Pr(x_h | x_o, θ) ∝ Pr(x_o | x_h, θ) Pr(x_h | θ).
```

Mumford explicitly treats **synthesis** as a model check: sample from the model and inspect whether the samples reproduce the structures seen in real data. This is stronger than matching a few summary statistics.

### 3. HMMs, speech, and dynamic programming

Hidden Markov models are used as the first concrete example. A sequence of hidden states evolves with local transition probabilities and emits observations. The linear graph permits forward/backward recursion and dynamic programming.

The useful lesson is structural: inference becomes tractable when conditional-independence structure makes a huge probability model decomposable.

The limitation is equally important. Real processes are not truly Markov at a small fixed state description. Speech depends on words, grammar, semantics, and context on many scales. Enlarging the state can make the label space explode.

This motivates richer graphical and compositional models rather than assuming that a small HMM state is the whole explanation.

### 4. Continuous and discrete structure occur together

Observed signals are often continuous-valued, while hidden causes contain discrete events and objects.

Mumford emphasizes heavy-tailed, non-Gaussian statistics of natural signals. In particular, changes in many natural signals have kurtosis greater than the Gaussian value 3. This is treated as evidence that smooth-looking continuous signals are driven by intermittent discrete events or structures.

Examples discussed include image intensities, speech power, and log price changes.

### 5. Particle filtering

When exact state enumeration is impossible, approximate a posterior distribution by a finite weighted sample:

```text
Pr(x_k | observations) ≈ Σ_i w_i δ_{x_i}.
```

The conceptual point is that the approximation attempts to retain a **distribution**, including multimodality, rather than collapsing immediately to a single best estimate.

### 6. Grammars and compositional structure

Probabilistic context-free grammars give a branching-tree version of conditional independence. But real linguistic and visual constraints can require information to propagate across the tree, producing an explosion of labels if everything is encoded in the state.

Mumford points toward unification/compositional grammars as a broader class. The important pattern-theory idea is that **objects and interpretations are built compositionally**, with relations between parts, rather than represented as one flat feature vector.

### 7. Markov random fields and graphical models

For less sequential problems, the natural generalization is a graph whose vertices carry random variables and whose edges/local cliques encode dependencies.

This connects directly to Gibbs distributions and statistical mechanics. Important computational ideas in the notes include:

- Gibbs sampling;
- simulated annealing;
- mean-field approximation;
- Bayesian belief propagation;
- the Bethe approximation;
- graph cuts / segmentation methods and multiscale methods.

The broad idea is that the graph is not decoration: it is the mathematical representation of which parts of the problem interact directly.

### 8. Continuum limits: denoising and segmentation

Mumford then moves from discrete graphical models to continuous images.

One route is variational: approximate an observed image by a piecewise-smooth image while penalizing disagreement, roughness away from edges, and total edge length. This is the Mumford–Shah style of model.

Another route is nonlinear diffusion, treating image restoration as an evolution equation / gradient-flow problem.

The shift from discrete variables to continuum models opens the problem to analysis, PDEs, geometric measure theory, and calculus of variations.

### 9. Scale invariance and natural-image statistics

Natural-image statistics exhibit approximate power laws and scale invariance. Mumford formulates a continuum idealization in which the image is a random generalized function modulo constants, with a probability law invariant under translations and scalings.

A key warning is that ordinary functions are too restrictive for an exactly translation-and-scale-invariant random field; generalized functions/distributions are required.

The target statistical theory should simultaneously account for:

- translation and scale invariance;
- heavy-tailed filter responses / kurtosis greater than 3;
- preferred local geometries such as edges, corners, bars, junctions, and relatively featureless patches.

### 10. Generative image models

The notes discuss two families:

- random wavelet expansions / infinitely divisible models;
- occlusion or “dead leaves” models.

The second is important because it introduces hidden discrete objects and depth/occlusion rather than treating the image as a simple additive superposition. It therefore captures a genuinely compositional property of scenes.

### 11. Shape as geometry on transformation groups

For shape variation, Mumford follows Grenander's idea of modeling deformation through diffeomorphisms.

The space of diffeomorphisms is treated as an infinite-dimensional Riemannian manifold. Arnold's interpretation of incompressible Euler flow as geodesic motion on the volume-preserving diffeomorphism group supplies the prototype.

Replacing the weak metric by one defined using a positive self-adjoint differential operator leads to a template-matching equation and useful quotient spaces for landmarks and shapes.

This turns “similarity of shape” into geometry: distances, geodesics, stochastic motion, and probability distributions on spaces of deformations.

### 12. Pattern Theory as a mathematical program

The paper is not one algorithm. It is a program joining:

- probability and Bayesian inference;
- graphical models;
- information/statistical learning ideas;
- stochastic processes;
- statistical mechanics;
- variational methods and PDEs;
- harmonic/multiscale analysis;
- differential and infinite-dimensional geometry;
- computation and approximation algorithms.

The repeated methodological rule is useful beyond perception: **model enough structure that samples from the model resemble the world, and make the hidden structure explicit enough that inference is possible.**

## Other Mumford Pattern Theory items listed on his archive

Mumford's Pattern Theory page lists the following related works:

- **“Parametrizing Exemplars of Categories”**, *Journal of Cognitive Neuroscience* 3 (1991), pp. 87–88.
- Commentary on Grenander & Miller, **“Representations of Knowledge in Complex Systems”**, *Proceedings of the Royal Statistical Society* (1994).
- **“Pattern Theory: a Unifying Perspective”**, *Proceedings of the 1st European Congress of Mathematics*, Birkhäuser-Boston (1994); revised in D. Knill and W. Richards (eds.), *Perception as Bayesian Inference*, Cambridge University Press (1996), pp. 25–62.
- **“The Bayesian Rationale for Energy Functionals”**, in Bart ter Haar Romeny (ed.), *Geometry-Driven Diffusion in Computer Vision*, Kluwer Academic (1994), pp. 141–153.
- Peter Giblin, Gaile Gordon, Peter Hallinan, David Mumford, and Alan Yuille, **Two and Three Dimensional Patterns of the Face**, A K Peters (1999).
- **“Pattern Theory: The Mathematics of Perception”**, ICM 2002, pp. 401–422.
- David Mumford and Agnès Desolneux, **Pattern Theory: The Stochastic Analysis of Real-World Signals**, A K Peters/CRC Press (2010).

## Bibliography copied from Mumford's 2002 ICM paper

The entries below are transcribed from the paper's bibliography. Source abbreviations, incomplete publication details, and apparent source typos are intentionally retained rather than silently repaired.

- **[B-G-P]** E Bienenstock, S Geman and D Potter, *Compositionality, MDL priors and object recognition*, Adv. in Neural Information Proc., Mozer, Jordan and Petsche ed., 9, MIT Press, 1998.
- **[C-R-M]** G Christensen, RD Rabbitt and M Miller, *3D brain mapping using a deformable neuroanatomy*, Physics in Med. and Biol., 39, 1994.
- **[D-F-G]** A Doucet, N. de Freitas and N Gordon editors, *Sequential Monte Carlo Methods in Practice*, Springer, 2001.
- **[D-G-M]** P Dupuis, U Grenander and M Miller, *Variational problems on flows of diffeomorphisms for image matching*, Quarterly Appl. Math., 56, 1998.
- **[G]** U Grenander, *General Pattern Theory*, Oxford Univ. Press, 1993.
- **[G-G]** S Geman and D Geman, *Stochastic relaxation, Gibbs distr. and Bayesian restoration of images*, PAMI = IEEE Trans. Patt. Anal. Mach. Int., 6, 1984.
- **[Gr-M]** U Grenander and M Miller, *Computational anatomy*, Quart. of Appl. Math., 56, 1998.
- **[Gu-M]** F Guichard and J-M Morel, *Image Anal. and Partial Diff. Equations*, http://www.ipam.ucla.edu/publications/gbm2001/gbmtut_jmorel.pdf.
- **[G-S-S]** N Gordon, D Salmond and A Smith, *Novel approach to nonlinear/non-Gaussian Bayesian state estimation*, IEE Proc.-F, 140, 1993.
- **[H]** L.C.G. Haggerty, *What and two-and-a-half year old child said in one day*, J. Genet. Psych., 37, 1930.
- **[I-B]** M Isard and A Blake, *Contour tracking by stochastic propagation of conditional density*, Eur. Conf. Comp. Vis., 1996.
- **[L]** P Lieberman, *The Biology and Evolution of Language*, Harvard, 1984.
- **[L-M-H]** A Lee, D Mumford and J Huang, *An occlusion model for natural images*, Int. J. Comp. Vis., 41, 2001.
- **[L-P-M]** A Lee, K Pederson and D Mumford, *The non-linear statistics of high-contrast patches in natural images*, to appear, Int. J. Comp. Vis..
- **[M]** D Mumford, *The Dawning of the Age of Stochasticity*, in *Mathematics, Frontiers and Perspectives*, ed. by Arnold, Atiyah, Lax and Mazur, AMS, 2000.
- **[M-G]** D Mumford and B Gidas, *Stochastic models for generic images*, Quarterly of Appl. Math., 59, 2001.
- **[M-S]** J-M Morel and S Solimini, *Variational Methods in Image Segmentation*, Birkhauser, 1995.
- **[P]** J Pearl, *Probabilistic Reasoning in Int. Systems*, Morgan-Kaufmann, 1997.
- **[P-B]** J Puzicha and J Buhmann, *Multiscale annealing for grouping and texture segmentation*, Comp. Vis. and Image Understanding, 76, 1999.
- **[Sh]** S Shieber, *Constraint-Based Grammar Formalisms*, MIT Press, 1992.
- **[Sm]** C Small, *The Statistical Theory of Shape*, Springer, 1996.
- **[S-B-B]** E Sharon, A Brandt and R Basri, *Segmentation and boundary detection using multiscale intensity measurements*, Proc IEEE Conf. Comp. Vis. Patt. Recognition, Hawaii, 2001.
- **[S-M]** J Shi and J Malik, *Normalized cuts and image segmentation*, PAMI, 22, 2000.
- **[T-Z]** -W Tu and S-C Zhu, *Image segmentation by data driven Markov chain Monte Carlo*, PAMI, 24, 2002.
- **[V]** V Vapnik, *The Nature of Statistical Learning Theory*, Springer, 199?.
- **[W]** R.M. Warren, *Restoration of missing speech sounds*, Science, 167, 1970.
- **[Yi]** N-K Yip, *Stoch. motion by mean curv.*, Arch. Rat. Mech. Anal., 144, 1998.
- **[Yo]** L Younes, *Invariance, Déformations et Reconnaissance de Formes*, to appear.
- **[Y-F-W]** J Yedidia, W Freeman, and Y Weiss, *Generalized Belief Propagation*, Adv. in Neural Information Proc., edited by Leen, Dietterich, Tresp, 13, 2001.
- **[Z-Y]** S-C Zhu and A Yuille, *Region competition*, PAMI, 18, 1996.

## Source handling note

The note above is a summary, not a reproduction of the paper. The bibliography is copied because the citation trail is itself part of the research record. Mumford's Brown archive states that his site content is available under Creative Commons Attribution-NonCommercial-ShareAlike 3.0 Unported.
