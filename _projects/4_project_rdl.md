---
layout: page
title: Partially Observed Inventory Control
description:
img: assets/img/project_img/inventory_control/thumb-inventory-control.png
importance: 4
category: active
do_not_show_post_desc: true
---

<p class="text-justify">
Industrial investigations indicate that errors in inventory recordings are common and often unavoidable. Such errors result in dramatic wastes and cost to the industry. Inventory control in the presence of such errors is essentially a partially-observed decision-making problems. Although robust framework, such as the Partially Observable Markov Decision Processes (POMDPs) have been applied to inventory control, most work apply POMDPs to single commodity problems or assume independence between commodities, due to difficulties in solving problems with large discrete action space. This work applies our method, <a target="_blank" href="/papers/icaps18_qbase.pdf" >QBASE</a>, to problems with multiple commodities whose demand levels may be correlated. Numerical experiments on partially observed multi-commodity inventory control problems indicate that our proposed solution can find less conservative inventory control strategies that yield higher profits, compared to existing solutions.
</p>


{% include figure.liquid loading="eager" max-width="60%" path="assets/img/project_img/inventory_control/perfInventoryControl6.png" title="Result" class="img-fluid rounded z-depth-1" figure_class="text-center"%}

<div class="caption">
    A result of numerical experiments on a partially-observed inventory control problem with 6 different types of commodities. Left: Average total discounted reward, which reflects the average total profit, as time increases. Right: The inventory levels maintained for each type of commodity. Higher inventory level indicates a more conservative strategy.
</div>

<p class="text-justify">
Details of the POMDP model and experiments are available in the reference below:
</p>

<h2> References </h2>

<div class="publications">
   {% bibliography --file project_4.bib %}
</div>

<h2> People </h2>
<ul>
    <li>Erli Wang</li>
    <li>Hanna Kurniawati</li>
    <li>Dirk P. Kroese</li>
</ul>
