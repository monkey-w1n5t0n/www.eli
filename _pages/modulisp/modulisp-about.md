---
permalink: /modulisp-about/
title: "ModuLisp"
sidebar:
  nav: "useq"
toc: true
toc_sticky: true
---

## Defining & naming things

While a lot can be achieved by simply writing and using expressions directly in the code, sooner or later the ability to assign names to values and signals becomes an indispensable ally, helping us structure and organise our thoughts and code.


Naming things allows us to easily reference them in many different parts of the code, so that any changes made to them get automatically propagated to every other part of the code that uses them. It also encourages us to come up with useful and informative names that make it easier to remember, understand, and reason about our thought process and the resulting code.


In ModuLisp, there are four main ways of defining and naming things: `define`/`def`, `defun`/`defn`, `set`, and `let`. They all behave slightly differently, so understanding these differences is important in deciding which one is more appropriate to be used in any given situation.

### `define <name> <expression>` (alias: `def`, variation: `defs`)

| Parameter  | Description                               | Type   |
|------------|-------------------------------------------|--------|
| name       | The name to assign to the expression      | Symbol |
| expression | The expression to associate with the name | Any    |

Creates a new **signal** definition, essentially assigning a name to a (potentialy time-varying) expression. These definitions are accessible from anywhere in the code, unless they are shadowed by a local binding with higher precedence (e.g. when inside a `let` expression body).

``` clojure
(define my-number-three (+ 1 2))

(+ my-number-three 1) ;; => 4
```

Variables created with `define` will always have the value that their expression evaluates to at the time of _usage_, not just at the time of _definition_.

``` clojure
(define a 1)
(define b 2)
(define my-number-three (+ a b))

;; Check the value first
my-number-three ;; => 3 - as expected

;; Let's Redefine one of the variables used in its definition
(define a 101)

;; Check the value again
my-number-three ;; => 103 - it's been updated!
```


TIP: if you need to find out the definition of a variable that was created by `define`, you can use `get-expr`.

A variation by the name `defs` works the same way, but can be used to define multiple pairs of names and expressions at the same time:

``` clojure
(define my-ramp          (slow 2 beat))
(define my-inverted-ramp (- 1 my-ramp))

;; the two expressions above are equivalent to this one:

(defs my-ramp          (slow 2 beat)
      my-inverted-ramp (- 1 my-ramp))
```

### `set <name> <expression>` 

| Parameter  | Description                                          | Type   |
|------------|------------------------------------------------------|--------|
| name       | The name to assign to the expression's current value | Symbol |
| expression | The expression to evaluate                           | Any    |


Creates a new **static** definition, essentially assigning a name to the value that an expression evaluates to _at the moment_ that the definition is executed. These definitions are accessible from anywhere in the code, unless they are shadowed by a local binding with higher precedence (e.g. when inside a `let` expression body).

``` clojure
(set my-current-point-in-bar bar) ;; => stores the current 

;; Let's compare the value that `bar` had at the moment
;; of definition with the value that it has now:

(= bar my-current-point-in-bar) ;; => most likely false, as time has moved on
```

TIP: A handy mnemonic trick to remember the difference between `set` and `define` is _"set in stone"_: `set` will evaluate its expression only once and then _set its value in stone_, so that the name always refers to the same value, even if later on the expression evaluates to something different (either as a result of time-varying behaviour or simply by being redefined).

``` clojure
(set a 1)
(set b 2)
(set my-number-three (+ a b))

;; Check the value
my-number-three ;; => 3 - as expected

;; Change one of the variables used in its definition
(set a 99999)

;; Check the value again 
my-number-three ;; => 3 - nothing has changed!
```

### `defun <name> <params> & <body>` (alias: `defn`)

| Parameter | Description                              | Type                   |
|-----------|------------------------------------------|------------------------|
| name      | The name of the function                 | Symbol                 |
| params    | A list of the function's parameter names | List/Vector of Symbols |
| body      | The body of code to run when called      | Any                    |

Creates and names a function. The body can be any number of forms, and the function's return value will always be the return value of the last expression.

``` clojure
(defun my-no-args-function []
  (println "Hello from inside the function!"))
  
(my-no-args-function) ;; => prints the message

;; if the function name is long, you can put each of the other arguments
;; on their own lines

(defn my-addition-function 
  [a b] 
  (+ a b))

(my-addition-function 3 5) ;; => 8

```


### `let <bindings> & <body>` 

| Parameter | Description                                                    | Type                                            |
|-----------|----------------------------------------------------------------|-------------------------------------------------|
| bindings  | A list of binding pairs                                        | List/Vector of pairs of symbols and expressions |
| body      | Any number of expressions to evaluate within the local context | Any                                             |

Creates a number of locally-named variables that are only accessible within the body of the `let` expression. 

``` clojure
(let [a 1
      b 2]
  (+ a b)) ;; => 3
```

Local variables can also be defined in terms of other local variables defined earlier in the `let`:

``` clojure
(let [a "Hello"
      b (str a ", World!")]
  (println b)) ;; => "Hello, World!"
```

If any of the names clash with variables that have been named outside the `let` expression, the local names will "shadow" (i.e. overwrite) the external ones:

``` clojure
(define a 10000)
(define b 99999)
(define c 10)

(let [a 1
      b 2]
  (+ a b c)) ;; => 13
```
  
## Working with lists
NOTE: since lists and vectors are so similar, the terms are often used interchangeably.
  
### `list <args>`

| Parameter | Description | Range |
| --- | --- | --- |
| args | Any number of values | any |

Creates a list containing the given arguments.

```clojure
(list 1 2 3) ; => (1 2 3)

(list 'a 'b 'c) ; => (a b c)
```

NOTE: `list` _will_ evaluate its arguments, unlike `quote`, which is often the desired behaviour:

```clojure
(list 1 2 (+ 1 2)) ;; => (1 2 3)
(quote (1 2 3))           ;; => (1 2 3)
'(1 2 3) ;; => (1 2 3)

;; but, here the addition does not get evaluated first:
'(1 2 (+ 1 2) ;; => (1 2 (+ 1 2))
```

### `vec <args>`

| Parameter | Description | Range |
| --- | --- | --- |
| args | Any number of values | any |

Creates a vector containing the given arguments.

``` clojure
(vec 1 2 3) ; => [1 2 3]

(vec 'a 'b 'c) ; => [a b c]
```

### `index <list> <index>` (alias: `nth`)

| Parameter | Description | Range |
| --- | --- | --- |
| list | A list of values | any |
| index | The index of the element to retrieve | integer |

Returns the element at the given index in the list.

```clojure
(index [1 2 3] 1) ; => 2
(index '(a b c) 0) ; => a
```


### `first <list>` (alias: `head`)

| Parameter | Description      | Range |
|-----------|------------------|-------|
| list      | A list of values | any   |

Returns the first element of the list.

```clojure
(first [1 2 3 4]) ;; => 1

(first '(a b c d)) ;; => a
```
### `rest <list>` (alias: `tail`)

| Parameter | Description | Range |
| --- | --- | --- |
| list | A list of values | any |

Returns all but the first element of the list.


``` clojure
(tail [1 2 3 4]) ; => [2 3 4]
```
``` clojure
(tail '(a b c d)) ; => (b c d)
```

### `zeros <length>`

| Parameter | Description                     | Range            |
|-----------|---------------------------------|------------------|
| length    | The number of zeros to generate | any integer >= 0 |

Creates a list of zeros of the specified length.

``` clojure
(zeros 5) ; => [0 0 0 0 0]
(zeros 0) ; => []
```

### `insert <list> <position> <value>`

| Parameter | Description                        | Type             |
|-----------|------------------------------------|------------------|
| list      | The list to insert in              | List/Vector      |
| position  | Where in the list to put the value | any integer >= 0 |
| value     | The value to insert                | Any              |

Inserts a value in a specific position in the list.

```clojure
(insert '(a b c) 1 "hi") ;; => (a "hi" b c)
(insert [0 1 2 3] 3 0.5) ;; => [0 1 2 0.5 3]
```

### `remove <list> <index>`

| Parameter | Description                     | Type                                |
|-----------|---------------------------------|-------------------------------------|
| list      | The list to remove from         | List/Vector                         |
| index     | The index of the item to remove | any integer >= 0 and <= list length |

Removes from the list the value at the specified index (if it exists).

```clojure
(remove '(a b c) 0) ;; => (b c)
(remove [0 1 2 3 4] 2) ;; => [0 1 3 4]
```

### `len <list> `
Returns the length of a list or vector.

### `push <list> <value>`

| Parameter | Description         | Type        |
|-----------|---------------------|-------------|
| list      | The list to push to | List/Vector |
| value     | The value to push   | any         |

Pushes a value to the top of a list.

```clojure
(push '(b c d) 'a) ;; => (a b c d)
(push [1 2 3] 5) ;; => [5 1 2 3]
```

### `pop <list> `
Removes the first/top element of the list.

## Functional Programming
### `lambda <params> & <body>` (alias: `fn`)

| Parameter | Description                              | Type                   |
|-----------|------------------------------------------|------------------------|
| params    | A list of the function's parameter names | List/Vector of Symbols |
| body      | The body of code to run when called      | Any                    |

Creates an anonymous function that's ready to use. It works more-or-less exactly the same as `defun`/`defn`, except it doesn't expect a name.

TIP: `lambda`/`fn` is often used with higher-order functions, such as `map`, to create a local/temporary function:

```clojure
(map (lambda [x] (* x x)) 
  [1 2 3 4]) ; => [1 4 9 16]
```

### `map <function> <list>`

| Parameter | Description | Range |
| --- | --- | --- |
| function | A function to apply to each element | function |
| list | A list of values | any |

Applies a function to each element of a list and returns a list of the results.



```clojure
(map (fn [x] (* x x)) 
  [1 2 3 4]) ; => [1 4 9 16]
```

### `filter <test function> <list>`

| Parameter     | Description                         | Range                              |
|---------------|-------------------------------------|------------------------------------|
| test function | A function to apply to each element | 1-arg Function that returns a Bool |
| list          | A list of values                    | Any                                |

Filters a list using a single-argument function that should return `true` or `false`. Any value that's in the input list and for which the test function returns false will be excluded from the resulting list.

```clojure
(filter (fn [x] (> x 2)) 
  [1 2 3 4]) ; => [3 4]
```

### `reduce <function> <list>`
TODO

## Evaluation control
### `do & <expressions>`

| Parameter | Description | Range |
| --- | --- | --- |
| args | Any number of expressions | any |

Evaluates the given arguments in order and returns the value of the last argument.


``` clojure
(do (print "Hello") 
    (print "World!") 
    (+ 1 2)) ;; => 3
```

TIP: This is useful for applying changes to a many outputs at the same time:
```clojure
(do
  (d1 (sqr bar))
  (d2 (sqr (slow 2 bar))))
 
;; silence all analog outs
(do
  (a1 0)
  (a2 0)
  (a3 0))
```
### `if <test> <then> <else>`

| Parameter | Description                                          | Range |
|-----------|------------------------------------------------------|-------|
| test      | An expression that evaluates to either true or false | any   |
| then      | An expression to evaluate if the test returns true   | any   |
| else      | An expression to evaluate if the test returns false  | any   |

Conditionally executes one of two branches depending on the outcome of a boolean test.

```clojure
(if (> 3 2)
  (println "Three is greater than two")
  (println "Two is greater than three!?!?"))
```
### `for <name> <list> & <body>`

| Parameter | Description                        | Type        |
|-----------|------------------------------------|-------------|
| name      | The name to assign to each value   | Symbol      |
| list      | The list of values to iterate over | List/Vector |
| body      | Any number of expressions          | any         |

Iterates over a list of values, assigns each value to a name, and then evaluates a number of expressions in which that name can be used to refer to each of the iterated values.

```clojure
(for my-number [1 2 3]
  (println "My number is: ")
  (println my-number)
  my-number) ;; => prints each number in order, returns 3
```

### `while <condition> & <body>`

| Parameter | Description                                | Type |
|-----------|--------------------------------------------|------|
| condition | Any expression that evaluates to a boolean | Bool |
| body      | Any number of expressions to evaluate      | any  |

Executes a number of expressions repeatedly until the condition evaluates to false.

```clojure
(set i 0)

(while (< i 10)
  (println "i is now = ")
  (println i)
  (set i (+ i 1)))
```

## Arithmetic & Maths
### `+ [<numbers>]` (alias: `sum`)

Returns the sum of all its arguments, which are expected to evaluate to numbers.

``` clojure
(+ 1 2 3) ;; => 6
```

### `- <value1> <value2>`

Subtracts the second number from the first.

| Parameter | Description                | Range  |
|-----------|----------------------------|--------|
| value1    | The value to subtract from | number |
| value2    | The value to subtract      | number |

### `* [<numbers>]`

Returns the result of multiplying all of its arguments, which are expected to evaluate to numbers.

``` clojure
(* 2 2 2) ;; => 8
```

### `/ <value1> <value2>`
  
Divides the first value by the second value.

| Parameter | Description | Range |
| --- | --- | --- |
| value1 | The value to be divided | number |
| value2 | The value to divide by | (non-zero) number |

### `% <value1> <value2>`

| Parameter | Description | Range |
| --- | --- | --- |
| value1 | The value to be divided | number |
| value2 | The value to divide by | number |

Calculates the remainder of the division of the first value by the second value.
### `floor <value>`

| Parameter | Description             | Range  |
|-----------|-------------------------|--------|
| value     | The value to round down | number |

Rounds a number down to the nearest integer.
### `ceil <value>`

| Parameter | Description           | Range  |
|-----------|-----------------------|--------|
| value     | The value to round up | number |

Rounds a number up to the nearest integer.
### `= <value1> <value2>`

| Parameter | Description                 | Range |
|-----------|-----------------------------|-------|
| value1    | The first value to compare  | any   |
| value2    | The second value to compare | any   |

Checks if two values are equal.
### `!= <value1> <value2>`

| Parameter | Description                 | Range |
|-----------|-----------------------------|-------|
| value1    | The first value to compare  | any   |
| value2    | The second value to compare | any   |

Returns true if the two values are not equal, false otherwise.

```clojure
(!= 1 2) ; => true

(!= 'a 'a) ; => false
```

### `> <value1> <value2>`

| Parameter | Description                 | Range |
|-----------|-----------------------------|-------|
| value1    | The first value to compare  | any   |
| value2    | The second value to compare | any   |

Checks if the first value is greater than the second value.

### `< <value1> <value2>`

| Parameter | Description | Range |
| --- | --- | --- |
| value1 | The first value to compare | any |
| value2 | The second value to compare | any |

Checks if the first value is less than the second value.

### `>= <value1> <value2>`

| Parameter | Description | Range |
| --- | --- | --- |
| value1 | The first value to compare | any |
| value2 | The second value to compare | any |

Checks if the first value is greater than or equal to the second value.

### `<= <value1> <value2>`

| Parameter | Description | Range |
| --- | --- | --- |
| value1 | The first value to compare | any |
| value2 | The second value to compare | any |

Checks if the first value is less than or equal to the second value.

### `usin <phasor>`

| Parameter | Description             | Range |
|-----------|-------------------------|-------|
| phasor    | A value between 0 and 1 | 0-1   |

Generates a unipolar sine wave from the given phasor.

```clojure
(usin 0.5) ; => 1.0

(usin 0.25) ; => 0.8535533905932737
```


### `floor <number>`

| Parameter | Description             | Range |
|-----------|-------------------------|-------|
| number    | A floating-point number | any   |

Rounds a number down to the nearest integer.


```clojure
(floor 3.7) ; => 3

(floor -1.2) ; => -2
```


### `ceil <number>`

| Parameter | Description             | Range |
|-----------|-------------------------|-------|
| number    | A floating-point number | Any   |


Rounds a number up to the nearest integer.


```clojure
(ceil 3.7) ; => 4

(ceil -1.2) ; => -1
```

### `scale <val> <in min> <in max> <out min> <out max>`

| Parameter | Description              | Type   |
|-----------|--------------------------|--------|
| val       | The value to scale       | Number |
| in-min    | The minimum input range  | Number |
| in-max    | The maximum input range  | Number |
| out-min   | The minimum output range | Number |
| out-max   | The maximum output range | Number |

Scales a value from one range to another.

```clojure
(scale 0.5 0 1 0 100) ;; => 50.0
(scale 25 0 100 0 1) ;; => 0.25
```

### `bi->uni <value>` (alias: `b->u`)

| Parameter | Description                        | Type   |
|-----------|------------------------------------|--------|
| value     | The bipolar value to convert       | Number |

Converts a bipolar value (-1 to 1) to a unipolar value (0 to 1).

```clojure
(bi->uni -1) ;; => 0.0
(bi->uni 0) ;; => 0.5
(bi->uni 1) ;; => 1.0
```

### `uni->bi <value>` (alias: `u->b`)

| Parameter | Description                        | Type   |
|-----------|------------------------------------|--------|
| value     | The unipolar value to convert      | Number |

Converts a unipolar value (0 to 1) to a bipolar value (-1 to 1).

```clojure
(uni->bi 0) ;; => -1.0
(uni->bi 0.5) ;; => 0.0
(uni->bi 1) ;; => 1.0
```

### `pow <exponent> <value>`

| Parameter | Description                        | Type   |
|-----------|------------------------------------|--------|
| exponent  | The exponent value                 | Number |
| value     | The base value                     | Number |

Returns the result of raising the base to the power of the exponent.

```clojure
(pow 3 2) ;; => 8.0
(pow 2 5) ;; => 25.0
```

### `sine <value>` (alias: `sin`, variation: `usine`/`usin`)
TODO
### `cosine <value>` (alias: `cos`, variation: `ucosine`/`ucos`)
TODO
### `tan <value>`
TODO
### `abs <value>`
TODO
### `min <value>`
TODO
### `max <value>`
TODO
### `sqrt <value>`
TODO
## Utility functions
### range <low> <high>

| Parameter | Description | Type |
| --- | --- | --- |
| low | The starting value of the range | integer or float |
| high | The ending value of the range | integer or float |

Generates a list of numbers from the low value up to (but not including) the high value.

```clojure
(range 0 5) ; => [0 1 2 3 4]

(range 1 4) ; => [1 2 3]
```

## IO
### `dw <pinNumber> <value>`

| Parameter | Description                      | Range   |
|-----------|----------------------------------|---------|
| pinNumber | The pin number to write to       | integer |
| value     | The value to write (HIGH or LOW) | integer |

Writes a HIGH or LOW value to a digital pin.

(dw 13 1) ; => sets pin 13 to HIGH

(dw 13 0) ; => sets pin 13 to LOW

### `dr <pinNumber>`

| Parameter | Description | Range |
| --- | --- | --- |
| pinNumber | The pin number to read from | integer |


Reads the value from a specified digital pin.

```clojure
(dr 13) ; => returns the value from pin 13 (HIGH or LOW)
```

### `perf`

Reports the most recent performance metrics.

``` clojure
(useq_perf) ; => prints performance metrics
```

## Metaprogramming
### `eval <expression>`

| Parameter  | Description                | Type |
|------------|----------------------------|------|
| expression | The expression to evaluate | Any  |

Evaluates an expression at runtime, allowing for dynamic execution of code.

```clojure
(eval (+ 1 2 3)) ;; => 6
```

### `type <expression>`

| Parameter  | Description                       | Type |
|------------|-----------------------------------|------|
| expression | The expression to get the type of | Any  |

Returns the type of the given expression.

```clojure
(type 42) ;; => Number
```

### `get-expr <name>`

| Parameter | Description                         | Type   |
|-----------|-------------------------------------|--------|
| name      | The name whose definition to return | Symbol |

Returns the expression that was used to define a variable.

NOTE: this will only return something if the variable was defined using `define`.

```clojure
(define my-foo (+ 1 2 3))

(get-expr my-foo) ;; => (+ 1 2 3)
```
