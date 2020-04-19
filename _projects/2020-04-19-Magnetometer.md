---
layout: project
title: Datalogger
description: Datalogger_PYBoard
summary: Datalogger with a PYBoard
category: Project category
---

I have found myself in need of a decent datalogger while doing an experiment in the pysics laboratory (2<sup>nd</sup> year of MSci). Here you can find how to build one with the same characteristics as mine, with a sampling frequency of 5KHz over 2 channels for up to 4s per run. Note that the resolution is quite high (12bit).  
Here you can find the entire story about how I built it otherwise, if you are just interested only in building one, skip [here]().  

Let's start with order...


# [The Thomson's jumping ring](#thomson)
This experiment is very famous and consist of a simple ring of metallic material jumping because of induced electric current via an electromagnet. It is a very simple concept, but it can become messy when doing a deep analysis of the forces in field. Hence, me and my lab partner wanted to be able to record the spikes of the magnetic field over time, as the magnetometer provided wasn't able to catch spikes, nor had datalogging capabilities.


## The arduino datalogger
This is the first attempt I tried, failing miserably because the arduino board was showing the data on a simple 16x2 LCD screen.  An improovement would have been printing the data over serial port, and considering that an Arduino Uno hasa sampling frequency a bit less than 10KHz [1](https://arduino.stackexchange.com/questions/699/how-do-i-know-the-sampling-frequency), this would have been a good idea printing the data over serial port, then exporting these manually in a text file for analysis. Unpleasant, but still effective for a 10bit ADC resolution.  
This is why I choose to go with a more advanced board, with more RAM, higher resolution and sampling frequency.


## The full datalogger
The board intended to use is now a PYBoard lite v1.0 (specifications [here](https://store.micropython.org/product/PYBLITEv1.0)), which has 128KB RAM, an ADC sampling frequency up to 20KHz and a microSD card slot. Overall, building this datalogger took me an entire weekend, but it was worth it.

### Part one: reset the board
SO, you just got your new board, maybe someone else used it before you becasue you got it from the lab and you don't know how to use it, so you open your broswer, look for some info and... voilà, the board connects via serial port over usb. Of course you already connected it, but no light powered on, the computer told you nothjing about it and you thought it's broken.  
Well, don't worry and do a full reset holding the switch as described [here](https://docs.micropython.org/en/latest/pyboard/tutorial/reset.html#factory-reset-the-filesystem). This restores the board original files, it now lights up, a mass storage device (of about 90KB) shows up and opening the serial port (I personally use picocom for this) you find a micropython console.

### Part two: the SDcard
So, you familiarise a bit with the board, maybe try to write some files with it and discover that, if you record a file at 5KHZ, 12bit resolution over 4 seconds, in the end each file is about 97KB. And two of these need to be saved on each run, and maybe you need to do many runs before downloading the data...
This is when you realise you need an SD card, in particular a microSD card no bigger than 4GB. Of course you lost your last 4GB card, if you ever owned one, and the camera with the 1GB card is somewhere at your parents place. But you want to work on the project, it's about 21:00 and you'll never find one. Then, you realise that if you format the 8GB microSD in a smaller partition it may work. You try and create an __MBR prtition table with a primary 3GB partition__.  
Insert it in the board, reset it and the computer shows it got the device, a 3GB device. It worked.  
~~Actually, it took me about an hour just this part because of problems with my sd card reader, which comes from a desktop pc, readapted to conenct via  USB port to my laptop. Then repartitioning, it worked...~~

### Part three:


### References
[1] : [https://arduino.stackexchange.com/questions/699/how-do-i-know-the-sampling-frequency](https://arduino.stackexchange.com/questions/699/how-do-i-know-the-sampling-frequency)
