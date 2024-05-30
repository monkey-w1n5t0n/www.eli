---
permalink: /useqapi/useq-system/
title: "uSEQ API: System Utilities"
sidebar:
  nav: "useq"
toc: true
toc_sticky: true

---

## `perf`

Returns information on system performance and free memory

```
(perf)
```

## `timeit <statement>`

Returns the amount if time it took run evaluate ```statement``` in microseconds

| Parameter | Description | Range |
| --- | --- | --- |
| statement | Any LISP statement | - |


```
(timeit (+ 1 1))
```
