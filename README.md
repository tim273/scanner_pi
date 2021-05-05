# Howto For Creating a Digital Scanner for the Raspberry Pi 4

## Introduction

This guide will show you how to create a digital, trunked Raspberry Pi scanner using an RTL SDR and SDR Trunk software.  Top of the line digital scanners cost upwards of $500-$600, this project will cost about half of that with a lot of flexability in how it can be implmented. 

My goal with this was to create as close to an all-in-one solution as possible, where there was only one thing to plugin and it is fairly portable.

## Step One Obtain Parts

Here are a list of parts you'll need, some of these things can be obtained at Microcenter for lower prices so if you have one near you, check there first.

1. A Raspberry Pi 4 with at least 4 Gb of RAM (8 Gb works too if you'd like): https://www.amazon.com/Raspberry-Model-2019-Quad-Bluetooth/dp/B07TC2BK1X
2. A high quality power supply, I like this one (you can get one without a switch for $1 less): https://www.amazon.com/Raspberry-Model-2019-Quad-Bluetooth/dp/B07TC2BK1X
3. A good cooling fan, I used this one as we'll be overclocking the Pi and we will need to keep it cool: https://www.amazon.com/gp/product/B07ZV1LLWK
4. Optional: The above cooler has a fan with LED's on it, if you don't want to deal with that you can get this fan as a replacement: https://www.amazon.com/gp/product/B00NEMGCIA 
5. A case, I used this one because of it's large size which gives plenty of room to move around a do stuff inside: https://www.amazon.com/gp/product/B08VSFZDDT
6. An RTL SDR dongle: https://www.amazon.com/NooElec-NESDR-Smart-Enclosure-R820T2-Based/dp/B01HA642SW
7. You'll also need this extra part for that case (choose the one with USB and audio jack): https://smarticase.com/products/usb-and-audio-jack-extender-for-smartipi-touch-pro
8. An antenna: https://www.amazon.com/gp/product/B07HLKKHCM
9. As for sound options, in wanting to keep this as a single unit with one power cord, I chose this USB speaker: https://www.amazon.com/gp/product/B086JXJ1LF. It's size matches the case, the sound quality is decent, and the cord can be hidden behind.  The problem with this speaker is that in combination with the RTL SDR dongle they will draw too much power from the USB bus and you'll get over voltage errors which will also an annoying clicking sound and won't work at all. I got around this buy creating USB power cable that plugs into the 5 volt and ground pins on the Pi and paired it with this splitter: https://www.amazon.com/gp/product/B00NIGO4NM.  If you don't want to go to that much trouble, you could just use a pair of cheap powered computer speakers that plug into the 3.5 mm port, it just means there will be two wall warts rather than one.  Another option is this USB speaker: https://www.amazon.com/gp/product/B075M7FHM1, it will not be very loud and the sound quality is so-so, but it get's the job done and doesn't draw too much power.
10. MicroSD Card: There are some options here too, but any class 10 or A1/A2 card should work like this one: 
https://www.amazon.com/SanDisk-128GB-Extreme-microSD-Adapter/dp/B06XWMQ81P
11. A USB SD/MicroSD card reader (unless there is one on your PC already, many laptops have them): https://www.amazon.com/Anker-Portable-Reader-RS-MMC-Micro/dp/B006T9B6R2

## Step Two Install Operating System On Micro SD Card

1. Install the Raspbery Pi imager on your computer: https://www.raspberrypi.org/software/
2. Insert the microSD card in your computer and open Raspberrry Pi Imager
3. Click Choose SD Card and the card that is inserted should be the only option, choose that one.
4. Click Choose OS and scroll down to Ubuntu and click on it, then choose the LTS version of the 64 bit server, for example: "Ubuntu Server 20.04.2 LTS (RPi 3/4/400)" the line under it should read "64-bit server Os with long-term support for arm64 architectures" ![Raspberry Pi Imager](/images/piImager.png) The latest version may be different, but the important part is to choose LTS 64 bit
5. Click the Write button and the OS will be loaded on the SD card.

## Step Three 

