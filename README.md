# Howto For Creating a Digital Scanner for the Raspberry Pi 4

## Introduction

This guide will show you how to create a digital, trunked Raspberry Pi scanner using an RTL SDR and SDR Trunk software.  Top of the line digital scanners cost upwards of $500-$600, this project will cost about half of that with a lot of flexability in how it can be implmented. 

My goal with this was to create as close to an all-in-one solution as possible, where there was only one thing to plugin and it is fairly portable.

## Step One: Parts List Shopping Cart

Here are a list of parts you'll need, some of these things can be obtained at Microcenter for lower prices so if you have one near you, check there first.

1. A Raspberry Pi 4 with at least 4 Gb of RAM (8 Gb works too if you'd like): https://www.amazon.com/Raspberry-Model-2019-Quad-Bluetooth/dp/B07TC2BK1X
2. Raspberry Pi 7 inch touchscreen: https://www.amazon.com/Raspberry-Pi-7-Touchscreen-Display/dp/B0153R2A9I
3. A high quality power supply, I like this one (you can get one without a switch for $1 less): https://www.amazon.com/Raspberry-Model-2019-Quad-Bluetooth/dp/B07TC2BK1X
4. A good cooling fan, I used this one as we'll be overclocking the Pi and we will need to keep it cool: https://www.amazon.com/gp/product/B07ZV1LLWK 
5. A case, I used this one because of it's large size which gives plenty of room to move around and route stuff inside: https://www.amazon.com/gp/product/B08VSFZDDT
6. You'll also need this extra part for that case (choose the one with USB and audio jack): https://smarticase.com/products/usb-and-audio-jack-extender-for-smartipi-touch-pro
7. An RTL SDR dongle: https://www.amazon.com/NooElec-NESDR-Smart-Enclosure-R820T2-Based/dp/B01HA642SW
8. An antenna: https://www.amazon.com/gp/product/B07HLKKHCM
9. As for sound options, in wanting to keep this as a single unit with one power cord, I chose this USB speaker: https://www.amazon.com/gp/product/B086JXJ1LF. It's size matches the case, the sound quality is decent, and the cord can be hidden behind.  The problem with this speaker is that in combination with the RTL SDR dongle they will draw too much power from the USB bus and you'll get over voltage errors which will also an annoying clicking sound and won't work at all. I got around this buy creating USB power cable that plugs into the 5 volt and ground pins on the Pi and paired it with this splitter: https://www.amazon.com/gp/product/B00NIGO4NM.  If you don't want to go to that much trouble, you could just use a pair of cheap powered computer speakers that plug into the 3.5 mm port, it just means there will be two wall warts rather than one.  Another option is this USB speaker: https://www.amazon.com/gp/product/B075M7FHM1, it will not be very loud and the sound quality is so-so, but it get's the job done and doesn't draw too much power.
10. MicroSD Card: There are some options here too, but any class 10 or A1/A2 card should work like this one: 
https://www.amazon.com/SanDisk-128GB-Extreme-microSD-Adapter/dp/B06XWMQ81P
11. A USB SD/MicroSD card reader (unless there is one on your PC already, many laptops have them): https://www.amazon.com/Anker-Portable-Reader-RS-MMC-Micro/dp/B006T9B6R2
12. USB Extension Cables: https://www.amazon.com/gp/product/B01GA1GKYW
13. Optional: Right angle USB extension cords, these give a cleaner look if you're using the USB speaker: https://www.amazon.com/gp/product/B07CF7243G
14. Optional: The above cooler (from #4) has a fan with LED's on it, if you don't want with that you can get this fan as a replacement: https://www.amazon.com/gp/product/B00NEMGCIA, and you'll need these too: https://www.amazon.com/EDGELEC-Breadboard-Optional-Assorted-Multicolored/dp/B07GD2BWPY

## Step Two: Install Operating System On Micro SD Card

1. Install the Raspbery Pi imager on your computer: https://www.raspberrypi.org/software/
2. Insert the microSD card in your computer and open Raspberrry Pi Imager
3. Click Choose SD Card and the card that is inserted should be the only option, choose that one.
4. Click Choose OS and scroll down to Ubuntu and click on it, then choose the LTS version of the 64 bit server, for example: "Ubuntu Server 20.04.2 LTS (RPi 3/4/400)" the line under it should read "64-bit server Os with long-term support for arm64 architectures" ![Raspberry Pi Imager](/images/piImager.png) The latest version may be different, but the important part is to choose LTS 64 bit.
5. Click the Write button and the OS will be loaded on the SD card.
6. Once done the imager will say the card can be removed from the reader, click continue and close Raspberry Pi Imager.
7. Eject the card and reinsert it and if using Windows ignore any warnings that pop up.  
8. Go into the folder (if on Linux it's the system-boot folder), and open network-config with a text editor.
9. Comment out all the lines and add the below lines to the bottom, be sure to keep the spacing intact, note there are two spaces for the indent:

       version: 2
       renderer: networkd
       wifis:
         wlan0:
           dhcp4: true
           dhcp6: true
           optional: true
           access-points:
             "SSID":
                password: "PassPhrase"
                
10. Change SSID to the name of your WiFi (make sure it is in quotes) and same with the password (also in quotes).  The Raspberry Pi WiFi only works with 2.4 GHz WiFi's and will not work with 5 GHz ones.  Also if your WiFi has security, it will only work with WPA 2 Personal.
11. Next edit user-data in a text editor and add the following to the bottom (also keeping the spacing intact):

        ## Reboot after cloud-init completes
        power_state:
          mode: reboot

12. Save both files take out the card and insert into the Raspberry Pi.

## Step Three: Case Assembly
1. Follow the instructions for assembling the Raspberry Pi, touchscreen here, but see the below exceptions: https://cdn.shopify.com/s/files/1/0793/8029/files/touch_pro_assembly_instructions.pdf?v=1615409778
2. On Step 4 and 5, skip the red and black wires as they will not be needed.
3. Skip step 6 as we will be using the extender plate ordered from Smart Pi (number 6 on the parts list above).
4. On step 7 we will be mouting the Raspberry Pi at the inner position (the second image and instructions) and secure the Pi into that position using the standoffs that come with the cooler.  Also make sure the silver part of the ribbon cable faces the white part of the fastener.
5. Skip step 8, there won't be a camera.
6. On step 9, I used the adhesive front panel.
7. On step 11 use the door, but you may wish to drill small holes for air curculation for the fan on the cooler.
8. Skip step 12-14.
9. Before step 15 follow the below instructions.

