---
permalink: /useqapi/useq-scheduling/
title: "uSEQ API: Scheduler Functions"
sidebar:
  nav: "useq"
toc: true
toc_sticky: true

---

The scheduler runs a statement periodically.  This might be needed for stateful functions that require regular periodic updates

## `schedule <name> <period> <statement> `

Run the code in `statement` periodically, at the frequency of `period` times per bar

| Parameter | Description | Range |
| --- | --- | --- |
| name | An identifier | Any string |
| period | the number of times per bar to run the code | >0 |
| statement | A function | Any function |

To print "hi" 3 times per bar:
```
(schedule "test" 3 (println "hi"))
```

## `unschedule <name>`

Remove a function from the scheduler

| Parameter | Description | Range |
| --- | --- | --- |
| name | An identifier | Any string |

```
(unschedule "test")
```