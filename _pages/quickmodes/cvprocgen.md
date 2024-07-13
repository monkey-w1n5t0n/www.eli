---
permalink: /quickmodes/cvprocessor/
title: "uSEQ Quick Mode: CV Processor and Gate Generator"
sidebar:
  nav: "useq"
toc: true
toc_sticky: true

---

## Introduction

This mode takes input from the two CV inputs, and puts them together in different ways: a mix, an inverted mix, and ring modulation.  It also generates gate patterns based on the relationship between the two CV signals, outputing true if (a) cv 1 > cv 2, (b) cv 1 < cv 2 and (c) cv 1 and cv 2 are similar within a tolerance.  On top of these functions, use the attenuverter controls to adjust the outputs.

## Code 

Run each statement below as needed.

{% include useqperform.html params="nosave&txt=/assets/code/cvprocgen.useq" %}

