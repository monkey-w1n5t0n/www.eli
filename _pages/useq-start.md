---
permalink: /useq-start/
title: "uSEQ Quick Start"
sidebar:
  nav: "useq"
toc: true
toc_sticky: true
---

## Setup

Hook up a computer to the module, and load this webpage in Chrome browser (or any other browser that has the WebSerial extension).   You can run code from the embedded editor below.


## Connect to uSEQ

In the editor window, click the   ```connect``` button. You will see a list of possible connections. uSEQ is likely to be the one at the top of the list.

uSEQ uses serial-over-USB. If you're connecting to a serial USB device for the first ever time on a Linux or Apple system, you may need to set up some permissions.  The editor will suggest a link with some information if there's an issue.  If you're stuck, please ask on the Discord channel.

{% include useqperform.html params="nosave" %}



## Firmware update check

It's good to make sure you have the latest firmware.  When you connect to the module, it will tell you if there's an update available.  If so, follow the instructions in the links.


## Generate a signal

Once connected, run the following code, by placing the cursor with the brackets and pressing ```Ctrl-Enter```

```
(d1 (sqr bar))
```

This will generate a square wave on output  ```d1```, that cycles once per bar; you should see the blue light flashing.  You could use this signal to, for example, trigger an envelope.

## Example: generating a rhythm

uSeq's ```gatesw``` function is a quick way of creating some rhythms.  For this example, you'll need an oscillator, running through a VCA which is controlled by an envelope generator.

First, generate a rhythm on output ```d3``` to trigger the envelope generator

```
(d3 (gatesw '(3 0 5 9 0 8 2 1) bar))
```

Paste the code in the editor, and as before, hit ```ctrl-enter``` to run it.

This pattern will repeat every bar, creating a pattern by creating a series of pulses. The numbers determine the pulse width; 9 is the widest, and 0 is a rest. You can play with the numbers to change the rhythm.

To change the speed of the rhythm, you can speed up the bar phasor like this

```
(d3 (gatesw '(3 0 5 9 0 8 2 1) (fast 2 bar)))
```

replacing ```2``` with any number more than or equal to 1 to vary the speed.

Now let's add some CV modulation. Plug ```a1``` into something interesting in your signal chain e.g. oscillator pitch, and run this code

```
(a1 (interp '(1 0.5 0 0.6 1) bar))
```

```interp``` is a bit like a looping envelope generator, it moved linearly across the numbers in the brackets, where 1 is highest (5V output) and 0 is lowest (-5V output).  You can change the numbers in the list to change the shape of the envelope,

Now you have two lines of code, one generating a rhythm, and one modulating your sound.  You can play around with and vary this code, to get a feel for how uSEQ works.  Just change the numbers, and re-run the statements.

## What next?

A good way to learn livecoding is from examples: take a look at the tutorials and quick modes, try varying and customising the code to get a feel for how it works. Take a look at the API reference to see what uSEQ can do

## Getting Help

Join our Discord [community](/useq-community/).






