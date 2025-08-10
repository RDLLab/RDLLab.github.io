---
layout: page
title: On-line POMDP Planning Toolkit (OPPT)
description:
img: assets/img/project_img/oppt/thumb-oppt.png
importance: 3
category: active
do_not_show_post_desc: true
---

<p class="text-justify">
Good approximate solutions for problems framed as Partially Observable Markov Decision Processes (POMDPs) can now be computed on-line, with a few classes of problems being solved in near real-time. However, an easy to use software tool for on-line POMDP-based motion planning is still lacking. Software tools for solving POMDPs do exist. However, most, if not all, requires the user to program the problem directly in the solver's software, which increases the difficulty because the user will likely need to know and adjust their problem encoding to specific implementation details, such as the data structures used.
</p>

<p class="text-justify">
In this project, we develop On-line POMDP Planning Toolkit (OPPT) that allows users to specify POMDP model of motion planing problems via a configuration file and 3D models of the robot and environment. It also provides an API for ROS and Gazebo. Further, for more complex problems, users can use plug-in architectures to implement the core components of the POMDP model.
</p>

<p class="text-justify">
OPPT also aims to ease sensitivity analysis by allowing users to separate the POMDP model (including the robot’s environment) for planning and for simulated execution. It is known that developing a faithful POMDP model is often the main bottleneck in applications of POMDP based planning, today. However, it is also known that strategies computed with imperfect POMDP models can still generate relatively good robot behaviours. The ability to separate planning and execution environments will better facilitate sensitivity analysis studies of on-line POMDP solvers and allow users to better predict the performance of on-line POMDP solvers in the physical world.
</p>

<p class="text-justify">
For testing new solvers, OPPT provides an abstract and general POMDP solver class that is not restricted to specific data structures. The user has access to a rich framework that provides functionalities common for many motion planning problems, such as kinematic computations, physical simulation of the robot and the environment it operates in and collision detection. This enables the user to focus on implementing new POMDP solvers.
</p>

<p class="text-justify">
An example of OPPT in action:
</p>

<div class="video-wrapper" style="max-width: 600px; margin: auto;">
  <div class="embed-responsive embed-responsive-16by9">
    <iframe class="embed-responsive-item" src="https://www.youtube.com/embed/kHgLxKiNYmE" allowfullscreen>
    </iframe>
  </div>
</div>

<p class="text-justify">
OPPT can be downloaded from our <a target="_blank" href="https://github.com/RDLLab/oppt" >github repo</a>.
</p>

<p class="text-justify">
A simple grasping demo of using OPPT with Kinova Movo can be downloaded from our <a target="_blank" href="https://github.com/RDLLab/oppt_movo_grasp" >github repo</a>.
</p>

<p class="text-justify">
Details of the design and architecture are in the references below.
</p>


<h2> References </h2>

<div class="publications">
   {% bibliography --file project_3.bib %}
</div>

<h2> People </h2>
<ul>
    <li>Marcus Hoerger</li>
    <li>Jimy Cai</li>
    <li>Jihirshu Narayan</li>
    <li>Dimitri Klimenko</li>
    <li>Joshua Song</li>
    <li>Hanna Kurniawati</li>
</ul>
