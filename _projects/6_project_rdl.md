---
layout: page
title: Scaling Long-Horizon Online POMDP Planning with Rapid State Space Sampling 
description:
img: assets/img/project_img/ropras/ROPRaS_thumbnail.png
importance: 6
category: active
do_not_show_post_desc: true
---

<h4>Yuanchu Liang, Edward Kim, Wil Thomason, Zachary Kingston, Hanna Kurniawati and Lydia E. Kavraki</h4>
<hr>

<p class="text-justify">
Many realistic robotic problems contain high levels of uncertainties, long horizon planning and information gatherings are often required to find a robust action to execute. Despite tremendous improvement in the scalability of POMDP
solvers, long-horizon pomdps (e.g., $\geq$15 steps) remain difficult to solve. This paper proposes a new approximate online POMDP solver, called <strong>Reference-Based Online POMDP Planning via Rapid State Space Sampling (ROP-RaS3)</strong>. ROP-RaS3 uses novel extremely fast sampling-based motion planning techniques to sample the state space and generate a diverse set of macro actions online which are then used to bias belief-space sampling and infer high-quality policies <strong>without</strong> requiring exhaustive enumeration of the action space -- a fundamental constraint for modern online POMDP solvers. 
</p>

{% include figure.liquid loading="eager" path="assets/img/project_img/ropras/tree_compare.png" title="tree comparison" class="img-fluid rounded z-depth-1" %}
<div class="caption">
    A illustrated comparison of the belief tree constructed via exhuastive backup (left) and reference backup (right). The reference based belief tree is sparser and deeper thanks to the removal of exhuastive enumeration during construction.
</div>

<p class="text-justify">
The reference-based POMDP incorporates a reference policy $\bar{\pi}$ such that the objective of the POMDP is to find the best reward gathering action without deviating too far from the reference.
$$V(b) = \sup_{\pi\in\Pi}\left[R(b,\pi), \frac{1}{\eta}\text{KL}(\pi\| \bar{\pi}) + \gamma\sum_{a,o}P(o\mid a, b)\pi(a\mid b)V(\tau(b,a,o))\right]$$
Here $b$ is the current belief, $R$ is the stochastic reward, $\eta$ is a temperature term, $KL$ denotes the KL divergence, $P(o\mid a, b)$ denotes the conditional probability of receiving an observation based on the action and the belief, and $\tau$ denotes the belief update function. It turns out that such objective can be analytically solved to give,
$$
V(b) = \frac{1}{\eta}\log \left[\sum_a \bar{\pi}(a\mid b)\exp\left\{\eta\left[R(b,a)+\gamma\sum_o P(o\mid a, b)V(\tau(b,a,o))\right]\right\}\right]
$$
Notice that this form is an expectation of the future discounted reward under the reference policy and it can be estimated via monte-carlo sampling. To further boost computatiional efficiency in solving long horizon robotic problems, we sample deterministic motion plans from <a href="https://github.com/KavrakiLab/vamp">Vector Accelerated Motion Planning (VAMP)</a> as macro actions in our belief tree constructions.
</p>

{% include figure.liquid loading="eager" path="assets/img/project_img/ropras/experiment_pics.png" title="experiment pictures" class="img-fluid rounded z-depth-1" %}
<div class="caption">
Benchmark environments with obstacles (grey), landmarks (purple), danger zones (red), starting locations (orange), goals and targets (green). <br>
</div>

<div class="text-justify">
We benchmark our method against other state-of-the-art methods across four different domains and show that our methods performs exceeds other methods by a large extent. The environments are showing above and detailed below, <br>
<strong>Light Dark</strong>: The classical POMDP problem, the agent initiallised randomly is required to localised within the purple light and then navigate to the goal. <br>
<strong>Maze2D</strong>: A very long horizon problem. The agent that could start at two different entires is required utilise scattered lights to collect state information and navigate to the goal. <br>
<strong>Random3D</strong>: A environment with randomised obstacles to create arbitrary narrow passages. This environment is made to stress test the robustness of the proposed method if underlying reference policies fails probabilistically. <br>
<strong>Multi-Drone Tag</strong>: The agent needs to control four drones to capture the target. Target information is provided if any drone is within a certain detection radius. The drone can teleport to opposite sides if it reaches one end of the map, but drones cannot. <br>

The methods we compar against include, <br>
<strong> B-VAMP </strong>: The underlying reference policy with belief update during execution loops. No POMDP planning on top of the reference. <br>
<strong> Ref-Basic </strong>: The original reference-based solver proposed from our lab in the earlier work. <br>
<strong> POMCP </strong>: The classical state-of-the-art POMDP online planner. <br>
<strong> R-POMCP </strong>: POMCP but with actions sampled from our reference policy. <br>
<strong> MAGIC </strong>: An integrated learning and planning framework that generate learnt macro actions for planning. <br>
<strong> RMAG </strong>: The improved version of MAGIC that uses special modularised memory neural networks to generate better macro actions. <br>

Across all domains, ROPRaS3 has clear advantages in terms of successfully completing the problem while being efficient in terms of number of steps being executed in each task. In Maze2D, the agent equipped with ROPRaS3 figures out to use nearby lights to localise even though it might take an extra few steps. In Random3D, ROPRas3's performance do not drop much as the reference policy failure rates increases. And in Multi-Drone Tag, ROPRaS3 succesfully spread the drone out to capture the target from all directions, preventing it from escaping, demonstrating long horizon searching and planning capabilities. Details experimental results are given below.
</div>

{% include figure.liquid loading="eager" path="assets/img/project_img/ropras/ropras_tab1.png" title="experiment pictures" class="img-fluid rounded z-depth-1" %}
{% include figure.liquid loading="eager" path="assets/img/project_img/ropras/ropras_tab2.png" title="experiment pictures" class="img-fluid rounded z-depth-1" %}
{% include figure.liquid loading="eager" max-width="50%" path="assets/img/project_img/ropras/ropras_tab3.png" title="experiment pictures" class="img-fluid rounded z-depth-1" figure_class="text-center" %}

<h2> References </h2>

<div class="publications">
   {% bibliography --file project_6.bib %}
</div>




