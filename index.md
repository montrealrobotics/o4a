---
layout: default
title: Project template
permalink: index.html
---
{% include_relative _relative_includes/mathjax.html %}
{% include_relative _relative_includes/latex_macros.html %}


<!--Actual Page-->
<h1 style="text-align: center; font-family: Helvetica, sans-serif">One-4-All: Neural Potential Fields for Embodied Navigation</h1>

{% include_relative _relative_includes/authors.html %}
<div style="text-align: center;"><em>*Authors contributed equally.</em></div>
<br>
{% include_relative _relative_includes/badges.html %}
<br>
<a id="main_video"></a>
{% include_relative _relative_includes/main_video.html %}
<br>
<div style="text-align: center; width: 85%; margin:auto">
<p style='text-align: justify; font-style: italic'> 
<a style="font-weight: bold"> Abstract. </a> A fundamental task in robotics is to navigate between two locations. In particular, real-world navigation can require long-horizon planning using high-dimensional RGB images, which poses a substantial challenge for end-to-end learning-based approaches. Current semi-parametric methods instead achieve long-horizon navigation by combining learned modules with a topological memory of the environment, often represented as a graph over previously collected images. However, using these graphs in practice typically involves tuning a number of pruning heuristics to avoid spurious edges, limit runtime memory usage and allow reasonably fast graph queries. In this work, we present One-4-All (O4A), a method leveraging self-supervised and manifold learning to obtain a graph-free, end-to-end navigation pipeline in which the goal is specified as an image. Navigation is achieved by greedily minimizing a potential function defined continuously over the O4A latent space. Our system is trained offline on non-expert exploration sequences of RGB data and controls, and does not require any depth or pose measurements. We show that O4A can reach long-range goals in 8 simulated Gibson indoor environments, and further demonstrate successful real-world navigation using a Jackal UGV platform. </p>
</div>


## About
This page aims to present an overview of our method, as well as additional videos, figures and experiment details. 
Have a look at <a href="https://arxiv.org/abs/2303.04011">our paper</a> for an in-depth presentation of the method!

## Task
<p style="text-align: justify;">We consider a robot with a discrete action space 
$\actions = \{\mathtt{STOP}, \mathtt{FORWARD}, \mathtt{ROTATE\_ RIGHT}, \mathtt{ROTATE\_ LEFT}\}$ for an image-goal 
navigation task. Using our knowledge of the robot's geometry and an appropriate exteroceptive onboard sensor 
(e.g., a front laser scanner), we assume that the set of collision-free actions $\freeactions$ can be estimated.  
When prompted with a goal image, the agent should navigate to the goal location in a partially observable 
setting using only RGB observations $\obs_t$ and the $\freeactions$ estimates.
The agent further needs to identify when the goal has been reached by autonomously calling the $\mathtt{STOP}$ action 
in the vicinity of the goal.</p>

## Method
{% include_relative _relative_includes/main_diagram.html %}
<br>
<div style="text-align: justify;">
<p>O4A consists of 4 learnable deep networks trained with previously collected RGB observation trajectories 
$\tau_{\obs} =\{\obs_t\}_{t=1}^T$ and corresponding 
actions $\actiontraj = \{a_t\}_{t=1}^T$, without pose: </p>

<ul>
<li>The <a style="font-weight: bold">local backbone</a> $\local$ (left) 
takes as input RGB images  to produce low-dimensional latent codes $\code \in \latentspace$ and is trained with a 
self-supervised time contrastive objective. Once trained, the local backbone can output a local metric $\norm{\code_t - \code_s}$
to measure similarity between observations. The extracted codes will also be used as inputs for other modules;</li>
<li>The <a style="font-weight: bold">locomotion head </a> $\conn$ (center) uses pairs of latent codes to predict the action required to 
traverse from one latent code to the other (order matters), or the inability to do so through the 
$\mathtt{NOT\_CONNECTED}$ output;</li>
</ul>

<p>$\local$ and $\conn$ can then perform loop closures over $\tau_{\obs}$ and construct a directed graph 
$\graph$, where nodes represent images. Edges represent one-action traversability and are weighted using the local metric. 
$\graph$ will not be required for navigation, and is only relied on to derive training objectives for the last two components:</p> 
<ul>
<li>The <a style="font-weight: bold">forward dynamics
head </a> (bottom right) $\fd$ is trained using edges from $\graph$ to predict the next code $\code_j$ 
given the current code $\code_i$ and an action $a_{ij} \in \actions$;</li>
<li>The <a style="font-weight: bold"> geodesic regressor </a> $\georeg$ (top right), which 
learns to predict the shortest path length between from one code to the other. $\georeg$ is the core planning module and can be 
interpreted as encoding the geometry of $\graph$.</li>
</ul>
<p>When multiple environments are considered, $\local$, $\conn$ and $\fd$ are shared across environments, and we 
train environment-specific regressors $\georeg_i$.  </p>
</div>

## Navigation
<p style="text-align: justify;">
The geodesic regressor $\georeg$ provides a powerful signal to navigate to a goal image. Indeed, it factors in the 
environment geometry and can, for example, drive an agent out of a dead end to reach a goal that is close in terms of 
Euclidean distance, but far geodesically. We use $\georeg$ as the attractor in a potential function $\mathcal{P}$ in 
tandem with repulsors $p^{-}$ around previously visited observations (B,C,D). At each step, the agent A picks the action in $\freeactions$ that leads to a predicted waypoint W
minimizing the total potential function. All computations occur in the latent space 
$\latentspace$.
</p>

{% include_relative _relative_includes/potentials.html %}
<p style="text-align: justify">
 Furthermore, we call $\mathtt{STOP}$ by thresholding the local metric between the current 
image and the goal image. We found this to be more reliable than relying on $\conn$.
</p>



## Simulation Experiments
<p style="text-align: justify">
We perform our simulation experiments using 8 scenes from the <a href="http://gibsonenv.stanford.edu/database/">Gibson dataset</a> rendered with the 
<a href="https://aihabitat.org/">Habitat simulator</a>. Trajectories are categorized into easy (1.5 $-$ 3m), medium (3 $-$ 5m), hard (5 $-$ 10m) and
very hard (> 10m) based on their geodesic distance to the goal. The agent is a differential drive robot with 
two RGB cameras, one facing forward and the other facing backward. We showcase O4A trajectories for all scenes (rows)
and difficulty levels (columns).
</p>

{% include_relative _relative_includes/row_videos.html title="Aloha" src_1="img/aloha/Aloha_easy.mp4" src_2="img/aloha/Aloha_medium.mp4" src_3="img/aloha/Aloha_hard.mp4" src_4="img/aloha/Aloha_very_hard.mp4" %}

{% include_relative _relative_includes/row_videos.html title="Annawan" src_1="img/annawan/Annawan_easy.mp4" src_2="img/annawan/Annawan_medium.mp4" src_3="img/annawan/Annawan_hard.mp4" src_4="img/annawan/Annawan_very_hard.mp4" %}

{% include_relative _relative_includes/row_videos.html title="Cantwell" src_1="img/cantwell/Cantwell_easy.mp4" src_2="img/cantwell/Cantwell_medium.mp4" src_3="img/cantwell/Cantwell_hard.mp4" src_4="img/cantwell/Cantwell_very_hard.mp4" %}

{% include_relative _relative_includes/row_videos.html title="Dunmor" src_1="img/dunmor/Dunmor_easy.mp4" src_2="img/dunmor/Dunmor_medium.mp4" src_3="img/dunmor/Dunmor_hard.mp4" src_4="img/dunmor/Dunmor_very_hard.mp4" %}

{% include_relative _relative_includes/row_videos.html title="Eastville" src_1="img/eastville/Eastville_easy.mp4" src_2="img/eastville/Eastville_medium.mp4" src_3="img/eastville/Eastville_hard.mp4" src_4="img/eastville/Eastville_very_hard.mp4" %}

{% include_relative _relative_includes/row_videos.html title="Hambleton" src_1="img/hambleton/Hambleton_easy.mp4" src_2="img/hambleton/Hambleton_medium.mp4" src_3="img/hambleton/Hambleton_hard.mp4" src_4="img/hambleton/Hambleton_very_hard.mp4" %}

{% include_relative _relative_includes/row_videos.html title="Nicut" src_1="img/nicut/Nicut_easy.mp4" src_2="img/nicut/Nicut_medium.mp4" src_3="img/nicut/Nicut_hard.mp4" src_4="img/nicut/Nicut_very_hard.mp4" %}

{% include_relative _relative_includes/row_videos.html title="Sodaville" src_1="img/sodaville/Sodaville_easy.mp4" src_2="img/sodaville/Sodaville_medium.mp4" src_3="img/sodaville/Sodaville_hard.mp4" src_4="img/sodaville/Sodaville_very_hard.mp4" %}
<br>
<div style="text-align: justify">
We compare O4A with relevant baselines and further study two additional O4A variants by ablating terms in the 
potential function $\mathcal{P}$. We also denote which methods rely on a graph ($\graph$) for navigation and 
oracle stopping (other methods need to call $\mathtt{STOP}$ autonomously). We find that 
O4A substantially outperforms baselines, achieving a higher Success Rate (SR), Soft Success Rate (SSR), 
Success Weighted by Path Length (SPL), and a competitive ratio of Collision-Free Trajectories (CFT).
The two O4A ablations confirm that all considered potentials in our potential function $\mathcal{P}$ are essential 
contributors to success. 
</div>
<br>
{% include_relative _relative_includes/main_table.html %}

## Robot Experiments
<div style="text-align: justify">
For the real-world experiments we run O4A on the Clearpath Jackal platform over 9 episodes in our lab, repeated
3 times each. A video with 2 robot episodes is shown at the <a href="#main_video">top of the page</a>. In addition to Success Rate (SR) and 
Soft Success Rate (SSR), we evaluate the final distance to goal (DTG), the number of $\mathtt{FORWARD}$ steps, and 
the number of $\mathtt{ROTATION}$ steps. For context, we also teleoperated the robot over the same episodes to 
provide an estimate of human performance. 
</div>
<br>

{% include_relative _relative_includes/jackal_table.html %}
<br>

<div style="text-align: justify">
O4A solves most episodes and achieves an average DTG of under 1m, even if most goals were not visible from the starting location and located up to 9 meters away.
</div>

<br>
{% include_relative _relative_includes/jackal_video.html %}
<br>

## Embeddings
<div style="text-align: justify">
The geodesic regressor's training objective has strong connections with the manifold learning literature, and we show
how the first 2 principal components of its last layer results in interpretable visualizations 
by comparing them to the ground truth maps. Points correspond to the location of RGB observations and are colored by 
the sum of their $x$ and $y$ coordinates. The unsupervised latent geometry is often consistent with the environment geometry, 
and some topological features (e.g., the obstacle "holes") are evident in the latent space, even if the training of 
O4A never used pose information.
</div>
<br>

{% include_relative _relative_includes/graph_embedding_images_1.html %}
{% include_relative _relative_includes/graph_embedding_images_2.html %}


## Detailed architectures and baselines
We finally show detailed architectures and hyperparameters for O4A and the baselines we used.

### O4A

{% include_relative _relative_includes/architecture_o4a.html %}

### SPTM

{% include_relative _relative_includes/architecture_sptm.html %}

### ViNG

{% include_relative _relative_includes/architecture_ving.html %}

### Hyperparameters

{% include_relative _relative_includes/hyperparam_table.html %}

## Extra plots for the Simulation Experiments

{% include_relative _relative_includes/extended_results.html %}

## Augmentations used during Taining

{% include_relative _relative_includes/augmentations_images.html src_1="img/augmentations/original.png" caption_1="Original" src_2="img/augmentations/brightness_contrast.png" caption_2="Brightness/contrast" src_3="img/augmentations/dropout.png" caption_3="Dropout" src_4="img/augmentations/gauss_noise.png" caption_4="Gaussian noise" src_5="img/augmentations/hue_saturation.png" caption_5="Hue saturation" %}
{% include_relative _relative_includes/augmentations_images.html src_1="img/augmentations/jitter.png" caption_1="Color jitter" src_2="img/augmentations/motion_blur.png" caption_2="Brightness/Motion blur" src_3="img/augmentations/perspective.png" caption_3="Perspective change" src_4="img/augmentations/sharpening.png" caption_4="Sharpening" src_5="img/augmentations/shift_scale_rotate.png" caption_5="Shift-Scale-Rotate" %}

## Citation

{% include_relative _relative_includes/citation.html %}
