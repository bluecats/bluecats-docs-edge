
## What's in the box?

The BlueCats Edge kit contains:

- BlueCats Edge
- Micro USB cable for power
- External Bluetooth antenna

## Connecting to the Edge

Start by plugging the supplied micro-usb cable into the edge and any USB port to power up the device and attach the external bluetooth antenna. Note that it may take 20-30 seconds to complete startup. The easiest way to configure the Edge is to connect it directly to your computer via Ethernet. Connect a straight-through ethernet cable betwen the ethernet port on your computer and the WAN port of the Edge. DHCP is enabled by default so with DHCP selected for your ethernet network adapter, your machine should be assigned an IP address in the range 192.168.8.0 for example:

![Connect to Edge using Ethernet](https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-getting-started/010-Connect.png "Connect with Ethernet")

Note the assigned IP address for later (in this case `192.168.8.147`) as this will be used as the test endpoint to receive scan data over UDP.

## Logging into the Edge Web Configuration

Each edge runs a local web configuration tool to manage both network configuration and BlueCats BLE scanning settings. After your computer has connected to the Edge over Ethernet, navigate to the Edge's IP address `192.168.8.1` using your web browser.

You will be prompted to log in with the root user credentials. The default login details are:

Username: `root`

Password: `bluecats`

![Log in to Edge Web Configuration](https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-getting-started/020-Login.png "Log in")

## Configuring BLE Scanner

Once in the web configuration tool there are a considerable number of configuration menu items but only a couple will require any changes. Some of the additional useful features are listed at the end of this document under [Troubleshooting](#troubleshooting).

To configure BLE advertisement scanning navigate to `BlueCats -> BC Config` in the main menu and make the following changes:

- *App Settings* These relate to fine tuning of the BLE scanning module and should be left unchanged as their defaults.
- *BC UDP Packet Destination* This is where the UDP endpoint is configured:
  - PORT: 9942. This can be changed but will need to match the destination port of the UDP client (which we'll be setting up next)
  - IP Address: This should be changed to the local IP address of the connected computer as shown in the first screenshot, in this example `192.168.8.147`.
  - Enabled: `1` This is a switch to turn off the UDP feature of the Edge `0` for off and `1` for on.
  
For now all other settings can be left as their defaults.

![Configure BLE Scanning and UDP endpoint](https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-getting-started/030-ConfigureScanAndUDP.png "Configure BLE Scanning")

## Receiving data with a simple UDP server

The Edge will now be sending data over UDP to the configured endpoint - in the above example `192.168.8.147:9942`. 

The UDP implementation sends a single line of comma-separated content for each detected BLE advertisement with the following data:

| Field            | Example
| ---              | ---
| Edge MAC Address | E4956E40DFCF
| BLE MAC Address  | A0E6F854703A
| RSSI             | -53
| Timestamp        | 1480314352373689
| BLE Payload      | 0201061AFF4C00021561687109905F443691F8E602F514C96D00040F82BD

e.g. 
```
E4956E40DFCF,A0E6F854703A,-63,1480314351666436,02010617FF0401050413012600040F82BD640391F8E602F514C96D0302C4FE
```

To receive the data we will need to set up a local UDP client to listen on this port. Here is an example in Python:

```python
import socket
from datetime import datetime

UDP_IP = "0.0.0.0"
UDP_PORT = 9942

sock = socket.socket(socket.AF_INET, # Internet
                     socket.SOCK_DGRAM) # UDP
sock.bind((UDP_IP, UDP_PORT))

while True:
    data, addr = sock.recvfrom(1024) # buffer size is 1024 bytes
    print datetime.now(), " -UDP- ", data
```
After [installing python](https://www.python.org/downloads/) (this example is for Python 2.7) and saving this script to a text file name `udp-server.py` this can then be run from the console using:

`<path to your python installation>/python.exe udp-server.py`

and each received message will be printed to the console e.g.

```
2016-11-28 17:25:51.569708  -UDP-  E4956E40DFCF,A0E6F854703A,-63,1480314351666436,02010617FF0401050413012600040F82BD640391F8E602F514C96D0302C4FE
2016-11-28 17:25:51.673411  -UDP-  E4956E40DFCF,A0E6F854703A,-51,1480314351770220,02010617FF0401050413012600040F82BD640391F8E602F514C96D0302C4FE
2016-11-28 17:25:51.788579  -UDP-  E4956E40DFCF,A0E6F854703A,-51,1480314351885285,02010617FF0401050413012600040F82BD640391F8E602F514C96D0302C4FE
2016-11-28 17:25:51.898095  -UDP-  E4956E40DFCF,A0E6F854703A,-54,1480314351994814,02010617FF0401050413012600040F82BD640391F8E602F514C96D0302C4FE
2016-11-28 17:25:51.956482  -UDP-  E4956E40DFCF,A0E6F854703A,-57,1480314352053244,02010617FF0401050413012600040F82BD640391F8E602F514C96D0302C4FE
2016-11-28 17:25:52.074793  -UDP-  E4956E40DFCF,A0E6F854703A,-62,1480314352171590,02010614FF04010997BD34DB5E40A7346BC6F681F1D3D50A
2016-11-28 17:25:52.185231  -UDP-  E4956E40DFCF,A0E6F854703A,-61,1480314352282016,02010614FF04010997BD34DB5E40A7346BC6F681F1D3D50A
2016-11-28 17:25:52.277347  -UDP-  E4956E40DFCF,A0E6F854703A,-60,1480314352373689,0201061AFF4C00021561687109905F443691F8E602F514C96D00040F82BD
2016-11-28 17:25:53.804152  -UDP-  E4956E40DFCF,A0E6F854703A,-60,1480314353900957,0201061AFF4C00021561687109905F443691F8E602F514C96D00040F82BD
2016-11-28 17:25:55.335417  -UDP-  E4956E40DFCF,A0E6F854703A,-64,1480314355431999,0201061AFF4C00021561687109905F443691F8E602F514C96D00040F82BD
2016-11-28 17:25:56.865335  -UDP-  E4956E40DFCF,A0E6F854703A,-60,1480314356961980,0201061AFF4C00021561687109905F443691F8E602F514C96D00040F82BD
2016-11-28 17:25:57.283202  -UDP-  E4956E40DFCF,A0E6F854703A,-63,1480314357380080,02010617FF0401050413012600040F82BD640391F8E602F514C96D0302C4FE
2016-11-28 17:25:58.042214  -UDP-  E4956E40DFCF,A0E6F854703A,-69,1480314358139068,02010617FF0401050413012600040F82BD640391F8E602F514C96D0302C4FE
2016-11-28 17:25:58.816214  -UDP-  E4956E40DFCF,A0E6F854703A,-75,1480314358912551,0201061AFF4C00021561687109905F443691F8E602F514C96D00040F82BD
```
## Connecting to internet via WIFI

The Edge can be configured to send data to another machine on the local WIFI network, or connected to the internet to enable communication to the BlueCats Cloud or to download updated software.

Navigate to `Network` -> `Wireless` in the main menu and the scan for available networks

![Wireless](https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-getting-started/070-ConfigureWireless.png "Wireless")

Join an available network

![Join network](https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-getting-started/070-ConfigureWireless-2.png "Join network")

Enter network key and submit

![Enter key](https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-getting-started/070-ConfigureWireless-3.png "Enter key")

Save and apply changes

![Save and apply changes](https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-getting-started/070-ConfigureWireless-4.png "Save and apply")

Once connected to the local WIFI network, the Edge can be [configured to send to an IP address](#configuring-ble-scanner) on that network and the ethernet cable can then be disconnected as the Edge will continue sending data over the local WIFI connection while powered.

## Troubleshooting

### Viewing local logs

The local device logs can be used to see if beacons are being detected and if they are being successfully sent to the configured endpoint. For example the following logs show that the UDP endpoint is not available:

![View local logs](https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-getting-started/050-View-Logs.png "View Logs")

### Resetting Edge To Default Firmware When Locked Out
1. Connect edge WAN into your computer with ethernet cable
2. Unplug edge power
3. Hold Reset
4. Plug in power while holding reset. Edge will flash some LEDs, and then start blinking red LED. Let red LED blink 6 times, then let go of reset. You should see the red LED blink fast for 1 sec.
5. Disable your wifi, and set your own ip to 192.168.1.2 for your ethernet interface
![Network Settings](https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-getting-started/network-settings.jpg "Network Settings")
6. Go to 192.168.1.1 in your web browser
7. Get our firmware, upload and apply it
![Upload Firmware](https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-getting-started/upload-firmware-pic.jpg "Upload Firmware")
8. Wait a minute. Set your ip to something like 192.168.8.118 (just not 192.168.8.1) for the ethernet interface and go to 192.168.8.1 in your browser You should see the Bluecats OpenWrt web interface
