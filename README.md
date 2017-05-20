# panda
Quick Installation of Ubuntu Image on Pandaboard Rev A3; Host Windows 10
Step 1 :  Flashing Ubuntu Image on SD card ; 

refer this link for step by step indtallation https://wiki.ubuntu.com/ARM/OmapDesktopInstall

Step 2: PandaBoard setting a static IP
If you wish to set a static IP address for you Pandaboard you will need to do the following at the command line.

Please note that trying to use the graphical interface network manager will not work as the automatic IP address discovery process will occur at boot each time unless editing the /etc/network/interfaces file.
We will need several pieces of information before we set the IP.

Run: ifconfig
You will receive back an screen similar to this:

usb0  Link encap:Ethernet  HWaddr 1a:55:12:1c:49:33  
          inet addr:192.168.1.7  Bcast:192.168.1.255  Mask:255.255.255.0
          inet6 addr: fe80::a004:daff:febc:9927/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1492  Metric:1
          RX packets:910 errors:0 dropped:0 overruns:0 frame:0
          TX packets:561 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:78853 (78.8 KB)  TX bytes:77202 (77.2 KB)

We will copy some of this information in the next step.

To change from the default auto DHCP IP address allocation to static we need to edit the /etc/network/interfaces file. Using nano to edit it, we type the following at a command prompt:

sudo nano /etc/network/interfaces

It should already have the following:

auto lo
iface lo inet loopback

Below that add the following:

        auto usb0
        iface usb0 inet static
        address 192.168.1.10
        netmask 255.255.255.0
        network 192.168.0.0
        broadcast 192.168.1.255
        gateway 192.168.1.254

To exit out of nano, hit Ctrl+O, enter then Ctrl+X.
This assume you want to set the address to 192.168.1.10.  Feel free to change the value to suit your network.

Also make sure to compare the output of ifconfig above to ensure you have set the correct address for broadcast and gateway.   It you router uses 192.168.0.1 for example it is likely that gateway will need to be set to this value.

You may also need to ensure your Ubuntu installation is pointing to the correct DNS servers. If you get "unresolved name/address/location" errors etc then you may need to modify your "/etc/resolv.conf" file. To do this enter:

sudo nano /etc/resolv.conf

Then add a DNS server address or if your router supports it, your router's address:

nameserver 192.168.1.254

To restart the network interface and apply you changes type the following:

 sudo /etc/init.d/networking restart

If doing this over a secure SSH link, you will lose the connection after the network interface is taken down in the process, just log back in on the new IP address.

References: 1.  http://adventuresinsilicon.blogspot.de/2011/02/pandaboard-setting-static-ip.html
