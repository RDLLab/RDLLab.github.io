---
layout: page
title: A POMDP Approach for Safety Assessment of Autonomous Cars
# description:
img: assets/img/project_img/safety/preview.png
importance: 10
category: active
do_not_show_post_desc: true
---

<h2>Abstract</h2>

<p class="text-justify">
This paper proposes a mechanism to automatically assess the safety of autonomous robots, and in particular autonomous cars. Most methods to assess the safety of autonomous cars generate adver-sary strategies, which will then be used to test whether the car beingassessed can avoid accidents with the adversary. To generate such adversarial strategies, many have proposed learning techniques that requirea large amount of accident data. But, such data are difficult to obtain because accidents are rare. To alleviate this issue, we leverage the observation that when safe and colliding adversary trajectories are closer together, the vehicle is less safe because there is generally less buffer to avoid accidents. Specifically, we generate/utilise data on adversaries’ safe trajectories, which is more abundant than accidents data, and compute colliding adversarial trajectories that are as close as possible to the safe trajectories. The average distance between safe and colliding adversarial trajectories provides an indicator of the vehicles’ safety. To compute colliding adversarial trajectories, we take into account that the driving strategy of the vehicle being assessed is not fully known, and therefore propose a multi-objecive POMDP framing of the problem and an on-line planning method, called Constraint-Aware Tree (CAT), to compute approximate solution to the multi-objective POMDP. Evaluations of four learning-based autonomous driving software on pedestrian crossing and lane merging scenarios, derived from the National Highway Traffic Safety Administration (NHTSA), indicate the viability of the proposed testing mechanism in assessing a variety of autonomy software. Moreover, evaluations of CAT on the nuScenes dataset indicate that CAT generates more colliding adversarial trajectories in less time compared to state-of-the-artlearning-based method, STRIVE.
</p>



<h2>Proposed Safety Assessment Mechanism</h2>


    {% include figure.liquid loading="eager" path="assets/img/project_img/safety/framework.png" title="example image" class="img-fluid rounded z-depth-0" %}


The framework relies on the observation that in autonomous vehicles, the smaller the distance between adversaries’ safe trajectories and their closest colliding trajectories, the less safe the autonomous car being assessed is, because there is less margin for errors. An overview of the framework are as follows:


1. It takes the vehicles kinematics and dynamic constraints as inputs, together with the adversary safe trajectories.
2. It then uses our proposed online POMDP solver, CAT to compute a set of corresponding adversary collision trajectories.
3. The safety rating is then computed as the average Fréchet distance between the adversary safe trajectories and the generated collision trajectories.


<h2>Constraint-Aware Tree (CAT)</h2>


    {% include figure.liquid loading="eager" path="assets/img/project_img/safety/cat-algo.png" title="example image" class="img-fluid rounded z-depth-0" %}

Our proposed framework requires computation of collision trajectories with conflicting nature of objectives. In our mechanism, we want the adversaries to collide with the vehicle being assessed as fast as possible, while being as close as possible to a given safe trajectory. Hence, we propose CAT, A novel online POMDP solver that computes a close to optimal policy to solve this multi-objective problem. CAT adopts the epsilon-constraint approach to transform this multi-objective POMDP into multiple constrained POMDPs, where the difference between them are only in the values of the constraints. Here, the distance to the safe trajectory is set as a constraint.



Given CPOMDP problems $$P_{\phi,\epsilon_{i}}=\left\langle S,A,T,O,Z,R,\gamma,C,\phi,\epsilon_{i}  \right\rangle$$.
To solve these multiple constraint POMDP problems, CAT first selects a set of upper bound values $$\left\{ \epsilon_{1},\epsilon_{2} \cdots,\epsilon_{n} \right\}$$ for the distance constraint, and constructs CPOMDP problems for each $$\epsilon_{i}$$ for $$i \in \left[ 1,n \right]$$. We frame each constraint POMDP in such a way that we can solve them as POMDPs, where states that does not satisfy the constraints ($$d\left( \phi,s \right)\gt \epsilon_{i}$$) becomes terminating states, and entering them incurs a penalty.




    {% include video.liquid path="assets/img/project_img/safety/merge1.mp4" class="img-fluid rounded z-depth-0" autoplay=true controls=false  loop=true muted=true  %}



Although these simplified POMDP problems can be solved using existing solvers, solving n POMDP problems separately for each safe trajectory can be very expensive. Since each problem $$\epsilon_{i}$$ for $$i \in \left[ 1,n \right]$$ only differs only in their reward function, and having a substructure property where the set of reachable beliefs of POMDPs associated with smaller distance constraints is a subset of POMDPs associated with larger distance constraints, CAT exploits these 2 features to reuse prior computations and compute solutions to all $n$ POMDP problems at once. To achieve this, CAT maintains value functions $$\left[ V_{1}(b),V_{2}(b) \cdots,V_{n}(b) \right]$$ for each $$P_{\phi,\epsilon_{i}}$$, and during online planning, decide which POMDP problem to evaluate using PUCT strategy. Due to the aforementioned features, the outcome of sampling for one POMDP can be propagated to other POMDP problems, thus increasing sampling efficiency and accuracy.


<h2>Experiment: Evaluation of CAT on nuScenes Dataset</h2>

The first experiment here is to verify CAT as a suitable method for adversarial trajectory generation. We conduct this set of experiments on the nuScenes dataset, which consists of complex vehicle-vehicle traffic scenarios. In each of these scenarios, we control an adversarial vehicle to generate a trajectory to collide with the identified target vehicle, while avoiding other traffic participants.



<div class="row">
    <div class="col-sm-9 mt-3 mt-md-0 mx-auto">
        {% include video.liquid path="assets/img/project_img/safety/nuscne.mp4" class="img-fluid rounded z-depth-0" controls=true   muted=true width="800px" %}
    </div>
</div>


<h3>Quantitative Results</h3>

{% include figure.liquid loading="eager" path="assets/project_img/safety/table1.jpg" title="example image" class="img-fluid rounded z-depth-0" %}


<h3>Qualitative Results</h3>

{% include figure.liquid loading="eager" path="assets/project_img/safety/comp.jpg" title="example image" class="img-fluid rounded z-depth-0" %}

We observe that in some scenarios, STRIVE generates invalid collision trajectories by moving out of road boundaries, likely due to inaccuracies in its trained traffic model. The POMDP methods, CAT and ABT, are able to avoid this by encoding environmental collisions within the POMDP formulation, and can generate valid collision trajectories in these scenes.



<h2>Experiment: Evaluation of Assessment Mechanism</h2>

<h3>Experimental Setup</h3>

This second set of experiments is to evaluate if our proposed safety assessment mechanism, using Fréchet distance as a metric, can be used to accurately assign vehicle safety ranking to different car controllers. We implement 2 different experiment scenarios based on the NHTSA (National Highway Traffic Safety Administration) pre-crash scenario typology, namely pedestrian crossing and vehicle lane merging, to test our proposed safety assessment mechanism. We evaluate 4 of the top ranked vehicle controllers from the CARLA Leaderboard, each under 2 weather conditions, dry and wet.

{% include figure.liquid loading="eager" path="assets/img/project_img/safety/ranking.jpg" title="example image" class="img-fluid rounded z-depth-0" %}


<h3>Scenario 1: Pedestrian Crossing (Collision Trajectory Generation)</h3>


<div class="row">
    <div class="col-sm-9 mt-3 mt-md-0 mx-auto">
        {% include video.liquid path="assets/img/project_img/safety/cw.mp4" class="img-fluid rounded z-depth-0" controls=true   muted=true width="800px" %}
    </div>
</div>

<h3>Scenario 1: Experimental Results</h3>

{% include figure.liquid loading="eager" path="assets/img/project_img/safety/carla1.jpg" title="example image" class="img-fluid rounded z-depth-0" %}

<h3>Scenario 2: Vehicle Lane Merging (Collision Trajectory Generation)</h3>

<div class="row">
    <div class="col-sm-9 mt-3 mt-md-0 mx-auto">
        {% include video.liquid path="assets/img/project_img/safety/ml.mp4" class="img-fluid rounded z-depth-0" controls=true   muted=true width="800px" %}
    </div>
</div>

<h3>Scenario 2: Experimental Results</h3>

{% include figure.liquid loading="eager" path="assets/img/project_img/safety/carla2.jpg" title="example image" class="img-fluid rounded z-depth-0" %}



We observe that the safer the controller is (given by the lower collision rate from validating random trajectories), the higher the Fréchet Distance obtained from generating the collision trajectories using CAT, indicating the vehicle has larger room to account for errors and uncertainty. In this scenario, compared to Carla Leaderboard, the vehicle rankings are the same for collision involving other vehicles. The results indicate that it is important to account for the behavior and inner workings of the assessed vehicle as partially observed variables.

<h2> References </h2>

<div class="publications">
   {% bibliography --file project_10.bib %}
</div>
