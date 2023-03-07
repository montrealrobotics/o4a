---
layout: default
title: Project template
permalink: index.html
---

<h1 style="text-align: center;">One-4-All: Neural Potential Fields for Embodied Navigation</h1>

{% include_relative _relative_includes/authors.html %}

{% include_relative _relative_includes/badges.html %}

## Abstract

<p style='text-align: justify;'> A fundamental task in robotics is to navigate between two locations. In particular, real-world navigation can require long-horizon planning using high-dimensional RGB images, which poses a substantial challenge for end-to-end learning-based approaches. Current semi-parametric methods instead achieve long-horizon navigation by combining learned modules with a topological memory of the environment, often represented as a graph over previously collected images. However, using these graphs in practice typically involves tuning a number of pruning heuristics to avoid spurious edges, limit runtime memory usage and allow reasonably fast graph queries. In this work, we present One-4-All (O4A), a method leveraging self-supervised and manifold learning to obtain a graph-free, end-to-end navigation pipeline in which the goal is specified as an image. Navigation is achieved by greedily minimizing a potential function defined continuously over the O4A latent space. Our system is trained offline on non-expert exploration sequences of RGB data and controls, and does not require any depth or pose measurements. We show that O4A can reach long-range goals in 8 simulated Gibson indoor environments, and further demonstrate successful real-world navigation using a Jackal UGV platform. </p>

{% include_relative _relative_includes/main_video.html %}

## Videos

{% include_relative _relative_includes/row_videos.html title="Aloha" src_1="img/aloha/Aloha_easy.mp4" src_2="img/aloha/Aloha_medium.mp4" src_3="img/aloha/Aloha_hard.mp4" src_4="img/aloha/Aloha_very_hard.mp4" %}

{% include_relative _relative_includes/row_videos.html title="Annawan" src_1="img/annawan/Annawan_easy.mp4" src_2="img/annawan/Annawan_medium.mp4" src_3="img/annawan/Annawan_hard.mp4" src_4="img/annawan/Annawan_very_hard.mp4" %}

{% include_relative _relative_includes/row_videos.html title="Cantwell" src_1="img/cantwell/Cantwell_easy.mp4" src_2="img/cantwell/Cantwell_medium.mp4" src_3="img/cantwell/Cantwell_hard.mp4" src_4="img/cantwell/Cantwell_very_hard.mp4" %}

{% include_relative _relative_includes/row_videos.html title="Dunmor" src_1="img/dunmor/Dunmor_easy.mp4" src_2="img/dunmor/Dunmor_medium.mp4" src_3="img/dunmor/Dunmor_hard.mp4" src_4="img/dunmor/Dunmor_very_hard.mp4" %}

{% include_relative _relative_includes/row_videos.html title="Eastville" src_1="img/eastville/Eastville_easy.mp4" src_2="img/eastville/Eastville_medium.mp4" src_3="img/eastville/Eastville_hard.mp4" src_4="img/eastville/Eastville_very_hard.mp4" %}

{% include_relative _relative_includes/row_videos.html title="Hambleton" src_1="img/hambleton/Hambleton_easy.mp4" src_2="img/hambleton/Hambleton_medium.mp4" src_3="img/hambleton/Hambleton_hard.mp4" src_4="img/hambleton/Hambleton_very_hard.mp4" %}

{% include_relative _relative_includes/row_videos.html title="Nicut" src_1="img/nicut/Nicut_easy.mp4" src_2="img/nicut/Nicut_medium.mp4" src_3="img/nicut/Nicut_hard.mp4" src_4="img/nicut/Nicut_very_hard.mp4" %}

{% include_relative _relative_includes/row_videos.html title="Sodaville" src_1="img/sodaville/Sodaville_easy.mp4" src_2="img/sodaville/Sodaville_medium.mp4" src_3="img/sodaville/Sodaville_hard.mp4" src_4="img/sodaville/Sodaville_very_hard.mp4" %}

## Embeddings

{% include_relative _relative_includes/graph_embedding_images.html %}

## Main diagram

{% include_relative _relative_includes/main_diagram.html %}

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
