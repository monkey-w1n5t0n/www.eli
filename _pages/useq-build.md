---
permalink: /useq-build/
title: "uSEQ Build Guide"
sidebar:
  nav: "useq"

---


![uSEQ Kit](/assets/images/useq_kit.png){:class="img-responsive"}

This is probably classes as an easy build, although there are a few things to be careful with.  Most of the components are already pre-assembled on the PCB, so you just need to solder knobs, switches and headers and mount the panel.

## In the kit

- PCB, with most of the components pre-soldered
- aluminium face plate
- power cable
- 2M USB-A to USB-C cable
- 2 pin expander cable (for future firmware releases)
- 5 pink LEDs
- 5 ice blue LEDs
- 2 2-pin headers
- 3-pin header
- 2 surface mount momentary buttons
- 10 sockets
- 2 100k potentiometers
- momentary switch
- toggle switch
- 2 potentiometer caps
- momentary switch cap
- toggle switch cover

## About the PCB

The top part of the PCB is a small and very basic computer - a Raspberry Pi RP2040 microcontroller, some flash memory, a clock crystal, a USB connection and power conditioning.  The lower two-thirds of the board is mostly op-amps and transistors that deal with analogue inputs and outputs for the computer. When you send code to uSEQ, you are telling the computer what to do with these inputs and outputs.


## You will need:

1. Soldering equipment
2. Tweezers
3. Cutters

## Building uSEQ

### Step 1: Headers

Solder on the 2 x two pin expansion port headers and the 3 pin JTAG header

![build step 1](/assets/images/useq/build1headers.jpg){:class="img-responsive"}

### Step 2: Reset and boot select buttons

On the front size of the board, add on the two buttons.  These are surface mount buttons. The pads are pre-tinned. A good technique is to melt the solder on one side and slide the button in, then press down on the other side with the soldering iron to melt the solder on the second pad.  You don't need any sort of specialist iron from this, but keeping the temperature low (350C) might be useful, as high temperatures can melt the plastic in these buttons.

![build step 2](/assets/images/useq/build2ButtonsClose.jpg){:class="img-responsive"}
![build step 2](/assets/images/useq/build2Buttons.jpg){:class="img-responsive"}

If you're doing SMT for the first time, this is probably a good beginners first step, with a large component.  Here's a video that might help:

<iframe width="560" height="315" src="https://www.youtube.com/embed/Y-M-cEkLw8Q?si=l2de8HM_JkLd1v9a" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>





### Step 3: Place (but don't solder) potentiometers

Push the two pots into the board

![build step 3](/assets/images/useq/build4pots.jpg){:class="img-responsive"}

### Step 4: Place (but don't solder) the momentary button

Important: this is a polarised component.  You can see a small letter on one side of the top of the button (the letters themselves vary), this needs to go to the left.  

![build step 4](/assets/images/useq/buildMomentaryClose.jpg){:class="img-responsive"}
![build step 4](/assets/images/useq/buildMomentary.jpg){:class="img-responsive"}

### Step 5: Mount LEDs (don't solder yet)

Five pink LEDs go on the left hand side in D7-11. Five blue LEDs go on the right hand side in D5,6,12,13,14. The holes with square pads are ground (short leg). If you get LEDs mixed up, it's possible to tell the colours apart if you look closely in the light.

![build step 5](/assets/images/useq/ledsockets.jpg){:class="img-responsive"}
![build step 5](/assets/images/useq/buildledholes.png){:class="img-responsive"}
![build step 5](/assets/images/useq/build5leds.jpg){:class="img-responsive"}


### Step 6: Place sockets and switches (still don't solder)

Probably best to put the sockets in place first.  Double check that all pins are in the holes when you're done.

![build step 6](/assets/images/useq/build6sockets.jpg){:class="img-responsive"}

### Step 7: Put on the face plate

Shake it around a bit until things fall into place

![build step 7](/assets/images/useq/buildPanel.jpg){:class="img-responsive"}


### Step 8: Loosely put the nuts onto the switches, pots and sockets

![build step 8](/assets/images/useq/buildPanelNuts.jpg){:class="img-responsive"}
![build step 8](/assets/images/useq/build_nuts_2.jpg){:class="img-responsive"}

### Step 9: Now you can solder everying into place

This is a good time to double check that everything is in place properly before you solder.

First solder the momentary button.  Important: make sure that it's pressed fully flat against the panel when you solder, otherwise it might sit at an angle and rub against the faceplate.

Next, solder the sockets, switches and pots. Then, make sure that all of the LEDs are in the right place in the front panel and solder the LEDs.

### Step 10: Clip the LED legs

### Step 11: Tighten the nuts on the front panel

### Step 12: Place the potentiometer covers, toggle switch cover and momentary switch cap

![build step 12](/assets/images/useq/useq%20front%202%20sq.png){:class="img-responsive"}



## Testing 

There's no calibration required.  To test the module, try running the code in the [getting started](/useq-start/) guide.
