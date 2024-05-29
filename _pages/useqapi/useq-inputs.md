---
permalink: /useqapi/useq-inputs/
title: "uSEQ API: Inputs"
sidebar:
  nav: "useq"
toc: true
toc_sticky: true

---

# Digital Inputs

## `in1`

Returns the value of digital input 1 

Example: to echo digital input 1 to digital output 1
```
(d1 (in1))
```

## `in2`

Returns the value of digital input 2

Example: to echo digital input 2 to digital output 1
```
(d1 (in2))
```

# Analog Inputs

## `ain1`

Returns the value of analogue input 1

Example: to echo analogue input 1 to analogue output 1
```
(a1 (ain1))
```

## `ain2`

Returns the value of analogue input 2

Example: to echo analogue input 2 to analogue output 2
```
(a2 (ain2))
```

## `swm <index>`

Read the value of a momentary switch

| Parameter | Description | Range |
| --- | --- | --- |
| index | The index of the switch | 1 or 2 |


Control the speed of a square wave with momentary switch 1
```
(d2 (sqr (fast (+ 1 (swm 1)) beat)))
```

## `swt <index>`

Read the value of a toggle switch

| Parameter | Description | Range |
| --- | --- | --- |
| index | The index of the switch | any|


Control the speed of a square wave with toggle switch 2
```
(d4 (sqr (fast (scale (swt 1) 0 1 3 8) beat)))
```


