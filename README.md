# WeatherFlow_PyConsole
Rasberry Pi Python console for WeatherFlow Smart Weather Station

Follow these instructions to get WeatherFlow PyConsole setup and running on your 
Raspberry Pi Official 7 inch touch screen. The installation should take ~1 hour.

The instructions assume you have already sucesfully setup your Raspberry Pi, 
have attached the touch screen, and have either a keyboard and mouse attached 
directly to the Pi, or you can access the Pi through SSH/VNC. If you are 
starting from scratch, some of these links may help get you started:

* https://www.raspberrypi.org/downloads/raspbian/
* https://www.raspberrypi.org/documentation/configuration/security.md
* https://www.raspberrypi.org/documentation/remote-access/ssh/
* https://www.raspberrypi.org/documentation/remote-access/vnc/

---
Step 1: Make sure your Raspberry Pi is fully up to date
---

```
sudo apt-get update && sudo apt-get dist-upgrade
```

---
Step 2: Upgrade the Python 3 version of Pip
---

```
python3 -m pip install --upgrade pip
```	

---
Step 3: Confirm current user is in "input" and "video" groups
---

```
sudo usermod -a -G input,video $(whoami)
```

---
Step 4: Install required dependencies
---

`sudo apt-get install libsdl2-dev libsdl2-image-dev libsdl2-mixer-dev libsdl2-ttf-dev pkg-config libgl1-mesa-dev libgles2-mesa-dev python-setuptools libgstreamer1.0-dev git-core gstreamer1.0-plugins-{bad,base,good,ugly} gstreamer1.0-{omx,alsa} python-dev libmtdev-dev xclip xsel libatlas-base-dev`

```
y (confirms you want to install dependencies)
[press enter]
```

---
Step 5: Install required Python modules
---

```
sudo python3 -m pip install autobahn[twisted] pytz pyasn1-modules service_identity geopy ephem Cython
```

---
Step 6: Install Kivy Python library - this make take some time
--- 

```
sudo python3 -m pip install git+https://github.com/kivy/kivy.git@master`
```

---
Step 7: Configure Kivy for Raspberry Pi touchscreen
---
Run Kivy for the first time to generate configuration files:

```
python3 -c "import kivy"
```

Open file ".Kivy/config.ini" in Nano:

```
nano ~/.kivy/config.ini`
```

Delete everything in the [input] section. Add this:

```
mouse = mouse
mtdev_%(name)s = probesysfs,provider=mtdev
hid_%(name)s = probesysfs,provider=hidinput
```	

Save changes in Nano:

```
ctrl-x
y (confirms you want to save changes)
[press enter]
```

Reboot the system:

```
sudo reboot now
```

---
Step 8: Download WeatherFlow PyConsole source code
---

```
cd && mkdir WeatherFlow && cd WeatherFlow
wget https://github.com/peted-davis/WeatherFlow_PyConsole/archive/v0.1-beta.tar.gz -O PyConsole.tar.gz
tar -xvf PyConsole.tar.gz --strip 1
rm PyConsole.tar.gz
```

---
Step 9: Configure WeatherFlow PyConsole
---
To get the WeatherFlow PyConsole up and running, you need to specify your 
station number in the configuration file, as well as one API key needed to
determine the the station location (country) from its latitude/longitude, and 
one needed to download an apppropriate weather forecast.

First, open the PyConsole .ini file:

```
nano WeatherFlowPyConsole.ini
```
	
Then, go to http://www.geonames.org/login and register for a new account. Once
you have registered, go to your account (http://www.geonames.org/manageaccount)
and activate "Free Web Services". Once this is done, type your username into the
'GeoNamesKey' variable in WeatherFlowPyConsole.ini. Do not enclose your username
in quotation marks!

Second, if you live in the UK go to the UK MetOffice DataPoint homepage
(https://www.metoffice.gov.uk/datapoint), and register for a new account. Copy
your API key into the 'MetOfficeKey' variable in WeatherFlowPyConsole.ini. Again
no quotation marks.

If you live outside the UK, leave the 'MetOfficeKey' variable blank and register
instead for a DarkSky API account (https://darksky.net/dev/register). Copy your
API key into the 'DarkSkyKey' variable.

You should now have a username in the 'GeoNamesKey' variable, and an API key in
either the 'MetOfficeKey' variable if you live in the UK, or in the 'DarkSkyKey' 
variable if you live elsewhere. Next type your station ID into the 'StationID'
variable. 

The console is designed to display data from the first Air and Sky module
it finds associated with your station. If you have multiple modules, write the 
names of the modules you wish to display in the 'AirName' and 'SkyName' 
variables. 

Save your changes in nano:

```
ctrl-x
y (confirms you want to save changes)
[press enter]
```
	
---
Step 10: Run WeatherFlow PyConsole
---
Time to run the WeatherFlow PyConsole:

```
python3 main.py
```
