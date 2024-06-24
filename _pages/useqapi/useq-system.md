---
permalink: /useqapi/useq-system/
title: "uSEQ API: System Utilities"
sidebar:
  nav: "useq"
toc: true
toc_sticky: true

---

## `useq-firmware-info`

Prints the module's current firmware version.

```
(useq-firmware-info)
```

## `useq-get-id`

Prints the current module's ID.

```
(useq-get-id)
```

## `useq-set-id <new id>`

Sets the module's identity, which could be a number or a name (as a string).

```
(useq-set-id 3)
```


## `useq-memory-save`

Saves the current state of the module (i.e. all definitions) to the persistent flash storage, which will be re-loaded on every subsequent boot.

```
(useq-memory-save)
```

## `useq-memory-restore` (alias: `useq-memory-load`)

Restores the state of the module (i.e. all definitions) to that which is saved in the persistent flash storage, effectively resetting the module to how it was when it last booted.

```
(useq-memory-restore)
```

## `useq-memory-erase` (alias: `useq-memory-clear`)

Erases all user definitions that were previously saved on the module's persistent flash storage. The next time the module boots, it will default to only the built-in definitions.

WARNING: this will permanently delete all saved user definitions.

```
(useq-memory-erase)
```


## `useq-stop-all`

Stops and clears all outputs, effectively emptying the update loop.

```
(useq-stop-all)
```

 `useq-reboot`

Reboots the module.

```
(useq-reboot)
```
## `timeit <statement>`

Returns the amount if time it took run ```statement```, in microseconds.

| Parameter | Description | Range |
| --- | --- | --- |
| statement | Any LISP statement | - |


```
(timeit (+ 1 1))
```

## `useq-memory-save`

Stores the module's current state (i.e. all of the user's definitions) to persistent flash memory, so that it can automatically be reloaded next time the module powers on.


```
(useq-memory-save)
```

## `useq-memory-restore`

Manually restores the (previously saved) module state (i.e. all of the user's definitions) from the persistent flash memory, effectively restoring the module's state to how it was when `useq-save` was last executed. 

Note that if a saved state is found on startup, it will be automatically loaded and therefore running this manually is not necessary, but may still prove useful to reset back to that "checkpoint" during a session. 

```
(useq-memory-restore)
```

## `useq-memory-erase`

Erases the (previously saved) environment (i.e. all of the user's definitions) from the persistent flash memory, effectively initialising the module to the default definitions next time it powers on.

```
(useq-memory-erase)
```

