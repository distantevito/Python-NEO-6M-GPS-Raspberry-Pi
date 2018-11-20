# Python-NEO-6M-GPS-Raspberry-Pi
Python script for the NEO-6M GPS module on the Raspberry Pi
## Connecting Schema
![Image of Yaktocat](https://raspberrytips.nl/wp-content/uploads/2016/12/UBOLX-NEO-6M-RPI-600x274.png)
![Image of Yaktoc2at](https://www.raspberrypi-spy.co.uk/wp-content/uploads/2012/06/Raspberry-Pi-GPIO-Layout-Model-B-Plus-rotated-2700x900.png)
![Image of Yaktoc2at2](
http://www.gtkdb.de/images/00532_Raspberry_Pi_NEO-6M_GPS-Modul_-_Schaltplan.png)
## Dependencies
* pip installed.
```
sudo apt-get install python-pip
```
* you will need pynmea2.
```
sudo pip install pynmea2
```
* You need the GPS software
```
sudo apt-get install gpsd gpsd-clients python-gps minicom
```
## Configuration
* Serial
```
sudo nano /boot/cmdline.txt
```
and replace all with the following lines:
```
dwc_otg.lpm_enable=0 console=tty1 root=/dev/mmcblk0p2 rootfstype=ext4 elevator=deadline rootwait
```
* reboot the system
```
sudo reboot now
```
## Getting Started
These instructions will get you a quick start with the script and please check before if you have the dependencies installed. Also connect the raspberry like the obove schemata.
* Look if the terminal output of the sensor works
```
cat /dev/ttyAMA0
```
or use:
```
cgps -s
```
* Run the script
```
cd Python-NEO-6M-GPS-Raspberry-Pi
sudo python Neo6mGPS.py
```

## Example Code
```
import serial
import pynmea2
def parseGPS(str):
    if str.find('GGA') > 0:
        msg = pynmea2.parse(str)
        print "Timestamp: %s -- Lat: %s %s -- Lon: %s %s -- Altitude:
%s %s" %
(msg.timestamp,msg.lat,msg.lat_dir,msg.lon,msg.lon_dir,msg.altitude,m
sg.altitude_units)
serialPort = serial.Serial("/dev/ttyAMA0", 9600, timeout=0.5)
while True:
    str = serialPort.readline()
    parseGPS(str)

```
