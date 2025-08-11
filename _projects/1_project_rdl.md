---
layout: page
title: POMDP Planning for Problems with Complex Dynamics
description:
img: assets/img/project_img/complex_dynamics/thumb-pomdp-complex-dynamics.png
importance: 1
category: past project
do_not_show_post_desc: true
---

<p class="text-justify">
Many POMDP (Partially Observable Markov Decision Process) solvers that can compute good approximate solutions on-line have been proposed. Some of them can even find good solutions in near real-time for several realistic problems. Despite these advances, they perform poorly when the dynamics of the POMDP agent is non-linear and complex (i.e., has no closed-form solution). To compute a strategy, most state-of-the-art on-line solvers rely on a large number of forward simulations to evaluate sequences of actions from different beliefs. For robots with complex non-linear dynamics where even a single forward simulation requires expensive numerical integrations, this strategy quickly becomes infeasible, as indicated in the results
</p>

{% include figure.liquid loading="eager" path="assets/img/project_img/complex_dynamics/fig1.png" title="problem in compled dynamix" class="img-fluid rounded z-depth-1" %}


<div class="caption">
    (a) The Rocksample problem with simple and cheap transition dynamics. (b) The KukaOffice problem with complex and expensive non-linear transition dynamics. (c) The cost of forward-simulations in both problems (in percentage of the total planning cost).
</div>

<p class="text-justify">
In this project, we aim to make on-line POMDP solvers more tractable for systems with complex non-linear dynamics. Our methods are motivated by two key insights:
</p>

<ol type="1">
<li>Using cheaper, less accurate dynamic models is often sufficient to compute good policies.</li>
<li>Coarse, low-fidelity models can be combined with accurate, high-fidelity models to reduce the cost of sampling while maintaining correctness.</li>
</ol>

<p class="text-justify">
Based on these insights we show how systematic approximations of complex, non-linear transition dynamics can be used to design on-line POMDP solvers that are more efficient than current state-of-the-art solvers.
</p>

<h2>
Problem simplification via linearization
</h2> 

<p class="text-justify">
Linearization is a common practice in solving non-linear control and estimation problems. However, it is known that linearization only works well for systems with mild non-linearities and small uncertainties. Additionally, it is not clear how linearized models affect the quality of the computed policy, and more importantly, when such a simplification is admissible. To answer these questions, we developed a novel measure of non-linearty for stochastic systems called <strong>Statistical Distance-based Non-linearity measure (SNM)</strong>.

Intuitively, SNM is based on the distance between the distributions that represent the original motion-sensing models and their linearized version. We showed that the value difference of the optimal policy for the original model and the optimal policy for the linearized model can be upper-bounded by a function that is linear in SNM. Furthermore, extensive experimental evaluations indicate that SNM is more effective in measuring the effects obstacles have on non-linearity of a system compared to an extisting state-of-the-art measure of non-linearity.

We then developed an on-line POMDP solver, called <strong>SNM-Planner</strong>, that uses SNM as a heuristic to decide when a linearization-based should be used for the policy computation and when a general solver should be used. We tested SNM-Planner on a number of motion-planning problems under uncertainty involving robots with non-linear transition dynamics. By combining a general solver and a linearization-based solver, SNM-Planner can compute more robust policies compared to each of the component solvers alone.

Details of SNM and SNM-Planner results can be viewed <a target="_blank" href="/assets/pdf/papers/wafr16_linearization.pdf" >here</a>.
</p>

<h2>
Multilevel Monte-Carlo applied to POMDP planning
</h2>

{% include figure.liquid loading="eager" path="assets/img/project_img/complex_dynamics/fig2.png" title="example scenario for POMDP evaluation" class="img-fluid rounded z-depth-1" %}

<div class="caption">
Test scenarios used to evaluate MLPP
</div> 

<p class="text-justify">
Inspired by our second key insight, we developed a new on-line POMDP solver called Multilevel POMDP Planner (MLPP). MLPP combines the commonly used Monte-Carlo-Tree-Search with a recent concept in Monte-Carlo, called Multilevel Monte-Carlo (MLMC). MLMC is a variance reduction technique that uses cheap and coarse approximations to the system to carry out the majority of the simulations, and combines them with a small number of accurate but expensive simulations to maintain correctness. By constructing a set of correlated samples from a sequence of approximations to the original system’s dynamics, in conjunction with applying Multilevel Monte-Carlo estimation to compute the expected value of sequences of actions, MLPP can compute near-optimal policies substantially faster (up to 10x) than some of today's fastest on-line solvers on four challenging robot motion planning under uncertainty problems. Details of the results are available <a target="_blank" href="/assets/pdf/papers/isrr19_mlmc.pdf" >here</a>.
<p>

<h2> References </h2>

<div class="publications">
   {% bibliography --file project_1.bib %}
</div>

<h2> People </h2>
<ul>
    <li>Marcus Hoerger</li>
    <li>Hanna Kurniawati</li>
    <li>Alberto Elfes</li>
</ul>
