
# Operating System and Software Install

## Update and Set Up SSH

1. Now that the case has been assembled and the OS is installed on the card, it's time to boot up for the first time.  Plug in and allow the Pi to boot up and you will see a log in prompt, but do not log in yet as it is setting up WiFi, wait until it reboots first and then log in with username ubuntu and password ubuntu. It will prompt you to change your password at this point. Once this is done do an update. You can use the small keyboard for this, but soon we will ssh to make this eaiser.
        
        sudo apt update
        sudo apt full-upgrade
        
2. Once this finishes, reboot and then install net tools.

        sudo apt install net-tools
        
3. This will allow you to determine the IP address, to do this type ifconfig and the following will return:

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
                
4. Where it says "inet" is the ip address of your Pi, from now you can SSH to your pi from your PC.  If you're on Windows, you can use Putty to do this: https://bitlaunch.io/blog/how-to-connect-to-ssh-with-putty/

## Install Software
1. Next install the necessary software, this will take some time, but stay next to your computer as a pop up will come up asking which display manager to use. 

        sudo apt install xorg openbox suckless-tools terminator lxpanel thunar lightdm pavucontrol

2. When the pop up comes up choose gdm3, tab to Ok and hit enter.  At this point, wait for the install to finish and then reboot.

        sudo reboot
        
## Login and Set Up Auto Login

1. Once the Pi has rebooted a login screen will come up, click on the Ubuntu user, but before entering the password, click on the the gear icon in the bottom right corner and choose Openbox, then enter your password and hit enter.
2. Once logged in, the screen will be blank with just a cursor, this is normal as Openbox is designed this way.  Right clicking will bring up a menu.
3. Next SSH into the Pi again and edit the following file:

        sudo nano /etc/gdm3/custom.conf
4. Find the following two lines and uncomment them and change user1 to ubuntu and then save the file (Ctrl+O, Enter, Ctrl+X)

        AutomaticLoginEnable = true
        AutomaticLogin = ubuntu

 
        
