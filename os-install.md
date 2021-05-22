
# Operating System and Software Install

## Update and Set Up SSH

1. Now that the case has been assembled and the OS is installed on the card, it's time to boot up for the first time.  Plug in and allow the Pi to boot up and you will see a log in prompt, but do not log in yet as it is setting up WiFi, wait until it reboots first and then log in with username ubuntu and password ubuntu. It will prompt you to change your password at this point. Once this is done do an update. You can use the small keyboard for this, but soon we will ssh to make this eaiser.
        
        sudo apt update
        sudo apt full-upgrade
        
2. Once this finishes, reboot and then install net tools.

        sudo apt install net-tools
        
3. This will allow you to determine the IP address, to do this type ifconfig and the following will return.

        lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
                inet 127.0.0.1  netmask 255.0.0.0
                inet6 ::1  prefixlen 128  scopeid 0x10<host>
                loop  txqueuelen 1000  (Local Loopback)
                RX packets 126  bytes 10146 (10.1 KB)
                RX errors 0  dropped 0  overruns 0  frame 0
                TX packets 126  bytes 10146 (10.1 KB)
                TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

        wlan0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
                inet 192.168.1.117  netmask 255.255.255.0  broadcast 192.168.1.255
                inet6 <your mac address>  prefixlen 64  scopeid 0x20<link>
                ether <your mac address>  txqueuelen 1000  (Ethernet)
                RX packets 3174  bytes 4587982 (4.5 MB)
                RX errors 0  dropped 0  overruns 0  frame 0
                TX packets 2153  bytes 207662 (207.6 KB)
                TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
                
4. Where it says "inet" is the ip address of your Pi, from now you can SSH to your pi from your PC.  If you're on Windows, you can use Putty to do this. https://bitlaunch.io/blog/how-to-connect-to-ssh-with-putty/

## Install Software and Overclock
1. Next install the necessary software, this will take some time, but stay next to your computer as a pop up will come up asking which display manager to use. 

       sudo apt install xorg openbox suckless-tools terminator lxpanel thunar lightdm pavucontrol rtl-sdr git alsamixergui unclutter triggerhappy

2. When the pop up comes up choose gdm3, tab to Ok and hit enter.  At this point, wait for the install to finish.
3. As an optional step we can overclock the Pi so it will perform better.  Only do this if you purchased and installed the CPU fan otherwise the Pi will overheat and may become unstable.  Edit the following file.

       sudo nano /boot/firmware/config.txt
       
4. Under the section called [pi4] but before the next section, add this.

       over_voltage=6
       arm_freq=2000
       
5. Save the file and reboot.

       sudo reboot
        
## Login and Set Up Auto Login

1. Once the Pi has rebooted a login screen will come up, click on the Ubuntu user, but before entering the password, click on the the gear icon in the bottom right corner and choose Openbox, then enter your password and hit enter.
2. Once logged in, the screen will be blank with just a cursor, this is normal as Openbox is designed this way.
3. Next SSH into the Pi again and edit the following file.

        sudo nano /etc/gdm3/custom.conf
        
4. Find the following two lines and uncomment them and change user1 to ubuntu and then save the file (Ctrl+O, Enter, Ctrl+X).

       AutomaticLoginEnable = true
       AutomaticLogin = ubuntu
       
5. Add the file Xdefaults.

       cd
       nano .Xdefaults

6. And add the following into .Xdefaults.

       XTerm*geometry: 800x400
       
## Set Up RTL SDR Rules

1. Create the following file.

       sudo nano /etc/udev/rules.d/20.rtlsdr.rules

2. Add the following to it.

       SUBSYSTEM=="usb", ATTRS{idVendor}=="0bda", ATTRS{idProduct}=="2838", GROUP="adm", MODE="0666", SYMLINK+="rtl_sdr"

## Install Java
1. SSH into the Pi and download Java and install it.

       wget https://download.bell-sw.com/java/15.0.1+9/bellsoft-jdk15.0.1+9-linux-aarch64-full.deb
       sudo apt install ./bellsoft-jdk15.0.1+9-linux-aarch64-full.deb
       
2. Typing "java -version" should print the following.

       ubuntu@ubuntu:~$ java -version
       openjdk version "15.0.1" 2020-10-20
       OpenJDK Runtime Environment (build 15.0.1+9)
       OpenJDK 64-Bit Server VM (build 15.0.1+9, mixed mode)

4. Next set the JAVA_HOME variable, edit /etc/profile.

       sudo nano /etc/profile
       
4. Then edit the file to look like this which will be the third line of the file right after the comments.

       # /etc/profile: system-wide .profile file for the Bourne shell (sh(1))
       # and Bourne compatible shells (bash(1), ksh(1), ash(1), ...).
       export JAVA_HOME=/usr/lib/jvm/bellsoft-java15-full-aarch64
       
5. Save the file and then reboot.

## Install SDR Trunk
1. Clone the SDR Trunk application.

       git clone https://github.com/DSheirer/sdrtrunk.git
       cd sdrtrunk
        
2. Build the application.

       ./gradlew clean build
       
3. Once finished copy the built zip file to your home directory.

       cp build/distributions/sdr-trunk-0.5.0-alpha6.zip ~
       
4. Then go to your home directory and upzip it.

       cd
       unzip sdr-trunk-0.5.0-alpha6.zip
       
5. Next download the audio library and unzip it.

       wget https://github.com/DSheirer/jmbe/releases/download/v1.0.7b/jmbe-creator-linux-x86_64-v1.0.7b.zip
       unzip jmbe-creator-linux-x86_64-v1.0.7b.zip
       
6. After unzipping it we're going to use our Java version rather than the one that comes with it and then build the library.

       cd creator-linux-x86_64-v1.0.7b/bin
       rm java
       ln -s /usr/bin/java java
       ./creator
       
7. On the Rasperry Pi (not in SSH) right click and choose terminal emulator and then start SDR Trunk, this will be a test run.
   
       cd sdr-trunk-0.5.0-alpha6/bin
       ./sdr-trunk
       
8. Once the application has started, drag it over until you can see the top right corner and click the maximize button which will resize it to fit the screen, then click View and Disable Spectrum & Waterfall
9. Then click View and User Preferences and under Decoder choose JMBE Audio Library, click on Select... click on Home in the left panel, double click creator-linux-x86_64-v1.0.7b and choose jmbe-1.0.7b.jar and click Open.
10. At this point you should have a working application.  You can exit out of the application for now.

## SDR Trunk Setup

I find this next part I find easier to do on a PC and then tranfer over to the Pi since the Pi is slow and the screen is very small. So to install SDR Trunk on your PC go to https://github.com/DSheirer/sdrtrunk/releases choose the latest release and find the Asset link and choose the one for your system.  Unzip it go the bin file an choose sdr-trunk to start it. 

1. Open SDR trunk, click on View and Playlist editor and then click on the Radio Reference tab.
2. Click on Login and enter your Radio Reference username and password.  If you do not have one, you can sign up here: https://register.radioreference.com/
3. Once logged in, choose your country, state and county.  Choose the system for your area, in the case of Minnesota the entire state uses Allied Radio Matrix for Emergency Response (ARMER), refer to Radio Reference to determine this for your area.  In the System View tab, sort by county choose your county. Click on New Alias List and create an alias and then choose it in the drop down. ![Playlist Editor System View](/images/playlistEditorSystemView.png)
4. Click on the Talkgroup View tab choose the alias you created in the last tab then under category choose the talk groups you wish to monitor.  In the case of Minnesota, you can choose by county or you can choose (All Talkgroups) if you wish to monitor all agencies state-wide that use this system.  Then click Import All Talkgroups. ![Playlist Editor Talkgroup View](/images/playlistEditorTalkgroupView.png)
5. Next go back to the System View tab and click Create Channel Configuration and make sure the Go To Channel Editor box is checked. 
6. On the Channels tab click on the new channel list, click Auto-Start, choose your alias listand on Decoder change Max Traffic Channels to 2.  You may experiment with 3 as well. ![Channel List](/images/channelList.png)
7. Next click on the Aliases tab.  There's a couple options here, if you chose talk groups from a specific county or department and you don't want to hear other traffic, create a omission list of talkgroups you don't want to listen to.  To do this click on New in the right side, give the alias a name of Omitted.  Turn off the listen tab and then in the Identifiers section click the Add Identifier dropdown and choose APCO-25 and Talkgroup range. ![Aliases One](/images/aliases1.png)
8. Then change the range from 0 to 65535 and click Save.  Once you have clicked Save the application will appear to freeze, but be patient and it will respond again after a few minutes. ![Aliases One](/images/aliases2.png)
9. As an optional step, there are icons for police, fire, ambulance and others along with priorities and colors, if you wish you can assign these to the different aliases or just leave them as is.
10. Next we are going to copy the configuration to the Raspberry Pi using scp (secure copy) this copies files over SSH.  If you are using Windows, Putty has an SCP client: https://it.cornell.edu/managed-servers/transfer-files-using-putty

        scp SDRTrunk/playlist/default.xml ubuntu@192.168.1.106:/home/ubuntu/SDRTrunk/playlist
        
11. SSH into the Raspberry Pi and edit/create the below file and add the following line to it.

        nano ~/SDRTrunk.properties
        spectral.display.enabled=false
      
12. Once copied, go back to the Rasbperry Pi and right click to open a terminal and type the following commands.

        cd sdr-trunk-0.5.0-alpha6/bin
        ./sdr-trunk
        
13. At this point you should have a working SDR trunk application.  If you are using powered computer speakers, connect them to the 3.5 mm port and you should have sound. If you purchased a USB speaker go back to the terminal by typing Alt-Tab and then open a new terminal tab by typing Ctrl+Shift+T then type the following.

        pavucontrol
        
14. This will open a new window click on the Output Devices tab and click the green button to the right of USB2.0 Device Analog Stero and this should enable sound for the USB speaker.  The sound adjustment for this application is not very good so to adjust the sound close this application and type the following.

        alsamixer
        
15. This will bring up a command line volume control.  Type F6 (the function keys at the top of your keyboard) and choose USB2.0 Device then you can use the up and down arrows to adjust the volume.  We will be adding updates to all of this to make it more automatic.

The last part will be the final steps to get it to start up automatically and sound/screen options.

[Final Steps](final-touches.md)
