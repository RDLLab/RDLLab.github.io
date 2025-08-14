---
layout: page
title: Approximate POMDP Solving without Numerical Optimisation 
description:
img: assets/img/project_img/analytical/deep.png
importance: 7
category: active
do_not_show_post_desc: true
---

<p class="text-justify">
Current state-of-the-art online POMDP planners rely on algorithms that compute estimates of the expected total 
reward of performing an exhaustive set of actions before optimising over these estimates. 
As such, these solvers *exhaustively enumerate* over the entire action space at each node in the belief tree, 
which massively hinders fast computation of a close-to-optimal solution for problems with large action spaces and long horizons. 
This problem is even worse when the environment is also dynamically changing at each execution step.
</p>

<p class="text-justify">
The core difficulty is the curse of history where the set of possible futures branches by the size of the action space and grows exponentially with respect to the horizon.
Most existing methods try to abstract the problem into a simpler one by either reducing the size of the action space or relying on *macro actions*---i.e. a set of open-loop action sequences---to reduce the planning horizon.
Still, the fundamental problem---i.e. exhaustive action enumeration---remains.
</p>

<p class="text-justify">
This project seeks to ameliorate this undesirable requirement by employing *reference-based* methods.
In short, this is a form of Kullback-Leibler (KL) regularisation on the objective relative to some *reference policy*
which encodes some (though not all) information about the solution.
We propose two main contributions to this end.
</p>

<ul>
    <li>Reference-Based POMDPs <a target="_blank" href="/assets/pdf/papers/neurips23.pdf" >here</a></li>
    <li>Partialy Observable Reference Policy Programming 
</ul>

{% include figure.liquid loading="eager" path="assets/img/project_img/analytical/deep.png" title="deep sampling" class="img-fluid rounded z-depth-1" %}
<div class="caption">
    Current state-of-the-art online planners are "exhaustive" in the sense that they branch over actions at each node before proceeding deeper down the tree, so meaningful long horizon futures are seldom explored.
    In contrast, reference-based approaches sample using information encoded by the reference policy and can sample deeper and sparser while gradually improving the policy.
</div>

<h2>
Reference-Based POMDPs
</h2> 

<p class="text-justify">
The notion of a Reference-Based POMDP (RBPOMDP) is a reformulation of a POMDP whose objective is penalised by the KL-divergence between a chosen and nominal reference policy.
The form of ojbective allows analytical action optimisation at each belief node, so that the value can be approximated by estimating expectations under the reference policy.
This property accommodates solvers that have been shown to perform effectively on certain long-horizon tasks and forms a first step in our analysis.
The full paper is available <a target="_blank" href="/assets/pdf/papers/neurips23.pdf" >here</a></li>.
</p>


{% include figure.liquid loading="eager" path="assets/img/project_img/analytical/nav3d.png" title="policy trace" class="img-fluid rounded z-depth-1" %}
<div class="caption">
    The executed policy trace of a reference-based approximate planner (in blue) whose reference policy naively takes actions which minimise the Euclidean distance. The deformation achieved by the new policy is clearly seen.
</div>

<h2>
Partially Observable Reference Policy Programming
</h2>


<p class="text-justify">
While approximate RBPOMDP solvers have been shown to perform well with well-chosen reference policies, the formulation comes at the cost that the solution has a baked-in commitment to the reference policy.
In general, it is unclear a priori which reference policies would yield near optimal policies for the original POMDP of interest, and the computed solution is vulnerable to reference policy mis-specification.
The aim of Partially Observable Reference Policy Programming (PORPP) is to exploit the information encoded the the reference policy to sample towards promising belief nodes, while simultaneous enforcing a gradual and systematic
update to the reference policy. In doing so, one can guarantee asymptotic convergence to the solution of the original POMDP and also adaptive policies to changing environments or models.
Furthermore, our theoretical analysis shows that the performance loss of the exact scheme is bounded by the *average* of the sampling errors,
meaning the algorithm is less sensitive to large approximation errors.
The full paper is available <a target="_blank" href="/assets/pdf/papers/ijcai25.pdf" >here</a></li>.
<p>

{% include figure.liquid loading="eager" path="assets/img/project_img/analytical/deform1.png" title="example scenario
for POMDP evaluation" class="img-fluid rounded z-depth-1" %}
{% include figure.liquid loading="eager" path="assets/img/project_img/analytical/deform2.png" title="example scenario
for POMDP evaluation" class="img-fluid rounded z-depth-1" %}
<div class="caption">
Executed trace of a helicopter during a search and rescue mission in the Corsica region. The pilot needs to avoid a danger zone which appears without warning during the middle of the run. PORPP can adapt quickly to the change, executing a well-crafted policy.
</div> 

<h2> References </h2>

<div class="publications">
   {% bibliography --file project_7.bib %}
</div>

<h2> People </h2>
<ul>
    <li>Edward Kim</li>
    <li>Hanna Kurniawati</li>
    <li>Yohan Karunanayake</li>
</ul>
