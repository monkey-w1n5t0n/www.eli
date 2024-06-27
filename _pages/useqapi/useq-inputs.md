---
permalink: /useqapi/useq-inputs/
title: "uSEQ API: Inputs"
sidebar:
  nav: "useq"
toc: true
toc_sticky: true

---

## Digital Inputs

### `in1`

Returns the value of digital input 1.

Example: to echo digital input 1 to digital output 1
```
(d1 (in1))
```

### `in2`

Returns the value of digital input 2.

Example: to echo digital input 2 to digital output 1
```
(d1 (in2))
```

## Analog Inputs

### `ain1`

Returns the value of CV input 1

Example: to echo CV input 1 to CV output 1
```
(a1 (ain1))
```

### `ain2`

Returns the value of CV input 2

Example: to echo CV input 2 to CV output 2
```
(a2 (ain2))
```

### `swm`

Read the value of the momentary switch


Control the speed of a square wave with momentary switch 1
```
(d2 (sqr (fast (+ 1 (swm)) beat)))
```

### `swt`

Read the value of the toggle switch

| Position | Value  |
| --- | --- |
| Left | 0 |
| Centre | 1 |
| Right | 2 |



Control the speed of a square wave with the toggle switch 
```
(d4 (sqr (fast (scale (swt) 0 2 3 8) beat)))
```


