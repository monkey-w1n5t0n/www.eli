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

The number of seconds since the module was switched on.

### `t`

Logical time;  the number of seconds since the last reset.

### `beat` 

A phasor, rising from 0-1 over the length of a beat.

### `bar` 

A phasor, rising from 0-1 over the length of a bar.

### `phrase` 

A phasor, rising from 0-1 over the length of a phrase.


### `section` 

A phasor, rising from 0-1 over the length of a section.

### `bpm`

The tempo in beats per minute.

### `bps`

The tempo in beats per second.


## Timing functions

### `set-bpm <bpm>`

Set the speed of the sequencer in beats per minute.

| Parameter | Description | Range |
| --- | --- | --- |
| bpm | beats per minute | >0 |


### `set-time-sig <numerator> <denominator>`

Set the time signature of the sequencer.

| Parameter | Description | Range |
| --- | --- | --- |
| numerator | number of beats in a measure | any |
| denominator | the length of a beat | any |

### `get-clock-source`

Returns the source for the clock, either internal (0), gate input 1 (1) or gate input 2 (2).

### `set-clock-int`

Sets the clock source to the internal clock

### `set-clock-ext`

Sets the clock source to be driven by either gate input 1 or 2.  uSEQ will calculate the current bpm from the time between rising edges of the gate.  It will also stay in phase by reseting uSEQs logical time to zero, either at the start of each phrase if the bpm tracking is stable, or every bar if the bpm tracking is unstable.  If you change the speed of the external signal, uSEQ will try to adapt to it as quickly as possible. uSEQ will track bars and phrases by counting from the first rising edge it receives.  You can reset this information using `reset-clock-ext`. 

| Parameter | Description | Range |
| --- | --- | --- |
| input number | which gate input should uSEQ synchronise to? | 1 or 2|
| clock divisor | the number of clocks per beat | >0 |

### `reset-clock-int`

Reset the internal clock, starting the logical time from 0.

### `reset-clock-ext`

Reset the data used by the external clock tracker.  If you are changing from one external clock to another, it might be useful to run this.  After running this function, the first rising edge to be detected will be assumed to be the start of the first bar.
