---
layout: page
title: POMDP Planning for Problems with Large Discrete Action Space
description:
img: assets/img/project_img/large_discrete_action_space/thumb-pomdp-large-discrete-action-spaces.png
importance: 2
category: active
do_not_show_post_desc: true
---

<p class="text-justify">
Many good approximate POMDP (Partially Observable Markov Decision Process) solvers have been proposed since mid 2000, allowing POMDP to generate good strategies for realistic problems. Despite these advances, solving problems with large discrete action spaces remains difficult. This project aims to reduce such difficulties. To this end, we develop an adaptive method for action selection based on quantile statistics, specifically Cross-Entropy methods for optimisation.
</p>

<p class="text-justify">
We first propose a method, called <a target="_blank" href="assets/pdf/papers/acalci17_cemab.pdf" >Crossed-Entropy-based Multi Armed Bandit (CEMAB)</a>, for Multi-arm bandit problem --a framework used for action selection in many good online POMDP solvers today. Numerical experiments with up to 10,000 arm indicates that CEMAB outperforms various existing MAB methods, including ε-greedy, softmax, EXP1, UCB1.
</p>

<p class="text-justify">
We then propose an expansion of CEMAB to approximately solve POMDP problems on-line, and call this solver <a target="_blank" href="assets/pdf/papers/icaps18_qbase.pdf" >Quantile-Based Action SElecter (QBASE)</a>. Experiments on multiple benchmark, multi-robot problems and <a target="_blank" href="assets/pdf/papers/modsim19_inventory.pdf" >partially observed inventory control problems</a> with up to 1M actions indicate that QBASE can generate good policies for problems with large discrete action space within reasonable time.
</p>

{% include figure.liquid loading="eager" max-width="80%" path="assets/img/project_img/large_discrete_action_space/hunting_perf_light.png" title="Result" class="img-fluid rounded z-depth-1" figure_class="text-center"%}

{% include figure.liquid max-width="30%" loading="eager" path="assets/img/project_img/large_discrete_action_space/HN_11_3_3_show.gif" title="Result" class="img-fluid rounded z-depth-1" figure_class="text-center" %}


<div class="caption">
    An example of QBASE applied to multi-agent multi-target finding with centralised control. Left: Performance on different numbers of agents and targets. The notation |S| is the size of the state space, while |A| is the size of the action space. Right: A visualisation of a policy for the case of 3 agents finding 3 targets. The agents are green squares marked with letters. The actual positions of the targets before being captured are marked with dark red. The pink color indicates the agents' belief that a target is at the particular cell, the darker the color the higher the probability.
</div>

<p class="text-justify"> Details of the methods and more results are in the following references </p>

<h2> References </h2>

<div class="publications">
   {% bibliography --file project_2.bib %}
</div>

<h2> People </h2>
<ul>
    <li>Erli Wang</li>
    <li>Hanna Kurniawati</li>
    <li>Dirk P. Kroese</li>
</ul>
