---
permalink: /useqapi/useq-scheduling/
title: "uSEQ API: Scheduler Functions"
sidebar:
  nav: "useq"
toc: true
toc_sticky: true

---

The scheduler runs a statement periodically.  This might be needed for stateful functions that require regular periodic updates

## `schedule <name> <statement> <period>`

Run the code in `statement` periodically, at the frequency of `period` times per bar

| Parameter | Description | Range |
| --- | --- | --- |
| name | An identifier | Any string |
| statement | A function | Any function |
| period | the number of times per bar to run the code | >0 |

To print "hi" 3 times per bar:
```
(schedule "test" (print "hi") 3)
```

## `unschedule <name>`

Remove a function from the scheduler

| Parameter | Description | Range |
| --- | --- | --- |
| name | An identifier | Any string |

```
(unschedule "test")
```