---
layout: page
title: "Meta-RL with Bayesian Linear Task Models (GLiBRL)"
center_title: true
description: "GLiBRL: a deep Bayesian RL framework with exact, closed-form task inference over generalised linear transition and reward models, achieving the highest aggregate zero-shot performance on MuJoCo and MetaWorld ML10/45."
img: assets/img/project_img/glibrl/glibrl_overview.png
importance: 12
category: active
do_not_show_post_desc: true
---

<div class="text-center mb-3">
  <p class="text-muted" style="font-size: 1.35rem; margin-bottom: 0;">
    Jingyang You, Hanna Kurniawati
  </p>
</div>

<div class="text-center mb-3">
  <span class="badge rounded-pill bg-primary" style="font-size: 1rem; padding: 0.6rem 1rem;">
    NeurIPS 2026
  </span>
</div>

<div class="text-center mt-3 mb-4">
  <a href="https://arxiv.org/pdf/2512.20974" target="_blank" class="btn btn-lg btn-outline-primary mx-2">
    <i class="fas fa-file-pdf"></i> Paper
  </a>

  <a href="https://arxiv.org/abs/2512.20974" target="_blank" class="btn btn-lg btn-outline-secondary mx-2">
    <i class="ai ai-arxiv"></i> arXiv
  </a>

  <a href="https://github.com/RDLLab/GLiBRL" target="_blank" class="btn btn-lg btn-outline-dark mx-2">
    <i class="fab fa-github"></i> Code
  </a>
</div>


<p class="text-justify">
<strong>GLiBRL</strong> (<strong>G</strong>eneralised <strong>Li</strong>near Models in Deep <strong>B</strong>ayesian <strong>RL</strong> with Learnable Basis Functions) combines learned neural features with Bayesian linear reward and transition models. Closed-form Bayesian updates infer the task from new interactions, and a belief-conditioned policy uses this inference to adapt its behaviour.
</p>

<p class="text-justify">
Standard reinforcement learning algorithms typically do not account for variations in transition and reward dynamics, so they generalise poorly to unseen tasks. Bayesian Reinforcement Learning (BRL), a special case of Meta-RL, addresses this by maintaining distributions over the transition and reward models and continually performing Bayesian inference on them. Recent deep BRL methods, such as PEARL and VariBAD, learn these models with neural networks. However, applying neural networks directly to the joint space of data and task parameters makes exact inference intractable, so these methods resort to variational inference and the Evidence Lower Bound (ELBO). This brings high-variance Monte Carlo estimates, amortisation gaps and posterior collapse, which often prevent the agent from extracting meaningful, distinctive task representations.
</p>

<p class="text-justify">
GLiBRL takes a different route. It enforces a <strong>generalised linear</strong> relation between task parameters and data features computed by <strong>learnable non-linear basis functions</strong>. This structure admits <strong>conjugate Bayesian inference</strong>: exact, sequential posterior updates over both the task parameters and the model noise, together with a <strong>closed-form marginal likelihood</strong> that removes variational inference entirely.
</p>

<div class="row justify-content-center mt-3 mb-4">
  <div class="col-sm-10">
    {% include figure.liquid
      loading="eager"
      path="assets/img/project_img/glibrl/glibrl_overview.png"
      title="Overview of GLiBRL"
      class="img-fluid rounded z-depth-1" %}
    <div class="caption">
      Overview of GLiBRL. At each timestep, the belief is updated to the exact posterior over the task parameters, and the belief-conditioned policy selects the next action.
    </div>
  </div>
</div>

<h2>Key Ideas</h2>

<ul>
  <li><strong>Exact, sequential posterior updates.</strong> Transition and reward models are linear in learned features, with a Normal-Wishart prior over task parameters and model noise. The posterior is updated in closed form after every interaction, with no sampling or amortised encoder.</li>
  <li><strong>ELBO-free model learning.</strong> The basis functions are trained by directly maximising the exact marginal log-likelihood of the observed data, avoiding the approximation error of variational objectives.</li>
  <li><strong>Inference over model noise.</strong> Unlike ALPaCA and CAMeLiD, which assume known noise, GLiBRL infers the noise exactly. This reduces prediction errors on unseen tasks for both the transition and reward models.</li>
  <li><strong>Permutation invariance.</strong> The exact Bayesian update does not depend on the order of the data, so GLiBRL plugs into both off-policy (SAC) and on-policy (PPO) algorithms.</li>
  <li><strong>Analytically traceable task representations.</strong> An exact kernel identity relates distances between learned task representations to signed-measure kernel discrepancies over the task contexts.</li>
</ul>

<h2>Training Loop</h2>

<p class="text-justify">
GLiBRL is an online algorithm that alternates between data collection and learning. During data collection, a batch of tasks is sampled from the task distribution. At each timestep, GLiBRL (1) updates the current belief to the exact posterior over the task's transition and reward parameters, (2) samples an action from the belief-conditioned policy, and (3) executes it to observe the next state and reward. Once enough data has been collected, GLiBRL updates the policy network with standard RL losses and the basis functions with the exact marginal log-likelihood.
</p>

<h2>Benchmark Problems</h2>

<p class="text-justify">
We evaluate GLiBRL on locomotion and manipulation benchmarks, focusing on <strong>zero-shot</strong> test-time performance: the agent receives no task-specific observations before evaluation and adapts only by updating its posterior online, with no gradient updates at test time.
</p>

<ul>
  <li><strong>MuJoCo locomotion</strong>: HalfCheetahDir, AntDir and HalfCheetahVel. The agent must infer from rewards whether to run forward or backward, or at which target velocity.</li>
  <li><strong>MetaWorld ML10 / ML45</strong>: 10 or 45 training manipulation tasks, with 5 held-out test tasks where both the transition and reward functions are unseen. This is one of the most challenging Meta-RL benchmarks.</li>
</ul>

<h2>Results</h2>

<p class="text-justify">
GLiBRL was compared against eight representative and recent Meta-RL methods across the three main families: gradient-based (MAML), black-box (RL<sup>2</sup>, AMAGO, TrMRL, ECET) and task-inference / deep BRL (PEARL, VariBAD, SDVT). GLiBRL achieves the <strong>highest aggregate zero-shot test performance</strong> on both the MuJoCo locomotion and the MetaWorld ML10/45 manipulation benchmarks.
</p>

<div class="row justify-content-center mt-3 mb-4">
  <div class="col-sm-12">
    {% include figure.liquid
      loading="eager"
      path="assets/img/project_img/glibrl/glibrl_results.png"
      title="Test-time return and success rate in all benchmarks"
      class="img-fluid" %}
    <div class="caption">
      Test-time return and success rate in all benchmarks. Curves report the interquartile mean (IQM), and shaded regions denote 95% confidence intervals. In GLiBRL, $D_T=16$ and $D_R=256$ for MetaWorld tasks, and $D_R=8$ for MuJoCo tasks, which do not require inference in transition. In MuJoCo tasks, PEARL and GLiBRL are run with SAC for $10^6$ steps and the other baselines with PPO for $10^7$ steps.
    </div>
  </div>
</div>

<h2>Code</h2>

<p class="text-justify">
<a href="https://github.com/RDLLab/GLiBRL" target="_blank">GLiBRL</a>
</p>

<h2>References</h2>

<div class="publications">
  {% bibliography --file project_12.bib %}
</div>
