---
permalink: /quickmodes/wavefolder/
title: "uSEQ Quick Mode: Wave Folder"
sidebar:
  nav: "useq"
toc: true
toc_sticky: true

---

## Introduction

A wavefolder acts on a CV signal and reflects it downwards or upwards when it's under or over a threshold.  The code processes the signal on CV input 1, and sends the wavefolded version to a1.

You can adjust ```highthreshold```, ```lowthreshold``` and ```gain``` to change the effect.

## Code 

{% include useqperform.html params="nosave&txt=/assets/code/wavefolder.useq" %}

