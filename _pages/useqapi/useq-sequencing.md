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
| phasor | A phasor (e.g. bar, beat) | 0-1 |
| pulse width | The relative width of each pulse | 0-1 |


## List-based patterns

### `from-list <list> <position/phasor>`  (alias: `seq`)

Read an item from a list, using a normalised index.

Items in the list are evaluated before being returned, so you can use functions, variables, and generally any valid expression in the list.

| Parameter | Description | Range |
| --- | --- | --- |
| list | A list of values | any |
| position | A normalised index | 0-1 |

Examples:

```
(fromList [1 2 3 4] 0.6)) ; => 3
```

```
(fromList [1 2 3 4] bar))
```

```
(seq [1 (/ phrase 2)] bar))
```

```
(fromList '(1 phrase) bar))
```

```
(fromList [1 2 (fromList [1 2] bar)] 
    (slow 2 bar))

```

### `flat <list>`

Take a list that might contain other lists or functions, evaluate them all in turn and collect them in a one dimensional list.

| Parameter | Description | Range |
| --- | --- | --- |
| list | A list of values | any |

Examples:

```
(flat [1 [2 3] bar])
```


```
(define part1 [1 2 3])
(define part2 [4 5])
(flat [part1 part2 part1])
```

### `from-flat-list <list> <position/phasor>` (alias: `flat-seq`)

Read an item from a list, using a normalised index, but flatten then list first (using `flat`).

| Parameter | Description | Range |
| --- | --- | --- |
| list | A list of values | any |
| position | A normalised index | 0-1 |

This is a shortcut, equivelant to;

```
(from-flat-list (flat <list>) position)
```

### `gates <list> (<pulse width> = 0.5) <phasor>`

Output a sequence of gates, with variable pulse width.

| Parameter | Description | Range |
| --- | --- | --- |
| list | A list of gate values | 0 or 1 |
| phasor | The sequence is output once per cycle of the phasor | 0-1 |
| speed | Modify the speed of the phasor | >= 1 |
| pulse width | Optional, default: 0.5. The pulse width of the gates | 0-1 |

```
(d2 (gates [0 1 1 0  1 1 1 0  1 1 0 1  1 0 0 1] (+ (swm 1) 0.3) bar))
```

```
(d2 (gates [0 1 1 0 1 0 0 (swt 1)] 0.5 (fast 2 bar))))
```

### `gatesw <list> <phasor>`

Output a sequence of gates, with pulse width controlled from values in the list

| Parameter | Description | Range |
| --- | --- | --- |
| list | A list of gate/pulse width values, varying from 0 (0% pulse width) to 9 (100% pulse width / tie into the next note) | 0 - 9 |
| phasor | The sequence is output once per cycle of the phasor | 0-1 |
| speed | Optional, default: 1. Modify the speed of the phasor | >= 1 |

```
(d2 (gatesw [9 9 5 9 3 0 3 8] (fast 2 bar)))
```

### `trigs <list> <phasor> (<speed>) (<pulsewidth>)`

Output a sequence of gates, with pulse width controlled from values in the list

| Parameter | Description | Range |
| --- | --- | --- |
| list | A list of trigger values, varying from 0 (0% amplitude) to 9 (100% amplitude) | 0 - 9 |
| phasor | The sequence is output once per cycle of the phasor | 0-1 |
| speed | Optional, default: 1. Modify the speed of the phasor | >= 1 |
| pulseWidth | Optional, default: 0.1. Modify the pulse width of the trigger | 0 - 1 |

```
(s3 (trigs [0 1 9 0 1] (fast 2 bar)))
```

### `interp <list> <phasor>`

Interpolate across a list, using a phasor.  This function acts as if the list of values describes a continuous envelope, and returns the value at a position in that envelope.  e.g.

| Parameter | Description | Range |
| --- | --- | --- |
| values | A list of values | any list |
| phasor | A phasor | 0 - 1 |

```
(interp [0 0.5 0] 0.75)
```

describes a triangle shape, and returns the value that it 75% along the triangle (0.25).

```
(a1 (interp [1 0.5 0 0.6 1] bar))
```

makes a roughly inverted triangle, and plays it once per bar on PWM output 1

```
(a2 (interp [0 (sin phrase) 1] section))
```

creates a slowly changing envelope that loops every section, sent to PWM output 2

## Phasor Processing

### `looph <phasor> <looppoint>`

Take a phasor, and make it wrap/loop around at a certain point

| Parameter | Description | Range |
| --- | --- | --- |
| phasor | A phasor | 0 - 1 |
| looppoint | The point at which the phasor should wrap/loop | 0-1 |

E.g. the code below will play the first half of the sequence repeatedly

```
(d2 (gatesw (quote 9 9 5 9 3 0 3 8) (looph bar 0.5) 2))
```

### `step <phasor> <count> (<offset> = 0)`

Turn a phasor into an integer counter

| Parameter | Description | Range |
| --- | --- | --- |
| phasor | A phasor | 0 - 1 |
| count | the number of divisions to divide the phasor into | >0 |
| offset | Optional. the point to start the counter from | any |

## Pattern Generation

### `euclid <phasor> <n> <k> (<offset> = 0) (<pulseWidth> = 0.5)`

Generate a sequence of gates based on euclidean sequencing.

For more info: https://erikdemaine.org/papers/DeepRhythms_CGTA/paper.pdf

Demaine, E.D., Gomez-Martin, F., Meijer, H., Rappaport, D., Taslakian, P., Toussaint, G.T., Winograd, T. and Wood, D.R., 2009. The distance geometry of music. Computational geometry, 42(5), pp.429-454.

| Parameter | Description | Range |
| --- | --- | --- |
| phasor | A phasor | 0 - 1 |
| n | the number of beats to fit into the period of the phasor | >0 |
| k | the number of beats to fit, equally spaced, into n beats | >0 |
| offset | Optional. A phase offset, measured in beats | >0 |
| pulseWidth | Optional. Width of the gates | >0 and <1 |

```
(d1 (euclid bar 16 (step phrase 4 4) 2))
(d2 (euclid bar 32 8 4 0.1))
(d3 (euclid bar 16 6 (step (fast 4 phrase) 4)))
```
