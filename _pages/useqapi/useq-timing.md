---
permalink: /useqapi/useq-timing/
title: "uSEQ API: Timing System"
sidebar:
  nav: "useq"
toc: true
toc_sticky: true

---

Most computer music systems run with a fixed quantum, but uSEQ is a bit different. It runs as fast as possible, a bit like a game will try to run at the fastest fps possible. 

Timing functions take a functional rendering approach; they take the current time ```t``` as an argument, and react accordingly. 

Much of the sequencing in uSEQ is done using phasors, ramps that rise from 0 to 1 in a fixed time period.

## Timing Variables

### `time`

The number of milliseconds since the last reset.

### `beat` 

A phasor, rising from 0-1 over the length of a beat

### `bar` 

A phasor, rising from 0-1 over the length of a bar

### `phrase` 

A phasor, rising from 0-1 over the length of a phrase

### `section` 

A phasor, rising from 0-1 over the length of a section

## Timing functions

### `set-bpm <bpm> (<change threshold> = 0)`

Set the speed of the sequencer in beats per minute

| Parameter | Description | Range |
| --- | --- | --- |
| bpm | beats per minute | any |
| change threshold | the bpm will only change if the difference between the current bpm and the new bpm is more than this threshold. Use this to help stabilise the bpm when it is tracked from an external source using getbpm, as small changes in bpm get disrupt phasors and patterns. | >=0 |

### `get-bpm <input>`

Set the speed of the sequencer in beats per minute

| Parameter | Description | Range |
| --- | --- | --- |
| input | get the estimated BPM of the signal on either input 1 or 2, based on the average time between pulses | 1 or 2 |

Use this to get the bpm of the uSEQs sequencing engine:

```
(q0 (set-bpm (get-bpm 1)))
```

### `set-time-sig <numerator> <denominator>`

Set the time signature of the sequencer

| Parameter | Description | Range |
| --- | --- | --- |
| numerator | number of beats in a measure | any |
| denominator | the length of a beat | any |
