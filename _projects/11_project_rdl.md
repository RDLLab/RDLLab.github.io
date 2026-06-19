---
layout: page
title: "Vectorized Online POMDP Planning (VOPP)"
description: "VOPP: a fully GPU-parallelized online POMDP solver that is at least 20× faster than the state-of-the-art parallel solver, using pure tensor operations with no synchronization overhead."
img: assets/img/project_img/vopp/ma_rocksample_2.png
importance: 11
category: active
do_not_show_post_desc: true
---

<h2>Abstract</h2>

<p class="text-justify">
We present the <strong>Vectorized Online POMDP Planner (VOPP)</strong>, a fully GPU-parallelized online solver for planning under partial observability in robotics. The Partially Observable Markov Decision Process (POMDP) is a powerful framework for sequential decision-making under uncertainty, capturing the stochastic effects of actions and the noisy, partial information available to autonomous robots. POMDP solving could benefit enormously from massive parallelization on modern hardware, but parallelizing online POMDP solvers has remained difficult: most approaches interleave numerical optimization over actions with value estimation, creating data dependencies and synchronization bottlenecks that erode the gains of parallel execution.
</p>

<p class="text-justify">
VOPP overcomes this by building on a recent POMDP formulation — <strong>Partially Observable Reference Policy Programming (PORPP)</strong> — that analytically solves part of the optimization, reducing the remaining numerical work to the estimation of expectations alone. This lets VOPP express every step of online planning as batched tensor operations under the SIMD paradigm of GPUs, eliminating the action-value interleaving that limits prior parallel solvers. The result is a massively parallel online solver running entirely on the GPU, using tens of thousands of parallel simulations to compute a policy with no explicit synchronization between simulations required.
</p>

<p class="text-justify">
Experimental results on three POMDP benchmark problems —
<strong>Multi-Agent Rocksample (MARS)</strong>,
<strong>Navigation in a partially known map</strong>, and
<strong>CrowdNav</strong> — indicate that VOPP is <strong>at least 20&times; more efficient</strong> in
computing near-optimal policies compared to HyP-DESPOT, the current state-of-the-art parallel
online POMDP solver. For some benchmarks, VOPP is more than <strong>100&times; faster</strong> than
HyP-DESPOT. Furthermore, VOPP outperforms state-of-the-art sequential online solvers while using
a planning budget that is <strong>1000&times; smaller</strong>.
</p>

<h2>Key Idea: Planning as Pure Tensor Computation</h2>

<p class="text-justify">
Most parallel online POMDP solvers build on Monte Carlo Tree Search (MCTS), which interleaves
action selection (requiring maximization or UCB scores over action values) with value estimation
(forward simulation and backup). This interleaving creates data dependencies between concurrent
processes, forcing synchronization that offsets the benefits of GPU parallelism.
</p>

<p class="text-justify">
VOPP takes a different route. It builds on PORPP, a POMDP formulation that casts the Bellman
optimality objective as a log-sum-exp over action preferences. This analytical treatment of the
optimization step means the only remaining numerical work is estimating expectations — a
naturally data-parallel operation. VOPP represents the entire belief tree
<strong>&Tau; = {B, A, &Psi;}</strong> as a collection of three 2D tensors (beliefs, actions,
and preferences), and implements both the forward search and the preference backup as single
batched tensor operations over this representation.
</p>

<h2>Benchmark Problems</h2>

<p class="text-justify">
We evaluated VOPP on three challenging planning-under-uncertainty problems:
</p>

<ul>
  <li><strong>Multi-Agent Rocksample (MARS)</strong> — two cooperative agents on an <em>n&times;n</em> map sample good rocks while avoiding bad ones. MARS(50,50) has an action space of 3025 actions and a state space too large for HyP-DESPOT and POMCP to handle.</li>
  <li><strong>Navigation in a partially known map</strong> — a robot navigates a 13&times;13 grid with randomly placed obstacles visible only through noisy local sensing. State space size: 169&times;2<sup>124</sup>.</li>
  <li><strong>CrowdNav</strong> — a Stretch 3 mobile robot navigates a 50&times;40m conference hall populated by 300 people whose behavior (curious or shy) is initially unknown and must be inferred online.</li>
</ul>

<div class="row mt-3">
  <div class="col-sm-4 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/project_img/vopp/unc_navigation_2.png" title="Navigation in a partially known map" class="img-fluid rounded z-depth-1" %}
    <div class="caption">Navigation in a partially known map</div>
  </div>
  <div class="col-sm-4 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/project_img/vopp/ma_rocksample_2.png" title="Multi-Agent Rocksample (MARS)" class="img-fluid rounded z-depth-1" %}
    <div class="caption">Multi-Agent Rocksample (MARS)</div>
  </div>
  <div class="col-sm-4 mt-3 mt-md-0">
    {% include figure.liquid loading="eager" path="assets/img/project_img/vopp/crowd_nav_0.png" title="CrowdNav" class="img-fluid rounded z-depth-1" %}
    <div class="caption">CrowdNav</div>
  </div>
</div>

<h2>Results</h2>

<p class="text-justify">
VOPP was compared against HyP-DESPOT (the state-of-the-art parallel online solver), DESPOT, and POMCP on MARS and Navigation, across planning budgets of 0.01s to 1.0s per step.
</p>

<ul>  
  <li>On <strong>Navigation</strong>, VOPP achieves a success rate of <strong>94%</strong> with an average of <strong>19.8 steps</strong> to reach the goal, compared to HyP-DESPOT's 77% success rate and 26.8 steps at a 1s/step budget.</li>
  <li>On <strong>MARS(20,20)</strong>, VOPP achieves an average total discounted reward of <strong>58.8$\pm$2.1</strong> at 1s/step, compared to 47.9$\pm$1.6 for HyP-DESPOT.</li>
  <li>On <strong>MARS(50,50)</strong> (3025 actions), VOPP achieves <strong>45.1$\pm$2.0</strong> at 1s/step. HyP-DESPOT, DESPOT, and POMCP could not run this variant (implementation crashed for sizes larger than MARS(36,36)).</li>
  <li>At a planning budget of <strong>0.05s/step</strong>, VOPP achieves a better result than HyP-DESPOT running at <strong>1s/step</strong> —  demonstrating an efficiency gain of at least 20&times;. (At a 0.01s/step budget, VOPP achieves ~64% of what HyP-DESPOT achieves with 1s/step ).</li>
  <li>Against sequential solvers (DESPOT, POMCP) running at 10s/step, VOPP with 0.01s/step still achieves better results — a <strong>1000&times; smaller</strong> planning budget.</li>
</ul>

<p class="text-justify">
VOPP will be released as open source software.
</p>

<h2>Paper</h2>

<p>
  M. Hoerger, M. Sudrajat, H. Kurniawati. <em>Vectorized Online POMDP Planning.</em>
  IEEE International Conference on Robotics and Automation (ICRA), 2026.
  <a target="_blank" href="/assets/pdf/papers/icra26_VOPP.pdf">[PDF]</a>
</p>

<h2>References</h2>

<div class="publications">
  {% bibliography --file project_11.bib %}
</div>

<h2>People</h2>
<ul>
  <li>Marcus Hoerger</li>
  <li>Muhammad Sudrajat</li>
  <li>Hanna Kurniawati</li>
</ul>
