---
permalink: /tutorials/chaotic/
title: "uSEQ Tutorial: Nonlinear and Chaotic Oscillators and Patterns"
sidebar:
  nav: "useq"
toc: true
toc_sticky: true

---

## Introduction

uSEQ can act as a source of nonlinear and chaotic oscillation, using the scheduler to update complex systems equations.

## The Logistic Map

This is a simple discrete dynamical system, with one single parameter which moves the system between statis and chaos.  In the example below, cv input 1 controls this parameter, and the momentary button resets the system.  The signal is output to ```a1``` and ```s1```. Also, a pattern is generated on ```d1``` by setting this output to 1 when the signal is over a threhold, set by CV input 2.

## Code

This example uses the scheduler, which runs a function periodically, n times per bar.  This function updates the logistic map.  The scheduler is used here because it runs at regular intervals, unlike other time-based functions in uSEQ which run as quickly as possible, and render their value as a function of the current time.  In this case, its output is non-deterministic, so we can't calculate its value as a function of time. Instead, we need to take the previous value and use this to calculate the next value, and do this repeatedly at a fixed speed so there's no jitter in the output.

{% include useqperform.html params="nosave&txt=/assets/code/logisticmap.useq" %}

