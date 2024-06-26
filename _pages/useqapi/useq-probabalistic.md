---
permalink: /useqapi/useq-prob/
title: "uSEQ API: Probabilities"
sidebar:
  nav: "useq"
toc: true
toc_sticky: true

---


### `random <low> <high>`

Generates a random integer

| Parameter | Description | Range |
| --- | --- | --- |
| low | the lowest random integer the function should generate | any |
| high | the highest random integer the function should generate | any |

Create a random number between 0 and 1
```
(* (random 0 1000) 0.001)
```

