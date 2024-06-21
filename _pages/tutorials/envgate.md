---
permalink: /tutorials/envgate/
title: "uSEQ Tutorial: Sequencing with Gated Envelopes"
sidebar:
  nav: "useq"
toc: true
toc_sticky: true

---

## Introduction

With uSEQ's CV outs, you can skip the need for an envelope generator, and create the envelopes yourself, before applying a rhythm to them.  This example starts by speeding up an inverted bar phasor to 16 times per bar, giving 16 invididual note envelopes.  A second pattern is then made which is a series of gates.  This pattern is multiplied with the envelopes, so that some envelopes are switched off or scaled down, creating a rhythmic sequence.

## Video

<iframe width="560" height="315" src="https://www.youtube.com/embed/0nk_4zVpwUk?si=G591iINYlMI4xmFX" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>


## Code 

{% include useqperform.html params="nosave&txt=/assets/code/envgate.useq" %}

