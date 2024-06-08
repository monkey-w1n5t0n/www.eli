---
permalink: /useqinfo/useq-specification/
title: "uSEQ Specs"
sidebar:
  nav: "useq"
toc: true
toc_sticky: true
---


## Size 

8hp width

## Electrical 

### Voltages

| IO | Voltage |
|:---|:---|
| CV Inputs | Sensitive to -5V - 5V range (clips outside this range) |
| Pulse Inputs | Zero crossing |
| CV Outputs | -5V - 5V |
| Pulse Outputs | 0V or 5V |

### Current

75 mA

### Power Connector

10 pin Eurorack power


### Data Connector

USB-C (unpowered)


## Digital

### Microcontroller

- Raspberry Pi RP2040
- 8MB Flash Memory

### Serial Communication

Serial over USB, 115200 bps


### Analog / Digital Conversion

| IO | Resolution |
|:---|:---|
| CV Inputs | 11 bit ADCs |
| CV Outputs | 11 bit Pulse Width Modulation |



















