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

There are two parts to the next section, the first is controlling the volume with the keyboard, you can skip this if using external speakers (not USB).  The second part is for turning off the screen.

### Volume Configuration

1. Edit/Create the following file:

       sudo nano /etc/triggerhappy/triggers.d/audio.conf
       
2. And add the following.

       KEY_VOLUMEUP    1      /usr/bin/amixer -c 1 -- set PCM 3+
       KEY_VOLUMEDOWN  1      /usr/bin/amixer -c 1 -- set PCM 3-
       KEY_MUTE        1      /usr/bin/amixer -c 1 -- set PCM toggle
       
3. Then add the following to the end of .bashrc.

       nano .bashrc
       
       sudo thd --daemon --triggers /etc/triggerhappy/triggers.d/ /dev/input/event*
      
4. Then reboot, once restared you will be able to controll the volume using the volume up, volume down and mute buttons.

### Turning on and off the screen

1. SSH into your Pi and edit the following file.

       sudo nano /etc/triggerhappy/triggers.d/backlight.conf
       
2. Add the following to it.

       KEY_COMPOSE     1      /home/ubuntu/turnOffBacklight.sh
       KEY_BACK        1      /home/ubuntu/turnOnBacklight.sh
       
3. Then edit the following file and add the following line to it.

       nano ~/turnOffBacklight.sh
       sudo -E sh -c 'echo 1 > /sys/class/backlight/rpi_backlight/bl_power'
    
4. Edit the following file and add the following line to it.

       nano ~/turnOnBacklight.sh
       sudo -E sh -c 'echo 0 > /sys/class/backlight/rpi_backlight/bl_power'

5. Then make both executable.

       chmod +x ~/turnO*
       
6. Then reboot to make the change stick.
7. On both sides of the keyboard there are a row of three buttons at a diagonal.  The button that will turn off the screen is bottom button on the right side that has two lines (the compose mail button), the button that turns on the screen is the bottom button on the left side of the screen (the back button).

This concludes this tutorial, you should now have an operational scanner. 

## Some Issues that Might Pop Up

I've found that after a reboot the waterfall will sometimes still start, if this happens, just click on View and click Disable Spectrum & Waterfall.
