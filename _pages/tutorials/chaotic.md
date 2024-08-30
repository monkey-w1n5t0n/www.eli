---
permalink: /tutorials/chaotic/
title: "uSEQ Tutorial: Nonlinear and Chaotic Oscillators and Patterns"
sidebar:
  nav: "useq"
toc: true
toc_sticky: true

---

## Introduction

uSEQ can act as a source of nonlinear and chaotic oscillation, using the scheduler to update complex systems equation, rendering fractals and dynamical systems.  These systems can make great sources of oscillation, that vary in shape and texture over long periods.

## The Logistic Map

This is a simple discrete dynamical system, with one single parameter which moves the system between statis and chaos.  In the example below, cv input 1 controls this parameter, and the momentary button resets the system.  The signal is output to ```a1``` and ```s1```. Also, a pattern is generated on ```d1``` by setting this output to 1 when the signal is over a threhold, set by CV input 2.

## Code

This example uses the scheduler, which runs a function periodically, n times per bar.  This function updates the logistic map.  The scheduler is used here because it runs at regular intervals, unlike other time-based functions in uSEQ which run as quickly as possible, and render their value as a function of the current time.  In this case, its output is dependent on the previous time step, so we simply can't calculate its value as a function of time. Instead, we need to take the previous value and use this to calculate the next value, and do this repeatedly at a fixed speed so there's no jitter in the output.

{% include useqperform.html params="nosave&txt=/assets/code/logisticmap.useq" %}

## The Lorenz Attractor

This is the famous *butterfly attractor* created by Edward Lorenz in the 1960s.

![Lorenz Attractor (Image: Wikimedia Commons)](/assets/images/Lorenz_attractor_boxed.svg){:class="img-responsive"}
*Image: Wikimedia Commons*

This code below simulates a Lorenz system, and outputs the coordinates of the attractor as control voltages.  Use CV in 1 and 2 to alter the parameters and behaviour of the system, and the momentary button to reset the attractor.  Some combinations of parameters may saturate the system, while others will push the system into varied patterns.

## Code

Run each section separately, as needed.

{% include useqperform.html params="nosave&txt=/assets/code/lorenz.useq" %}





## More Strange Attractors

You can find the equations for more attractors here: [https://www.dynamicmath.xyz/strange-attractors](https://www.dynamicmath.xyz/strange-attractors/)

Based on the example above, you should be able to program in the equations from these systems instead.

