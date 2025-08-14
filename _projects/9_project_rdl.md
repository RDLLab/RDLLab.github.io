---
layout: page
title: An Approximate Solver for Continuous POMDPs
description:
img: assets/img/project_img/continuous_pomdps/partition_tree.png
importance: 9
category: active
do_not_show_post_desc: true
---

<p class="text-justify">
Many decision making under uncertainty problems can naturally be specified as POMDP problems with continuous state, action and observation spaces. While many effective online POMDP solvers for discrete action and observation spaces exist, solving fully continuous POMDPs remains challenging, particularly for problems with high-dimensional action spaces. To alleviate this difficulty, we propose <b>A</b>daptive <b>D</b>iscretization using <b>V</b>oronoi Trees (ADVT), a new Monte Carlo Tree Search (MCTS) based online POMDP solver. ADVT adaptively partitions the action space for each sampled belief using a hierarchical data structure, called Voronoi Tree -- a Binary Space Partitioning that implicitly maintains the partition of a cell as as the Voronoi Diagram of two points sampled from the cell. This enables ADVT to exploit local structure of the action value function, thereby leading to a more effective exploration of the action space, particularly for higher-dimensional action spaces, compared to existing state-of-the-art methods. Additionally, ADVT handles continuous observation spaces by adopting a progressive widening strategy in combination with a weighted particle representation of beliefs. Empirical results indicate that ADVT scales substantially better to high-dimensional action spaces compare to existing methods. Our C++ implementation of ADVT is available at <a target="_blank" href="https://github.com/hoergems/ADVT" >https://github.com/hoergems/ADVT</a>.
</p>

<h2>
Voronoi Trees
</h2>

Key to ADVT is the combination of Monte Carlo Tree Search with a new hierarchical partitioning of the action space, called <b>Voronoi Tree</b>, illustrated in Figure 1. A Voronoi Tree is a Binary Space Partitioning (BSP) of the action space, where each partitioning hyperplane is implicitly maintained based on the Voronoi diagram of a pair of sampled representative actions. At each sampled belief, ADVT maintains a Voronoi Tree that is refined adaptively during planning: Cells are refined if they either represent large, unexplored regions of the action space, or contain actions with large estimated values. This enables ADVT to quickly focus its search on the most promising regions of the action space.

{% include figure.liquid loading="eager" path="assets/img/project_img/continuous_pomdps/partition_tree.png" title="" class="img-fluid rounded z-depth-1" %}
<div class="caption">
    Figure 1: Illustration of the relation between a belief tree (left), the Voronoi tree associated to belief (middle) and the partition of the action space induced by the Voronoi tree (right).
</div>

<h2>
Empirical Results
</h2>
We evaluated ADVT on a set of benchmark POMDP problems with continuous state, action and observation spaces, including a manipulation problem with a 12-dimensional continuous action space. Our results indicate that ADVT scales to higher-dimensional action spaces much more effectively compared to two state-of-the-art online POMDP solvers.

{% include figure.liquid loading="eager" path="assets/img/project_img/continuous_pomdps/results_1.png" title="" class="img-fluid rounded z-depth-1" %}
<div class="caption">
    Figure 2: Average total discounted reward achieved by <b>ADVT</b> (ours) and the state-of-the-art solvers VOMCPOW and POMCPOW on six benchmark POMDP problems. The average is taken over 1,000 simulation runs per problem and solver.
</div>

{% include figure.liquid loading="eager" path="assets/img/project_img/continuous_pomdps/results_2.png" title="" class="img-fluid rounded z-depth-1" %}
<div class="caption">
    Figure 2: Average total discounted reward achieved by <b>ADVT</b> (ours) and the state-of-the-art solvers VOMCPOW and POMCPOW on the scalable manipulation problem SensorPlacement. The x-axis indicates the number of dimensions of the action space, up to a 12 dimensional continuous action space.
</div>

Additional details and results are available in our <a target="_blank" href="/assets/pdf/papers/ijrr23-advt.pdf" >paper</a>.

<h2> References </h2>

<div class="publications">
   {% bibliography --file project_9.bib %}
</div>

<h2> People </h2>
<ul>
    <li>Marcus Hoerger</li>
    <li>Hanna Kurniawati</li>
    <li>Nan Ye</li>
    <li>Dirk Kroese</li>    
</ul>

