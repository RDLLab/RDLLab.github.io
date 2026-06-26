---
layout: page
title: "Vectorized Online POMDP Planning (VOPP)"
center_title: true
description: "VOPP: a fully GPU-parallelized online POMDP solver that is at least 20× faster than the state-of-the-art parallel solver, using pure tensor operations with no synchronization overhead."
img: assets/img/project_img/vopp/vopp_overview.png
importance: 11
category: active
do_not_show_post_desc: true
---

<!--h4>Marcus Hoerger, Muhammad Sudrajat, Hanna Kurniawati</h4-->
<div class="text-center mb-3">
  <p class="text-muted" style="font-size: 1.35rem; margin-bottom: 0;">
    Marcus Hoerger, Muhammad Sudrajat, Hanna Kurniawati
  </p>
</div>

<div class="text-center mb-3">
  <span class="badge rounded-pill bg-primary" style="font-size: 1rem; padding: 0.6rem 1rem;">
    ICRA 2026
  </span>
</div>

<div class="text-center mt-3 mb-4">
  <a href="/assets/pdf/papers/icra26_VOPP.pdf" target="_blank" class="btn btn-lg btn-outline-primary mx-2">
    <i class="fas fa-file-pdf"></i> Paper
  </a>
  
  <a href="https://arxiv.org/abs/2510.27191" target="_blank" class="btn btn-lg btn-outline-secondary mx-2">
    <i class="ai ai-arxiv"></i> arXiv
  </a>

  <a href="https://github.com/RDLLab/VOPP" target="_blank" class="btn btn-lg btn-outline-dark mx-2">
    <i class="fab fa-github"></i> Code
  </a>
  
  <a href="/assets/img/project_img/vopp/icra2026_vopp_poster.pdf" target="_blank" class="btn btn-lg btn-outline-success mx-2">
    <i class="fas fa-image"></i> Poster
  </a>
</div>


<!--h2>Abstract</h2-->

<p class="text-justify">
<strong>Vectorized Online POMDP Planner (VOPP)</strong> is a fully vectorized online POMDP solver that performs planning entirely through batched tensor operations. The Partially Observable Markov Decision Process (POMDP) is a powerful framework for sequential decision-making under uncertainty, capturing the stochastic effects of actions and the noisy, partial information available to autonomous robots. POMDP solving could benefit enormously from massive parallelization on modern multicore hardware, but parallelizing online POMDP solvers has remained difficult: most approaches interleave numerical optimization to find actions with the highest expected total reward and estimation of expected total rewards themselves. This creates dependencies and synchronization bottlenecks during planning that can quickly diminish the gains of parallelization.
</p>

<p class="text-justify">
VOPP takes a fundamentally different approach. It builds on a recent POMDP formulation &mdash; <strong><a href="/projects/7_project_rdl/">Partially Observable Reference Policy Programming (PORPP)</a></strong> &mdash; which introduces <strong>analytical</strong> value functions. This eliminates the need for numerical optimization during planning, leaving only the estimation of expectations to be performed numerically, which can be parallelized efficiently. VOPP exploits this by implementing all online planning operations as fully vectorized computations over a tensor-based planning data structure. The result is a massively parallel online POMDP solver that fully harnesses the immense data-parallel throughput of modern multicore hardware, such as GPUs. In practice, VOPP computes policies using tens of thousands of parallel simulations with no explicit synchronization between simulations required.
</p>

<div class="row justify-content-center mt-3 mb-4">
  <div class="col-sm-10">
    {% include figure.liquid
      loading="eager"
      path="assets/img/project_img/vopp/vopp_overview.png"
      title="Overview of VOPP"
      class="img-fluid rounded z-depth-1" %}
    <div class="caption">
      Overview of the Vectorized Online POMDP Planner (VOPP).
    </div>
  </div>
</div>

<p class="text-justify">
Experimental results on three POMDP benchmark problems indicate that VOPP is <strong>at least 20&times; more efficient</strong> in
computing near-optimal policies compared to a state-of-the-art parallel
online POMDP solver. For some benchmarks, VOPP is more than <strong>100&times; faster</strong>. Furthermore, VOPP outperforms state-of-the-art sequential online solvers while using
a planning budget that is <strong>1000&times; smaller</strong>.
</p>

<h2>Key Idea: Planning as Batched Tensor Computations</h2>

<div class="row justify-content-center mt-3 mb-4">
  <div class="col-sm-10">
    {% include figure.liquid
      loading="eager"
      path="assets/img/project_img/vopp/belief_tree.png"
      title="Overview of VOPP"
      class="img-fluid" %}
    <div class="caption">
      Traditional online POMDP solvers (left) represent belief trees as linked node structures that are traversed and expanded sequentially. VOPP (right) represents belief trees as a collection of tensors, enabling planning to be formulated as batched tensor operations over this representation.
    </div>
  </div>
</div>

<p class="text-justify">
Similar to most online POMDP solvers, VOPP performs guided belief space sampling followed by backup operations to construct a belief tree and evaluate different action sequences. However, unlike existing solvers, VOPP represents a belief tree as a collection of tensors $\{B, A, \Psi\}$ with belief node tensor $B$, action node tensor $A$ and action-preference tensor $\Psi$. Using this tensor representation, VOPP iterates the following steps during planning:

<ul>
	<li><em>Vectorized forward search</em>: Starting from the currently belief, VOPP performs tens of thousands of batched forward simulations in parallel to expand the belief tree</li>
	<li><em>Vectorized preference backup</em>: Starting from <em>all</em> leaf nodes, VOPP traverses the belief tree back to the root, updating the action preferences and belief values of all nodes at each depth using batched tensor operations.</li>
</ul>

Together, these two vectorized operations transform online POMDP planning from sequential tree traversal into a sequence of batched tensor computations, eliminating the synchronization bottlenecks of existing parallel solvers and fully exploiting the massive parallelism of modern multicore hardware, including GPUs.

<div class="row justify-content-center mt-3 mb-4">
  <div class="col-sm-10">
    {% include figure.liquid
      loading="eager"
      path="assets/img/project_img/vopp/vopp_forward_search.png"
      title="Overview of VOPP"
      class="img-fluid" %}
    <div class="caption">
      VOPP uses fully vectorized forward simulations during search to expand the belief tree.
    </div>
  </div>
</div>


<h2>Benchmark Problems</h2>

<p class="text-justify">
We evaluated VOPP on three challenging planning-under-uncertainty problems:
</p>

<ul>
  <li><strong>Navigation in a partially known map</strong> — a robot navigates a 13&times;13 grid with randomly placed obstacles visible only through noisy local sensing. State space size: 169&times;2<sup>124</sup>.</li>
  <li><strong>Multi-Agent Rocksample (MARS)</strong> — two cooperative rovers on a <em>n&times;n</em> map sample good rocks while avoiding bad ones. The quality of rocks must be inferred through noisy sensor readings. MARS(50,50) has an action space of 3025 actions and a state space too large for HyP-DESPOT and POMCP to handle.</li>
  <li><strong>CrowdNav</strong> — a Stretch 3 mobile robot navigates a 50&times;40m conference hall populated by 300 people whose behavior (curious or shy) is initially unknown and must be inferred online.</li>
</ul>

<div class="row mt-3">
  <div class="col-sm-4 mt-3 mt-md-0 text-center">
    {% include figure.liquid
      loading="eager"
      path="assets/img/project_img/vopp/unc_navigation_2.png"
      title="Navigation in a partially known map"
      class="rounded z-depth-1"
      height="200"
      width="auto"
    %}
    <div class="caption">Navigation in a partially known map</div>
  </div>

  <div class="col-sm-4 mt-3 mt-md-0 text-center">
    {% include figure.liquid
      loading="eager"
      path="assets/img/project_img/vopp/ma_rocksample_2.png"
      title="Multi-Agent Rocksample (MARS)"
      class="rounded z-depth-1"
      height="200"
      width="auto"
    %}
    <div class="caption">Multi-Agent Rocksample (MARS)</div>
  </div>

  <div class="col-sm-4 mt-3 mt-md-0 text-center">
    {% include figure.liquid
      loading="eager"
      path="assets/img/project_img/vopp/crowd_nav_0.png"
      title="CrowdNav"
      class="rounded z-depth-1"
      height="200"
      width="auto"
    %}
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

<h2>References</h2>

<div class="publications">
  {% bibliography --file project_11.bib %}
</div>
