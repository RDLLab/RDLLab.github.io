---
layout: page
title: "Efficient Diffusion Policy Fine Tuning with Latent Noise Representation Bridging (LARBRID)"
center_title: true
description: "LARBRID: fine-tunes the initial noise of frozen diffusion and flow policies through a shared linear-MDP representation that removes noise aliasing, with near-optimal fine-tuning guarantees and up to 2-3× performance gains on robotic control tasks."
img: assets/img/project_img/larbrid/larbrid_overview.png
importance: 13
category: active
do_not_show_post_desc: true
---

<div class="text-center mb-3">
  <p class="text-muted" style="font-size: 1.35rem; margin-bottom: 0;">
    Yuanchu Liang, Yue Yang, An-Chi He, Hanna Kurniawati
  </p>
</div>

<div class="text-center mb-3">
  <span class="badge rounded-pill bg-primary" style="font-size: 1rem; padding: 0.6rem 1rem;">
    NeurIPS 2026
  </span>
</div>

<div class="text-center mt-3 mb-4">
  <a href="https://arxiv.org/pdf/2611.07275" target="_blank" class="btn btn-lg btn-outline-primary mx-2">
    <i class="fas fa-file-pdf"></i> Paper
  </a>

  <a href="https://arxiv.org/abs/2611.07275" target="_blank" class="btn btn-lg btn-outline-secondary mx-2">
    <i class="ai ai-arxiv"></i> arXiv
  </a>

  <a href="https://github.com/Maincode-Research/LARBRID" target="_blank" class="btn btn-lg btn-outline-dark mx-2">
    <i class="fab fa-github"></i> Code
  </a>
</div>

<p class="text-justify">
<strong>LARBRID</strong> (<strong>La</strong>tent <strong>R</strong>epresentation <strong>Brid</strong>ging for Diffusion) fine-tunes pre-trained diffusion and flow policies with reinforcement learning by learning only the distribution of their <strong>initial noise</strong>. The pre-trained model stays frozen, so it can be treated as a black box, and no gradients need to be backpropagated through the denoising chain.
</p>

<p class="text-justify">
Pre-trained diffusion and flow policies have become the backbone of many robotic foundation models, but they often fail on tasks that are not well covered by their training data. Fine-tuning them with RL is a natural fix, yet existing approaches are computationally expensive, sample inefficient and lack theoretical guarantees. Steering the initial noise of a deterministic sampler (e.g. DDIM or flow) is a cheap alternative, but it suffers from the <strong>noise aliasing</strong> problem: many noises can produce the same action and similar noises can produce different actions, so there is no natural link between the value of a noise and the value of the action it generates.
</p>

<p class="text-justify">
LARBRID resolves this by endowing the environment with a <strong>linear MDP</strong> structure. A frozen generative policy acting in a linear MDP induces a <strong>Latent Noise Linear MDP (LNL-MDP)</strong> over the noise space, whose representation is simply the environment representation pulled back through the pre-trained policy. State-action and state-noise pairs therefore share a single representation space, and the resulting <strong>Bridge Equation</strong> aligns action-space and noise-space Q-values exactly, removing noise aliasing without separate critic networks.
</p>

<div class="row justify-content-center mt-3 mb-4">
  <div class="col-sm-12">
    {% include figure.liquid
      loading="eager"
      path="assets/img/project_img/larbrid/larbrid_overview.png"
      title="Noise aliasing and the LNL-MDP"
      class="img-fluid rounded z-depth-1" %}
    <div class="caption">
      (a) Noise aliasing causes ambiguous associations between action values and noise values. (b) The LNL-MDP maps the state-action and state-noise spaces into a shared representation space, where the Q-values are automatically aligned by the Bridge Equation.
    </div>
  </div>
</div>

<h2>Key Ideas</h2>

<ul>
  <li><strong>Latent Noise Linear MDP.</strong> When the environment MDP is linear, the latent noise MDP induced by a frozen diffusion/flow policy is also linear, with features obtained by pulling the environment features back through the pre-trained policy.</li>
  <li><strong>The Bridge Equation.</strong> One shared representation gives the same Q-value for a noise and the action it generates, so a single critic serves both the action and noise spaces and noise aliasing disappears.</li>
  <li><strong>Near-optimal fine-tuning guarantees.</strong> Treating rollouts of the pre-trained model as a virtual offline dataset places LNL-MDP fine-tuning within the FineTuneRL framework, which gives near-optimal (minimax up to horizon factors) bounds on the number of online episodes needed once the representation is known.</li>
  <li><strong>Practical representation learning.</strong> LARBRID learns the shared representation from scratch with diffusion spectral representation, and then optimises a Gaussian noise policy with Soft Actor-Critic on top of it.</li>
  <li><strong>Flexible across settings.</strong> Since fine-tuning in the noise space and in the action space are two sides of the same coin, LARBRID has simplified variants, including <strong>LARBRID-I</strong>, which works directly in the action space for offline RL and avoids large critic ensembles.</li>
</ul>

<h2>Results</h2>

<p class="text-justify">
We evaluate LARBRID in online, offline and offline-to-online fine-tuning settings across Gymnasium, Robomimic and OGBench locomotion and manipulation tasks (including visual-input tasks), and when fine-tuning the 3.3B-parameter π<sub>0</sub> vision-language-action model on Aloha and Libero tasks. LARBRID gives <strong>up to 2-3× performance increases</strong> over state-of-the-art baselines. For example, it improves on DSRL by roughly 70% in the Robomimic Transport task, and LARBRID-I yields a 3× improvement on the OGBench large humanoid maze in the offline setting.
</p>

<div class="row justify-content-center mt-3 mb-2">
  <div class="col-sm-4">
    {% include figure.liquid loading="eager" path="assets/img/project_img/larbrid/halfcheetah_reward.png" title="Halfcheetah" class="img-fluid" %}
  </div>
  <div class="col-sm-4">
    {% include figure.liquid loading="eager" path="assets/img/project_img/larbrid/walker_reward.png" title="Walker" class="img-fluid" %}
  </div>
  <div class="col-sm-4">
    {% include figure.liquid loading="eager" path="assets/img/project_img/larbrid/hopper_reward.png" title="Hopper" class="img-fluid" %}
  </div>
</div>
<div class="caption mb-4">
  Online fine-tuning of pre-trained DDIM policies on Gymnasium control tasks, averaged over 4 random seeds.
</div>

<h2>Videos</h2>

<p class="text-justify">
Rollouts on OGBench tasks before and after fine-tuning. Each row is one task: the pre-trained policy is shown on the left and the fine-tuned policy on the right.
</p>

<div class="row text-center mt-3">
  <div class="col-6"><h5>Pre-trained</h5></div>
  <div class="col-6"><h5>Fine-tuned</h5></div>
</div>

{% assign larbrid_tasks = "antsoccer:Ant Soccer,cube:Cube,scene:Scene,puzzle:Puzzle" | split: "," %}
{% for task in larbrid_tasks %}
{% assign parts = task | split: ":" %}
<div class="row justify-content-center mb-3">
  <div class="col-12 text-center"><strong>{{ parts[1] }}</strong></div>
  <div class="col-6">
    <video src="{{ 'assets/img/project_img/larbrid/' | append: parts[0] | append: '-pretrain.mp4' | relative_url }}" class="img-fluid rounded z-depth-1" autoplay loop muted playsinline></video>
  </div>
  <div class="col-6">
    <video src="{{ 'assets/img/project_img/larbrid/' | append: parts[0] | append: '-fine-tune.mp4' | relative_url }}" class="img-fluid rounded z-depth-1" autoplay loop muted playsinline></video>
  </div>
</div>
{% endfor %}

<h2>Code</h2>

<p class="text-justify">
<a href="https://github.com/Maincode-Research/LARBRID" target="_blank">LARBRID</a>
</p>

<h2>References</h2>

<div class="publications">
  {% bibliography --file project_13.bib %}
</div>
