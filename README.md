# Meshtastic-python

A python client for using [Meshtastic](https://www.meshtastic.org) devices. This small library (and example application) provides an easy API for sending and receiving messages over mesh radios. It also provides access to any of the operations/data available in the device user interface or the Android application. Events are delivered using a publish-subscribe model, and you can subscribe to only the message types you are interested in.

Full documentation including examples [here](https://meshtastic.github.io/Meshtastic-python/meshtastic/index.html).

Installation is easily done through the Python package installer pip (note, you must use pip version 20 or later):

- check that your computer has the required serial drivers installed, if not download them from [here](https://www.silabs.com/developers/usb-to-uart-bridge-vcp-drivers).
- check that your computer has Python 3 installed.
- check that your computer has “pip3” installed, if not follow [this guide](https://www.makeuseof.com/tag/install-pip-for-python/).
- check that pytap2 is installed by pip3. If not, install it:
```
sudo pip3 install --upgrade pytap2
```
- install meshtastic:
```
sudo pip3 install --upgrade meshtastic
```

An example using Python 3 code to send a message to the mesh:
```
import meshtastic
interface = meshtastic.SerialInterface() # By default will try to find a meshtastic device, otherwise provide a device path like /dev/ttyUSB0
interface.sendText("hello mesh") # or sendData to send binary data, see documentations for other options.
interface.close()
```

For the rough notes/implementation plan see [TODO](https://github.com/meshtastic/Meshtastic-python/blob/master/TODO.md).

## Command line tool

This pip package will also install a "meshtastic" command line executable, which displays packets sent over the network as JSON and lets you see serial debugging information from the meshtastic devices. The source code for this tool is also a good [example](https://github.com/meshtastic/Meshtastic-python/blob/master/meshtastic/__main__.py) of a 'complete' application that uses the meshtastic python API.

NOTE: This command is not run inside of python, you run it from your operating system shell prompt directly.  If when you type "meshtastic" it doesn't find the command and you are using Windows: Check that the python "scripts" directory [is in your path](https://datatofish.com/add-python-to-windows-path/).

To display a (partial) list of the available commands:
```
meshtastic -h
```

You can also use this tool to set any of the device parameters which are stored in persistent storage. For instance, here's how to set the device
to keep the bluetooth link alive for eight hours (any usage of the bluetooth protcol from your phone will reset this timer)

```
meshtastic --set wait_bluetooth_secs 28800
Connected to radio...
Setting preference wait_bluetooth_secs to 28800
Writing modified preferences to device...
```

Or to set a node at a fixed position and never power up the GPS.

```
meshtastic --setlat 25.2 --setlon -16.8 --setalt 120
```

Or to configure an ESP32 based board to join a wifi network as a station (wifi support in the device code is coming soon):

```
meshtastic --set wifi_ap_mode false --setstr wifi_ssid mywifissid --setstr wifi_password mywifipsw
```

Or to configure an ESP32 to run as a Wifi access point:

```
meshtastic --set wifi_ap_mode true --setstr wifi_ssid mywifissid --setstr wifi_password mywifipsw
```

For a full list of preferences which can be set (and their documentation) see [here](https://github.com/meshtastic/Meshtastic-protobufs/blob/master/docs/docs.md#.RadioConfig.UserPreferences).

## FAQ/common problems

This is a collection of common questions and answers from our friendly forum.

### [Permission denied: ‘/dev/ttyUSB0’](https://meshtastic.discourse.group/t/question-on-permission-denied-dev-ttyusb0/590/3?u=geeksville)

This indicates an OS permission problem for access by your user to the USB serial port.  Typically this is fixed by the following.
```
sudo usermod -a -G dialout <username>
```

## Required device software version

This API and tool both require that the device is running Meshtastic 0.6.0 or later.
