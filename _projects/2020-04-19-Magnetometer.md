---
layout: project
title: Datalogger
description: Datalogger_PYBoard
summary: Datalogger with a PYBoard
category: Project category
---

I have found myself in need of a decent datalogger while doing an experiment in the pysics laboratory (2^{nd} year of MSci). But let's start with order...

# The Thomson's jumping ring
This experiment is very famous and consist of a simple ring of metallic material jumping because of induced electric current via an electromagnet. It is a very simple concept, but it can become messy when doing a deep analysis of the forces in field. Hence, me and my lab partner wanted to be able to record the spikes of the magnetic field over time, as the magnetometer provided wasn't able to catch spikes, nor had datalogging capabilities.

## The arduino magnetometer
This is the first attempt I tried, failing miserably because the arduino board was showing the data on a simple 16x2 LCD screen.  An improovement would have been printing the data over serial port, and considering that an Arduino Uno hasa sampling frequency a bit less than 10KHz [1](https://arduino.stackexchange.com/questions/699/how-do-i-know-the-sampling-frequency), this would have been a good idea printing the data over serial port, then exporting these manually in a text file for analysis. Unpleasant, but still effective for a 10bit ADC resolution.
This is why I choose to go with a more advanced board, with more RAM, higher resolution and sampling frequency.






### References
[1]: https://arduino.stackexchange.com/questions/699/how-do-i-know-the-sampling-frequency
