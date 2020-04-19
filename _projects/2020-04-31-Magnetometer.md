---
layout: project
title: Datalogger
description: Datalogger_PYBoard
summary: Datalogger with a PYBoard
category: Project category
---

I have found myself in need of a decent datalogger while doing an experiment in the pysics laboratory (2<sup>nd</sup> year of MSci). Here you can find how to build one with the same characteristics as mine, with a sampling frequency of 5KHz over 2 channels for up to 4s per run. Note that the resolution is quite high (12bit).  
Here you can find the entire story about how I built it otherwise, if you are just interested only in building one, skip to parts three and four.

Let's start with order...


# [The Thomson's jumping ring](#thomson)
This experiment is very famous and consist of a simple ring of metallic material jumping because of induced electric current via an electromagnet. It is a very simple concept, but it can become messy when doing a deep analysis of the forces in field. Hence, me and my lab partner wanted to be able to record the spikes of the magnetic field over time, as the magnetometer provided wasn't able to catch spikes, nor had datalogging capabilities.


## [The arduino datalogger](#arduino)
This is the first attempt I tried, failing miserably because the arduino board was showing the data on a simple 16x2 LCD screen.  An improovement would have been printing the data over serial port, and considering that an Arduino Uno hasa sampling frequency a bit less than 10KHz [1](https://arduino.stackexchange.com/questions/699/how-do-i-know-the-sampling-frequency), this would have been a good idea printing the data over serial port, then exporting these manually in a text file for analysis. Unpleasant, but still effective for a 10bit ADC resolution.  
This is why I choose to go with a more advanced board, with more RAM, higher resolution and sampling frequency.


## [The full datalogger](#pyboard)
The board intended to use is now a PYBoard lite v1.0 (specifications [here](https://store.micropython.org/product/PYBLITEv1.0)), which has 128KB RAM, an ADC sampling frequency up to 20KHz and a microSD card slot. Overall, building this datalogger took me an entire weekend, but it was worth it.

### [Part one: reset the board](#reset)
SO, you just got your new board, maybe someone else used it before you becasue you got it from the lab and you don't know how to use it, so you open your broswer, look for some info and... voilà, the board connects via serial port over usb. Of course you already connected it, but no light powered on, the computer told you nothjing about it and you thought it's broken.  
Well, don't worry and do a full reset holding the switch as described in [2](https://docs.micropython.org/en/latest/pyboard/tutorial/reset.html#factory-reset-the-filesystem). This restores the board original files, it now lights up, a mass storage device (of about 90KB) shows up and opening the serial port (I personally use picocom for this) you find a micropython console.

### [Part two: the SDcard](#sdcard)
So, you familiarise a bit with the board, maybe try to write some files with it and discover that, if you record a file at 5KHZ, 12bit resolution over 4 seconds, in the end each file is about 97KB. And two of these need to be saved on each run, and maybe you need to do many runs before downloading the data...
This is when you realise you need an SD card, in particular a microSD card no bigger than 4GB. Of course you lost your last 4GB card, if you ever owned one, and the camera with the 1GB card is somewhere at your parents place. But you want to work on the project, it's about 21:00 and you'll never find one. Then, you realise that if you format the 8GB microSD in a smaller partition it may work. You try and create an __MBR prtition table with a primary 3GB partition__.  
Insert it in the board, reset it and the computer shows it got the device, a 3GB device. It worked.  
~~Actually, it took me about an hour just this part because of problems with my sd card reader, which comes from a desktop pc, readapted to conenct via  USB port to my laptop. Then repartitioning, it worked...~~

### [Part three: the acquisition code](#acquire)
You now need to write some code to acquire data, and given your experience with python it's not too difficult to write. In the end, you came up with __the code here [3](https://github.com/effeffe/PYBlite_datalogger/blob/master/script.py)__. This acquires data on channels Y11 and Y12, initialises a timer to acquire at 5KHz, creates one array per second per channel and then acquires accordingly to [4](https://docs.micropython.org/en/latest/library/pyb.ADC.html).  
The entire code had to be put together with some sleep functions for data sync and some gc.collect() functions to reduce the impact of the data on the RAM. In the end, I was not able to execute the code automatically, but had to copy/paste it into the serial console.  
Also, the write.py script to write the data on the SDcard is not executed automatically, and this needs to be fixed, as it requires a lot of copy/paste, as explained later.

### [Part four: the write script](#write)
Now, the data need to be written on the SDcard as 2 separate files. Laso, since the voltage is not really stable over time, it would be nice to acquire the voltage of the board. This is done by an internal function  and saved to a third text file.  
The script then saves first the voltage, as it needsa to be acquired as soon as possible after the data acquisition. Then the first set of data is written to a .csv file, one entry per line. At the end, the second set of data is written and the script ends.  
In  my opinion, it is safer to execute at the very end line `machine.soft_reset()` to avoid data loss because of improper file ending, as this allowes to ~~sync the data~~ (<- not too sure about this) and empty the memory without needing to do a power cycle.
The write script can be found here [5](https://github.com/effeffe/PYBlite_datalogger/blob/master/write.py).

### [IC used and final notes](#final)
In my case, the datalogger was used using a couple of TI DRV5055A1 (hall sensors) to measure the radial and vertical components of the magnetic field of a solenoid. In this case, the data have been analysed using the script here [6](https://github.com/effeffe/PYBlite_datalogger/blob/master/Analysis.py), which produces a png with the plot and the peak values in a txt file. This may need tweaks, as was origianally written for a negative magnetic field (for a positive changing the np.min to np.max should be enough).  
In general, all the notes regarding this project can be found here [7](https://github.com/effeffe/PYBlite_datalogger), where it is possible to discuss the code and improve it.  

Anyways, for some code written in a weekend, the outcome was nice and it was fully working.


### [References](#references)
[1] : [https://arduino.stackexchange.com/questions/699/how-do-i-know-the-sampling-frequency](https://arduino.stackexchange.com/questions/699/how-do-i-know-the-sampling-frequency)  
[2] : [https://docs.micropython.org/en/latest/pyboard/tutorial/reset.html#factory-reset-the-filesystem](https://docs.micropython.org/en/latest/pyboard/tutorial/reset.html#factory-reset-the-filesystem)  
[3] : [https://github.com/effeffe/PYBlite_datalogger/blob/master/script.py](https://github.com/effeffe/PYBlite_datalogger/blob/master/script.py)  
[4] : [https://docs.micropython.org/en/latest/library/pyb.ADC.html](https://docs.micropython.org/en/latest/library/pyb.ADC.html)  
[5] : [https://github.com/effeffe/PYBlite_datalogger/blob/master/write.py](https://github.com/effeffe/PYBlite_datalogger/blob/master/write.py)  
[6] : [https://github.com/effeffe/PYBlite_datalogger/blob/master/Analysis.py](https://github.com/effeffe/PYBlite_datalogger/blob/master/Analysis.py)  
[7] : [https://github.com/effeffe/PYBlite_datalogger](https://github.com/effeffe/PYBlite_datalogger)  
