---
permalink: /useqapi/useq-sequencing/
title: "uSEQ API: Sequence Generation and Manipulation"
sidebar:
  nav: "useq"
toc: true
toc_sticky: true

---

These functions are for pattern and waveform generation and manipulation

## Waveforms

### `sqr <phasor>`

Turns a phasor into a square wave.

| Parameter | Description | Range |
| --- | --- | --- |
| phasor | A phasor (e.g. bar, beat), or any other value between 0 and 1 | 0-1 |


### `pulse <pulse width> <phasor>`

Outputs a pulse wave.

| Parameter | Description | Range |
| --- | --- | --- |
| pulse width | The relative width of each pulse | 0-1 |
| phasor | A phasor (e.g. bar, beat) | 0-1 |

### `tri <duty cycle> <phasor>`

Variable duty triangle wave

| Parameter | Description | Range |
| --- | --- | --- |
| duty | The point within the phase when the triangle should reach its peak | 0-1 |
| phasor | A phasor (e.g. bar, beat) | 0-1 |

This waveform is really useful for creating envelopes

e.g. an envelope with fast attack, long decay, with the peak near the beginning
```clojure
(a1 (tri 0.1 (fast 4 bar)))
```

or transform the straight triangle edges into curves with ```pow```

```clojure
(a1 (pow (tri 0.2 (fast 4 bar)) 0.6))
```



## List-based patterns

### `from-list <list> <phasor>`  (alias: `seq`)

Read an item from a list, using a normalised index.

Items in the list are evaluated before being returned, so you can use functions, variables, and generally any valid expressions in the list.

| Parameter | Description | Range |
| --- | --- | --- |
| list | A list of values | any |
| position | A normalised index | 0-1 |


```clojure
(from-list [1 2 3 4] 0.6) ; => 3
```

```clojure
(from-list [1 2 3 4] bar))
```

```clojure
(seq [1 (/ phrase 2)] bar))
```

```clojure
(from-list '(1 phrase) bar))
```

```clojure
(from-list [1 2 (from-list [1 2] bar)] 
    (slow 2 bar))

```

### `flatten <list>`

Take a list that might contain other lists or functions, evaluate them all in turn and collect them in a one dimensional list.

| Parameter | Description | Range |
| --- | --- | --- |
| list | A list of values | any |

Examples:

```clojure
(flatten [1 [2 3] bar])
```


```clojure
(define part1 [1 2 3])
(define part2 [4 5])
(flatten [part1 part2 part1])
```

### `from-flattened-list <list> <position/phasor>` (alias: `flatseq`)

Read an item from a list, using a normalised index, but flatten then list first (using `flattened`).

| Parameter | Description        | Range |
|-----------|--------------------|-------|
| list      | A list of values   | any   |
| position  | A normalised index | 0-1   |

This is a shortcut, equivelant to;

```clojure
(from-flattened-list [0 1 2 [3 4 5]] 0.75)
```

### `gates <list> (<pulse width> = 0.5) <phasor>`

Output a sequence of gates, with variable pulse width.

| Parameter   | Description                                          | Range  |
|-------------|------------------------------------------------------|--------|
| list        | A list of gate values                                | 0 or 1 |
| phasor      | The sequence is output once per cycle of the phasor  | 0-1    |
| speed       | Modify the speed of the phasor                       | >= 1   |
| pulse width | Optional, default: 0.5. The pulse width of the gates | 0-1    |

```clojure
(d2 (gates [0 1 1 0  1 1 1 0  1 1 0 1  1 0 0 1] (+ (swm 1) 0.3) bar))
```

```clojure
(d2 (gates [0 1 1 0 1 0 0 (swt)] 0.5 (fast 2 bar))))
```

### `gatesw <list> <phasor>`

Output a sequence of gates, with pulse width controlled from values in the list

| Parameter | Description                                                                                                         | Range |
|-----------|---------------------------------------------------------------------------------------------------------------------|-------|
| list      | A list of gate/pulse width values, varying from 0 (0% pulse width) to 9 (100% pulse width / tie into the next note) | 0 - 9 |
| phasor    | The sequence is output once per cycle of the phasor                                                                 | 0-1   |

```clojure
(d2 (gatesw [9 9 5 9 3 0 3 8] (fast 2 bar)))
```

### `trigs <list> (<pulsewidth>) <phasor>`

Output a sequence of gates, each of which can have a different amplitude, determined by a list of amplitudes.

| Parameter  | Description                                                                   | Range |
|------------|-------------------------------------------------------------------------------|-------|
| list       | A list of trigger values, varying from 0 (0% amplitude) to 9 (100% amplitude) | 0 - 9 |
| phasor     | The sequence is output once per cycle of the phasor                           | 0-1   |
| pulseWidth | Optional, default: 0.1. Modify the pulse width of the trigger                 | 0 - 1 |

```clojure
(s3 (trigs [0 1 9 0 1] (fast 2 bar)))
;; With a higher pulse width:
(s4 (trigs [0 1 9 0 1] 0.7 (fast 2 bar)))
```

### `interp <list> <phasor>`

Interpolate across a list, using a phasor.  This function acts as if the list of values describes a continuous envelope, and returns the value at a position in that envelope.  e.g.

| Parameter | Description      | Range    |
|-----------|------------------|----------|
| values    | A list of values | any list |
| phasor    | A phasor         | 0 - 1    |

```clojure
(interp [0 0.5 0] 0.75)
```

describes a triangle shape, and returns the value that it 75% along the triangle (0.25).

```clojure
(a1 (interp [1 0.5 0 0.6 1] bar))
```

makes a roughly inverted triangle, and plays it once per bar on PWM output 1

```clojure
(a2 (interp [0 (sin phrase) 1] section))
```

creates a slowly changing envelope that loops every section, sent to PWM output 2

## Phasor Processing

### `step <count> (<offset> = 0) <phasor>`

Turn a phasor into an integer counter.

| Parameter | Description | Range |
| --- | --- | --- |
| count | the number of divisions to divide the phasor into | >0 |
| offset | Optional. the point to start the counter from | any |
| phasor | A phasor | 0 - 1 |

## Euclidean Sequencing

### `euclid <n> <k> (<pulseWidth> = 0.5) <phasor>`

Generate a sequence of gates based on euclidean sequencing.

For more info: https://erikdemaine.org/papers/DeepRhythms_CGTA/paper.pdf

Demaine, E.D., Gomez-Martin, F., Meijer, H., Rappaport, D., Taslakian, P., Toussaint, G.T., Winograd, T. and Wood, D.R., 2009. The distance geometry of music. Computational geometry, 42(5), pp.429-454.

| Parameter  | Description                                              | Range     |
|------------|----------------------------------------------------------|-----------|
| n          | the number of beats to fit into the period of the phasor | >0        |
| k          | the number of beats to fit, equally spaced, into n beats | >0        |
| pulseWidth | (optional) Width of the gates                            | >0 and <1 |
| phasor     | A phasor                                                 | 0 - 1     |

```clojure
(d1 (euclid 16 (step phrase 4 4) 0.3 bar))
(d2 (euclid 32 8 0.1 bar))
(d3 (euclid 16 6 (step (fast 4 phrase) bar)))
```

## Sequencing with Ratios

### `rpulse <ratios> <pulsewidth> <phasor>`

Generate pulses by dividing a phasor into sections according to a list of timing ratios, and emiting a pulse at the start of each section.


| Parameter  | Description                                              | Range     |
|------------|----------------------------------------------------------|-----------|
| ratios     | a list of ratios of time periods that the phasor will be divided into | any        |
| pulseWidth | width of the pulses                                      | >0 and <1 |
| phasor     | A phasor                                                 | 0 - 1     |


The relative size of the numbers in the ratios list determines how the bar will be divided up.

This is great for triggering drums and envelopes, or gating other signals.


Here are some code examples with graphs of the output.


```clojure
(d1 (rpulse [1 1] 0.5 bar))
```

![ratio sequencing example](/assets/images/useq/api/rpulse_1_1.jpg){:class="img-responsive"}

(also) generate 2 equal size beats per bar

```clojure
(d1 (rpulse [15 15] 0.5 bar))
```
![ratio sequencing example](/assets/images/useq/api/rpulse_1_1.jpg){:class="img-responsive"}



```clojure
(d1 (rpulse [3 3 3] 0.2 bar))
```
![ratio sequencing example](/assets/images/useq/api/rpulse_3_3_3.jpg){:class="img-responsive"}


```clojure
(d1 (rpulse [1 1 2 3 1] 0.1 bar))
```
![ratio sequencing example](/assets/images/useq/api/rpulse_11231.jpg){:class="img-responsive"}


```clojure
(d1 (rpulse [3 1 1 1 3 2 1] 0.9 bar))
```
![ratio sequencing example](/assets/images/useq/api/rpulse-09.jpg){:class="img-responsive"}


### `rstep <ratios> <phasor>`

Transform a phasor (or other signal) into a series of steps.  The list of ratios (as with ```rpulse``` above) determines the timing and length of the steps.  The amplitude of the step is determined by the phasor's value at the beginning of the step. 

It's the equivelant of triggering a sample and hold on the phasor, according to the timing of the ratios.


| Parameter  | Description                                              | Range     |
|------------|----------------------------------------------------------|-----------|
| ratios     | a list of ratios of time periods that the phasor will be divided into | any        |
| phasor     | A phasor                                                 | 0 - 1     |


You could use this as a signal on its own, or use it as a phasor to index into other sequencing functions (e.g. ```interp```)

```clojure
(d1 (rstep [1 1 2] bar))
```

![ratio sequencing example](/assets/images/useq/api/rstep1.jpg){:class="img-responsive"}

```clojure
(d1 (rstep [6 6 6 2 2 2] bar))
```

![ratio sequencing example](/assets/images/useq/api/rstep2.jpg){:class="img-responsive"}


```rstep``` can be used as a phasor for other functions. This code below is a bit like using a sample-and-hold on an envelope, timings determined by the ratios.

```clojure
(a2 (interp [0 0.2 1 0] (rstep [3 1 1 1] (slow 4 bar))))
```



### `ridx <ratios> <phasor>`

Transform a phasor (or other signal) into a series of steps.  The list of ratios (as with ```rpulse``` above) determines the timing and length of the steps.  The amplitude of the step is determined by the index of the time ratio in the list of ratios.  It's a bit like ```rstep``` above, but the steps occur at even divisions of the range 0-1, depending on the number of time ratios specificied. This means ```ridx``` is useful as a phasor for indexing other lists (e.g. using ```seq```); you can control the timing of when you move through items in another list.



| Parameter  | Description                                              | Range     |
|------------|----------------------------------------------------------|-----------|
| ratios     | a list of ratios of time periods that the phasor will be divided into | any        |
| phasor     | A phasor                                                 | 0 - 1     |



```clojure
(d1 (ridx [1 1 1 3] bar))
```

![ratio sequencing example](/assets/images/useq/api/ridx1.jpg){:class="img-responsive"}


```clojure
(d1 (ridx [1 1 15 3 3] bar))
```

![ratio sequencing example](/assets/images/useq/api/ridx2.jpg){:class="img-responsive"}


### `rwarp <ratios> <phasor>`

This works similarly to ```ridx``` above, except that the function produces a ramp between points at the start of each time period.  This could be useful with functions like ```gatesw```, which take a ramp as input and use this to control pulse width. In this case, you could control the timing of when you move though each gate.


| Parameter  | Description                                              | Range     |
|------------|----------------------------------------------------------|-----------|
| ratios     | a list of ratios of time periods that the phasor will be divided into | any        |
| phasor     | A phasor                                                 | 0 - 1     |



```clojure
(d1 (rwarp [1 5 12] bar))
```

![ratio sequencing example](/assets/images/useq/api/rwarp1.jpg){:class="img-responsive"}


```clojure
(d1 (rwarp [200 40 10] bar))
```

![ratio sequencing example](/assets/images/useq/api/rwarp2.jpg){:class="img-responsive"}


```clojure
;fibonacci ratios
(d1 (rwarp [1 2 3 5 8 13 21 34 55] bar))
```

![ratio sequencing example](/assets/images/useq/api/rwarp3.jpg){:class="img-responsive"}

