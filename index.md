---
layout: default
title: Project template
permalink: index.html
---

<h1 style="text-align: center; font-family: Helvetica, sans-serif">One-4-All: Neural Potential Fields for Embodied Navigation</h1>

{% include_relative _relative_includes/authors.html %}
<div style="text-align: center;"><em>*Authors contributed equally.</em></div>

<br>
<br>
{% include_relative _relative_includes/badges.html %}
<br>
<br>

{% include_relative _relative_includes/main_video.html %}

## Abstract

<p style='text-align: justify;'> A fundamental task in robotics is to navigate between two locations. In particular, real-world navigation can require long-horizon planning using high-dimensional RGB images, which poses a substantial challenge for end-to-end learning-based approaches. Current semi-parametric methods instead achieve long-horizon navigation by combining learned modules with a topological memory of the environment, often represented as a graph over previously collected images. However, using these graphs in practice typically involves tuning a number of pruning heuristics to avoid spurious edges, limit runtime memory usage and allow reasonably fast graph queries. In this work, we present One-4-All (O4A), a method leveraging self-supervised and manifold learning to obtain a graph-free, end-to-end navigation pipeline in which the goal is specified as an image. Navigation is achieved by greedily minimizing a potential function defined continuously over the O4A latent space. Our system is trained offline on non-expert exploration sequences of RGB data and controls, and does not require any depth or pose measurements. We show that O4A can reach long-range goals in 8 simulated Gibson indoor environments, and further demonstrate successful real-world navigation using a Jackal UGV platform. </p>

## Method

<div>$x_2$</div>
<div>$$x_2$$</div>

{% include_relative _relative_includes/main_diagram.html %}
O4A consists of 4 learnable modules for image-goal navigation. Learning is entirely achieved using previously collected RGB observation trajectories $\tau_{\obs} =\{\obs_t\}_{t=1}^T$ and corresponding actions $\actiontraj = \{a_t\}_{t=1}^T$, without pose. The \textbf{local backbone} $\local$ (left) takes as input RGB images  to produce low-dimensional latent codes $\code \in \latentspace$. The \textbf{locomotion head} $\conn$ (center) uses pairs of latent codes to predict the action required to traverse from one latent code to the other (order matters), or the inability to do so through the $\mathtt{NOT\_CONNECTED}$ output. $\local$ and $\conn$ are then used to construct a directed graph $\graph$, where nodes represent images and edges represent traversability. The \textbf{forward dynamics head} (bottom right) $\fd$ is trained using edges from $\graph$ to predict the next code $\code_j$ given the current code $\code_i$ and an action $a_{ij} \in \actions$. The geometry of the graph $\graph$ is embedded in a neural network using a \textbf{geodesic regressor} $\georeg$ (top right), which outputs the shortest path length for any pair of codes. Once all the modules are trained, $\graph$ can be discarded, and $\georeg$ be used as part of a potential function, as illustrated in Figure \ref{fig:potentials} and detailed in Equation \ref{eq:nav_cost}.



## Simulation Results
What up

{% include_relative _relative_includes/row_videos.html title="Aloha" src_1="img/aloha/Aloha_easy.mp4" src_2="img/aloha/Aloha_medium.mp4" src_3="img/aloha/Aloha_hard.mp4" src_4="img/aloha/Aloha_very_hard.mp4" %}

{% include_relative _relative_includes/row_videos.html title="Annawan" src_1="img/annawan/Annawan_easy.mp4" src_2="img/annawan/Annawan_medium.mp4" src_3="img/annawan/Annawan_hard.mp4" src_4="img/annawan/Annawan_very_hard.mp4" %}

{% include_relative _relative_includes/row_videos.html title="Cantwell" src_1="img/cantwell/Cantwell_easy.mp4" src_2="img/cantwell/Cantwell_medium.mp4" src_3="img/cantwell/Cantwell_hard.mp4" src_4="img/cantwell/Cantwell_very_hard.mp4" %}

{% include_relative _relative_includes/row_videos.html title="Dunmor" src_1="img/dunmor/Dunmor_easy.mp4" src_2="img/dunmor/Dunmor_medium.mp4" src_3="img/dunmor/Dunmor_hard.mp4" src_4="img/dunmor/Dunmor_very_hard.mp4" %}

{% include_relative _relative_includes/row_videos.html title="Eastville" src_1="img/eastville/Eastville_easy.mp4" src_2="img/eastville/Eastville_medium.mp4" src_3="img/eastville/Eastville_hard.mp4" src_4="img/eastville/Eastville_very_hard.mp4" %}

{% include_relative _relative_includes/row_videos.html title="Hambleton" src_1="img/hambleton/Hambleton_easy.mp4" src_2="img/hambleton/Hambleton_medium.mp4" src_3="img/hambleton/Hambleton_hard.mp4" src_4="img/hambleton/Hambleton_very_hard.mp4" %}

{% include_relative _relative_includes/row_videos.html title="Nicut" src_1="img/nicut/Nicut_easy.mp4" src_2="img/nicut/Nicut_medium.mp4" src_3="img/nicut/Nicut_hard.mp4" src_4="img/nicut/Nicut_very_hard.mp4" %}

{% include_relative _relative_includes/row_videos.html title="Sodaville" src_1="img/sodaville/Sodaville_easy.mp4" src_2="img/sodaville/Sodaville_medium.mp4" src_3="img/sodaville/Sodaville_hard.mp4" src_4="img/sodaville/Sodaville_very_hard.mp4" %}

## Embeddings

{% include_relative _relative_includes/graph_embedding_images_1.html %}
{% include_relative _relative_includes/graph_embedding_images_2.html %}

## Detailed architectures and baselines

### O4A

{% include_relative _relative_includes/architecture_o4a.html %}

### SPTM

{% include_relative _relative_includes/architecture_sptm.html %}

### ViNG

{% include_relative _relative_includes/architecture_ving.html %}

## Hyperparams

{% include_relative _relative_includes/hyperparam_table.html %}

## Table

{% include_relative _relative_includes/main_table.html %}

{% include_relative _relative_includes/jackal_table.html %}

## Extra plots

{% include_relative _relative_includes/extended_results.html %}

## Augmentations

{% include_relative _relative_includes/augmentations_images.html src_1="img/augmentations/original.png" caption_1="Original" src_2="img/augmentations/brightness_contrast.png" caption_2="Brightness/contrast" src_3="img/augmentations/dropout.png" caption_3="Dropout" src_4="img/augmentations/gauss_noise.png" caption_4="Gaussian noise" src_5="img/augmentations/hue_saturation.png" caption_5="Hue saturation" %}
{% include_relative _relative_includes/augmentations_images.html src_1="img/augmentations/jitter.png" caption_1="Color jitter" src_2="img/augmentations/motion_blur.png" caption_2="Brightness/Motion blur" src_3="img/augmentations/perspective.png" caption_3="Perspective change" src_4="img/augmentations/sharpening.png" caption_4="Sharpening" src_5="img/augmentations/shift_scale_rotate.png" caption_5="Shift-Scale-Rotate" %}
