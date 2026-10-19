---
layout: page
title: "VOiLA: Vectorized Online Planning with Learned Diffusion Models for POMDP Agents"
center_title: true
description: "VOiLA combines task-agnostic learned POMDP world models with massively parallel online planning, generalizing to unseen environments and transferring from simulation to a real quadruped."
img: assets/img/project_img/voila/overview.png
importance: 12
category: active
do_not_show_post_desc: true
---

<div class="text-center mb-3">
  <p class="text-muted" style="font-size: 1.35rem; margin-bottom: 0;">
    Marcus Hoerger, Rishikesh Joshi, Rahul Shome, Ian Manchester, Hanna Kurniawati
  </p>
</div>

<div class="text-center mb-3">
  <span class="badge rounded-pill bg-primary" style="font-size: 1rem; padding: 0.6rem 1rem;">
    ISRR 2026
  </span>
</div>

<div class="text-center mt-3 mb-4">
  <a href="/assets/pdf/papers/isrr26_VOiLA.pdf" target="_blank" class="btn btn-lg btn-outline-primary mx-2">
    <i class="fas fa-file-pdf"></i> Paper
  </a>
  <a href="https://arxiv.org/abs/2606.19729" target="_blank" class="btn btn-lg btn-outline-secondary mx-2">
    <i class="ai ai-arxiv"></i> arXiv
  </a>
</div>

<p class="text-justify">
<strong>VOiLA (Vectorized Online planning wIth Learned diffusion model for POMDP Agents)</strong> is a framework for online planning under uncertainty that combines <strong>learned, task-agnostic POMDP world models</strong> with the massively parallel <a href="/projects/11_project_rdl">Vectorized Online POMDP Planner (VOPP)</a>. It enables robots to plan in belief space without requiring manually specified transition and observation models.
</p>

<p class="text-justify">
Partially Observable Markov Decision Processes (POMDPs) provide a principled framework for reasoning about uncertain action outcomes and noisy, incomplete observations. However, constructing faithful POMDP models for complex robots is difficult. Reinforcement learning (RL) avoids the need for explicit models, but often requires substantial task-specific training data and can struggle when the environment changes. VOiLA combines the strengths of both approaches: it learns reusable models from offline simulation data, then uses those models to compute actions <strong>online</strong>, adapting its decisions to the robot's current belief and task.
</p>

<div class="row justify-content-center mt-3 mb-4">
  <div class="col-12">
    {% include figure.liquid
      loading="eager"
      path="assets/img/project_img/voila/overview.png"
      title="Overview of VOiLA"
      class="img-fluid rounded z-depth-1" %}
    <div class="caption">
      Overview of VOiLA: task-independent data collection in simulation, learning of POMDP models, and online belief-space planning with VOPP (Figure 1 in the paper).
    </div>
  </div>
</div>

<p class="text-justify">
Across three evaluation problems, VOiLA achieves competitive or better results than leading RL-based POMDP methods while using substantially less training data. In a physical robot experiment, models trained <strong>entirely in simulation</strong> enabled a Unitree Go2 quadruped to complete its target-finding task in <strong>10 out of 10 trials</strong>, without additional real-world model training.
</p>

<h2>Key Idea: Learn the World Model Offline, Plan Online</h2>

<p class="text-justify">
Instead of learning a task-specific policy, VOiLA learns the components of a POMDP model from trajectories collected by a <strong>task-independent uniform policy</strong> in simulation. The learned models can subsequently be used for different objectives that share the same underlying transition and observation models. VOiLA learns three components that support both online planning and particle-based belief updates:
</p>

<ul>
  <li><strong>Transition sampler</strong> $f_T(s,a,\omega_T)$: samples possible next states given the current state and action, capturing uncertainty in robot dynamics.</li>
  <li><strong>Observation sampler</strong> $f_Z(s',a,\omega_Z)$: generates possible observations conditioned on the next state and action. For high-dimensional observations, such as RGB images, sampling takes place in a compact learned latent space.</li>
  <li><strong>Observation-likelihood model</strong> $\ell(s',a,o)$: provides relative observation likelihoods used to weight particles during belief updates. It is trained with a contrastive, InfoNCE-style objective.</li>
</ul>

<h2>Efficient Generative Models through Diffusion Distillation</h2>

<p class="text-justify">
Complex robot dynamics and observation distributions can be high-dimensional and multimodal, making them difficult to represent with simple parametric models. VOiLA first trains <strong>conditional diffusion models</strong> to capture these distributions. However, generating samples through iterative diffusion inference is too expensive for online POMDP planning, which may require thousands of forward simulations per planning step.
</p>

<p class="text-justify">
To overcome this bottleneck, VOiLA <strong>distills diffusion samplers into compact feedforward neural networks</strong>. These generators produce stochastic next-state and observation samples in a single forward pass, making them suitable for massively batched inference. In our benchmarks, the distilled transition and observation samplers achieved approximately <strong>105&times;</strong> and <strong>67&times;</strong> the sampling throughput of their diffusion counterparts, respectively (batch size 1,024), while closely approximating their outputs.
</p>

<h2>Online Belief-Space Planning with VOPP</h2>

<p class="text-justify">
The learned models are integrated into <a href="/projects/11_project_rdl">VOPP</a>, a fully vectorized online POMDP planner. During planning, VOPP uses the learned transition and observation samplers to simulate candidate action sequences in parallel, expanding a belief tree and updating action preferences using batched tensor operations. VOiLA also extends VOPP with <strong>vectorized progressive widening</strong> to support continuous observation spaces.
</p>

<p class="text-justify">
After executing the selected action, VOiLA incorporates the new observation through a <strong>Sequential Importance Resampling (SIR) particle filter</strong>. The learned transition sampler generates proposal particles, and the learned likelihood model determines their relative weights. Planning then resumes from the updated belief.
</p>

<h2>Benchmark Problems</h2>

<p class="text-justify">
We evaluated VOiLA on three planning-under-uncertainty problems:
</p>

<ul>
  <li><strong>FloorPositioning</strong> &mdash; a robot must reach the correct goal in an environment with two possible floors, using noisy distance-to-wall measurements to infer its location.</li>
  <li><strong>LeggedNavigation</strong> &mdash; an ANYmal-C quadruped navigates a cluttered 20&times;20m environment using onboard 32&times;32 RGB observations. Two internal walls have <a href="#legged-navigation-environments">five possible configurations</a>; models are trained only in configurations 1 and 2 and tested on all five.</li>
  <li><strong>TargetFinding</strong> &mdash; a physical Unitree Go2 quadruped searches for a target asset hidden behind one of two movable boxes, using lidar observations while reasoning about uncertainty in its pose and obstacle and target locations.</li>
</ul>

<div class="row justify-content-center mt-3 mb-4">
  <div class="col-12">
    <div class="row g-3 justify-content-center">

      <div class="col-4 text-center">
        <div class="benchmark-image">
          {% include figure.liquid
            loading="eager"
            path="assets/img/project_img/voila/floor_positioning.png"
            title="FloorPositioning"
            class="img-fluid rounded z-depth-1" %}
        </div>
        <div class="caption text-center">FloorPositioning</div>
      </div>

      <div class="col-4 text-center">
        <div class="benchmark-image">
          {% include figure.liquid
            loading="eager"
            path="assets/img/project_img/voila/env_2.png"
            title="LeggedNavigation"
            class="img-fluid rounded z-depth-1" %}
        </div>
        <div class="caption text-center">LeggedNavigation</div>
      </div>

      <div class="col-4 text-center">
        <div class="benchmark-image">
          {% include figure.liquid
            loading="eager"
            path="assets/img/project_img/voila/unitree_step_0.png"
            title="TargetFinding"
            class="img-fluid rounded z-depth-1" %}
        </div>
        <div class="caption text-center">TargetFinding</div>
      </div>

    </div>

    <div class="caption text-center mt-3">
      The three VOiLA evaluation scenarios:
      FloorPositioning, LeggedNavigation, and TargetFinding
      (Figure 2 in the paper).
    </div>
  </div>
</div>

<style>
html {
  scroll-behavior: smooth;
}

#legged-navigation-environments {
  scroll-margin-top: 80px;
}

.benchmark-image {
  height: 220px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.benchmark-image img {
  width: 100% !important;
  height: 220px !important;
  object-fit: contain;
}
</style>

<h2>Results</h2>

<p class="text-justify">
We compared VOiLA against <strong>DreamerV3</strong>, <strong>Recurrent Soft Actor Critic for Discrete Actions (RSAC-D)</strong>, and <strong>Visual Tree Search (VTS)</strong>. The experiments used comparable total offline training-time budgets for the principal comparisons, while also evaluating RL baselines trained with substantially more data. VOiLA's learned models were trained on <strong>50,000 simulation transitions</strong> per problem.
</p>

<ul>
  <li><strong>FloorPositioning:</strong> VOiLA achieved a <strong>100% success rate</strong> across 100 trials, using 50K training transitions compared with 900K for DreamerV3, 1.8M for RSAC-D, and 32M for VTS.</li>
  <li><strong>LeggedNavigation (trained configurations):</strong> VOiLA achieved <strong>94%</strong> and <strong>100%</strong> success in environments 1 and 2, respectively, using 50K transitions. DreamerV3 and RSAC-D used 600K transitions in the comparable-training-time experiments.</li>
  <li><strong>Generalization to unseen environments:</strong> VOiLA achieved success rates of <strong>98%, 84%, and 88%</strong> in environments 3, 4, and 5, respectively. Both DreamerV3 and RSAC-D achieved <strong>0%</strong> in environment 3, even when trained with 2.6M transitions.</li>
</ul>

<div id="legged-navigation-environments" class="row justify-content-center mt-3 mb-4">
  <div class="col-12">
    <div class="row g-2 justify-content-center">

      <div class="col">
        {% include figure.liquid
          loading="eager"
          path="assets/img/project_img/voila/env_1.png"
          title="Environment 1"
          class="img-fluid rounded z-depth-1" %}        
      </div>

      <div class="col">
        {% include figure.liquid
          loading="eager"
          path="assets/img/project_img/voila/env_2.png"
          title="Environment 2"
          class="img-fluid rounded z-depth-1" %}        
      </div>

      <div class="col">
        {% include figure.liquid
          loading="eager"
          path="assets/img/project_img/voila/env_3.png"
          title="Environment 3"
          class="img-fluid rounded z-depth-1" %}        
      </div>

      <div class="col">
        {% include figure.liquid
          loading="eager"
          path="assets/img/project_img/voila/env_4.png"
          title="Environment 4"
          class="img-fluid rounded z-depth-1" %}        
      </div>

      <div class="col">
        {% include figure.liquid
          loading="eager"
          path="assets/img/project_img/voila/env_5.png"
          title="Environment 5"
          class="img-fluid rounded z-depth-1" %}        
      </div>

    </div>

    <div class="caption text-center mt-3">
      Five wall configurations for LeggedNavigation.
      Training uses only environments 1 and 2;
      environments 3&ndash;5 test generalization to unseen layouts
      (Figure 3 in the paper).
    </div>
  </div>
</div>

<h2>Real-World Deployment</h2>

<p class="text-justify">
For TargetFinding, VOiLA was deployed on a <strong>Unitree Go2</strong> in a 4.9&times;3.6m room. The robot had to identify which of two locations contained the target asset while navigating around obstacles whose positions were uncertain. Its lidar measurements were processed into 36-dimensional range observations, using the same preprocessing in simulation and on the robot. The target and movable obstacles were varied across test runs.
</p>

<p class="text-justify">
The learned models were trained entirely in simulation using just
<strong>50,000 transitions</strong> and transferred directly to the physical robot,
<strong>without any additional real-world training</strong>.
As the robot moved and gathered observations, its particle belief over
robot pose, target position, and obstacle locations evolved, supporting
information-gathering actions and collision-aware navigation.
VOiLA successfully located and reached the target in
<strong>10 out of 10 physical robot trials</strong>.
</p>

<div class="row justify-content-center mt-3 mb-4">
  <div class="col-12">
    <video
      class="w-100 rounded z-depth-1"
      controls
      playsinline
      preload="auto"
      width="1920"
      height="1080"
      style="max-width: 100%; height: auto;">
      <source src="{{ '/assets/video/record_cut_small_2.mp4' | relative_url }}" type="video/mp4">
      Your browser does not support HTML5 video.
    </video>

    <div class="caption text-center mt-2">
      Real-world deployment of VOiLA on the Unitree Go2 quadruped.
      Using POMDP models learned entirely in simulation, the robot
      performs online belief-space planning to locate and reach the target.
    </div>
  </div>
</div>

<h2>References</h2>

<div class="publications">
  {% bibliography --file project_12.bib %}
</div>
