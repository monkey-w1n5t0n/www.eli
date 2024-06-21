---
permalink: /tutorials/envgate/
title: "uSEQ Tutorial: Sequencing with Gated Envelopes"
sidebar:
  nav: "useq"
toc: true
toc_sticky: true

---

## Introduction

With uSEQ's CV outs, you can skip the need for an envelope generator, and create the envelopes yourself, before applying a rhythm to them.  This example starts by speeding up am inverted bar phasor to 16 times per bar, giving 16 invididual note envelopes.  A second pattern is then made which is a series of gates.  This pattern is multiplied with the envelopes, so that some envelopes are switch off, creating a rhythmic sequence.

## Video

/assets/audio/useq-phase.mp3

## Code 

{% include useqperform.html params="nosave&txt=/assets/code/phasepiece.useq" %}

