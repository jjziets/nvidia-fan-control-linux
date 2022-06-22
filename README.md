# nvidia-fan-control-linux

#Before running insall the following 
``` 
sudo apt-get update && sudo apt-get -y upgrade && sudo apt-get install -y libgtk-3-0 && sudo apt-get install -y xinit && sudo apt-get install -y xserver-xorg-core && sudo apt-get remove -y gnome-shell && sudo update-grub && sudo nvidia-xconfig -a --cool-bits=28 --allow-empty-initial-configuration --enable-all-gpus
```

There are 3 version  of this scrip
fan-control.sh is used with a desktop and requires that you have an x-server running

both fan-control4.sh and fan-control8.sh will work witout but due to some quirek in nvidia driver the fans dont follow the same logical order as the gpus. The two scripts are maped for 4 gpus and 8 gpus systems. This might not work fror systems so check that the fans chagnes correctly. 


Fan Curve Control Script for Nvidia GPUs on Linux

In order for this script to work, coolbits must be enabled in xorg.conf

This script allows directly setting fan speed on Nvidia GPUs either manually or with a "fan curve".
Supports Day and Night fan curves

While the default fan curve settings will work, you may want to customize them to meet your needs or
preferences.

Add a line to cron like this to enable automatic fan control:

```
* * * * *	~/fan-control.sh curve
```

If you don't wish to use cron but instead prefer a persistant running script in the background then you can!

Run the script in a terminal window with
```
$ fan-control.sh pcurve
```

Or have it started on boot with sudo crontab -e assuming the file is located under /root/fan-control/
```
sudo mkdir /root/fan-control/
sudo  wget https://raw.githubusercontent.com/jjziets/nvidia-fan-control-linux/master/fan-control4.sh
cd /root/fan-control/ 
sudo chmod +x fan-control4.sh
sudo (crontab -l; echo "@reboot screen -dmS fan-control /root/fan-control/fan-control4.sh pcurve ") | crontab -
```

Or set the script to run at login in the background
```
fan-control.sh pcurve &
```

Currently to adjust Fan Curve settings you must manually edit the 'Configurable Settings' section of the script. 
I want to eventually load it from a seperate config file but haven't bothered yet.

Example of Info Screen:

```
$ fan-control info
| Card |		| Fan Speed |	| Fan RPM |	| GPU Temp |
0: GeForce GTX 1080 Ti	     80%	    2632	     52°
1: GeForce GTX 1080 Ti	     80%	    2635	     53°
2: GeForce GTX 1080 Ti	     80%	    2661	     52°
3: GeForce GTX 1080	     70%	    2260	     48°
4: GeForce GTX 1080	     80%	    2908	     50°
5: GeForce GTX 1070 Ti	     90%	    3150	     55°
6: GeForce GTX 1070 Ti	     80%	    2802	     50°

```

Example of Setting Speed:
Note that setting a speed manually will disable fan curve if set in cron until 'set curve' command is given.
Setting speed manually will also stop a background running persistant curve. You will have to either start
a new persistant curve in the foreground or relaunch it into the background.

```
$ fan-control.sh set 75        OR      $ fan-control.sh s 75

$ fan-control.sh set default   OR      $ fan-control.sh s d

$ fan-control.sh set max       OR      $ fan-control.sh s m

$ fan-control.sh set off       OR      $ fan-control.sh s off

$ fan-control.sh set curve     OR      $ fan-control.sh s c
```

I hope this works for all of you as well as it does for me!
