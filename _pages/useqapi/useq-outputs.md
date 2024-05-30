---
permalink: /useqapi/useq-outputs/
title: "uSEQ API: Outputs and Callbacks"
sidebar:
  nav: "useq"
toc: true
toc_sticky: true

---

## Sequencing Callback Functions for CV

### `a1 <form>`

Specify a function to calculate the value of analog output 1, calculated every quantum

```
(a1 (* (fromList '(0.4 0.1) bar) 3))
```
```
(a1 0)
```

### `a2 <form>`

Specify a function to calculate the value of analog output 2, calculated every quantum

```
(a2 (* (fromList '(0.4 0.1) bar) 3))
```
```
(a2 0)
```

### `a3 <form>`

Specify a function to calculate the value of analog output 3, calculated every quantum

```
(a3 (* (fromList '(0.4 0.1) bar) 3))
```
```
(a3 0)
```

## Sequencing Callback Functions for Pulses

### `d1 <form>`

Specify a function to calculate the value of digital output 1, calculated every quantum

```
(d1 (sqr beat))
```
```
(d1 0)
```

### `d2 <form>`

Specify a function to calculate the value of digital output 2, calculated every quantum

```
(d2 (sqr beat))
```
```
(d2 0)
```

### `d3 <form>`

Specify a function to calculate the value of digital output 3, calculated every quantum

```
(d3 (sqr beat))
```
```
(d3 0)
```

## Sequencing Callback Functions for Serial/MIDI

### `s[x] <form>`

(where x is a number between 1 and 8)

Send a sequence over the USB serial connection.  This sequence can be decoded by your USB client software.  To differentiate from other text send by the module, serial data is send in a 11 byte message in the following format:
[31][0][index of serial stream][8 bytes representing a double]

The useq editor can decode these messages and forward them as MIDI.


## Timed Sequencing Callback Functions

### `q0 <form>`

Specify a function to run at the start of each quantisation period (by default, each bar)

```
(q0 (print bar))
```
```
(q0 0)
```
