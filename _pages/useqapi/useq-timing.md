---
permalink: /useqapi/useq-timing/
title: "uSEQ API: Timing System"
sidebar:
  nav: "useq"

---

Most computer music systems run with a fixed quantum, but uSEQ is a bit different. It runs as fast as possible, a bit like a game will try to run at the fastest fps possible. 

Timing functions take a functional rendering approach; they take the current time ```t``` as an argument, and react accordingly. 

Much of the sequencing in uSEQ is done using phasors, ramps that rise from 0 to 1 in a fixed time period.

# Timing Variables

## `time`

The number of milliseconds since the last reset.

## `beat` 

A phasor, rising from 0-1 over the length of a beat

## `bar` 

A phasor, rising from 0-1 over the length of a bar

## `phrase` 

A phasor, rising from 0-1 over the length of a phrase

## `section` 

A phasor, rising from 0-1 over the length of a section

