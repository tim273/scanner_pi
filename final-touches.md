# Final Steps

This is the last set of steps for auto start, sound options and using the keyboard.

## Auto Start

1. SSH into your Raspberry Pi and edit the following file (it probably won't exist so you'll need to create it).

       mkdir ~/.config/openbox
       nano ~/.config/openbox/autostart
       
2. Add the following to it.

       xset s off
       xset s noblank
       xset -dpms

       unclutter &
       terminator &
       /home/ubuntu/sdr-trunk-0.5.0-alpha6/bin/sdr-trunk &

3. Then reboot and a terminal and SDR Trunk should start up automatically.

## Sound Configuration

If you are using external speakers plugged into the 3.5 mm port skip this section, this is only if you are using a USB speaker.

1. SSH into the Pi and edit the following file.

       sudo nano /boot/firmware/usercfg.txt
       
2. Then add this after the comments.

       dtparam=audio=off
       
3. Then reboot and the onboard audio will be disabled and the USB audio should work automatically.

## Keyboard Configuration

1. 
