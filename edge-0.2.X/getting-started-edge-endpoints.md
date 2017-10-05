# BlueCats Edge Endpoints - Overview

- [BlueCats Edge Endpoints](#bluecats-edge-endpoints---overview)
- [Live View](#live-view)
- [Configure Endpoints](#configure-endpoints)
- [Configure Applications](#configure-applications)
    - [Scan and Forward](#application---scan-and-forward)
    - [Proximity](#application---proximity)
    - [Sensor Measurement](#application---sensor-measurement)
    - [BlueCats Beacon Health](#application---bluecats-beacon-health)
- [Example UDP Server](getting-started-edge-applications.md#example---receiving-data-with-a-simple-udp-server)

The Edge Relay provides the ability to send your data that you want to various endpoint types (UDP, MQTT, or HTTP) and message formats (CSV, JSON, or Binary). With the Edge you can:

- Configure network settings for connectivity to local network and the internet. The BlueCats Edge is a Wireless Access Point providing advanced network configuration options if required.
- View beacons in range of the Edge with Live View. 
- Configure filters for your endpoints. This provides a way to filter meaningful data to your endpoints. 

For endpoint type (UDP, MQTT, or HTTP), message format (CSV, JSON, or Binary) and message throttle limits for can be independently configured or the application can be disabled if not required.

## Edge Configuration

Once you have [connected to the Edge and configured network settings](getting-started-connect.md) you can manage sending data to your specified endpoints on the Edge.

### Live View

Live view provides a simple list of BLE devices detected in range of the Edge along with their current RSSI (signal strength).

<p style="text-align:center" align="center"><img width="600" src="https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/EdgeDocuments/EdgeLiveViewApplication.png" alt="Live View"/></p>

## Configure Endpoints

To make use of the data collected by the Edge, you will need to send it somewhere. The default Edge applications support three types of endpoint - UDP, MQTT, or HTTP. Configure one or all three of the endpoint options. Currently, the Edge supports a as many endpoints for each type. Data will be sent to the configured endpoints when an Edge application has been enabled.

* Click on *Add New Addpoint* to start configuring endpoints 

<p style="text-align:center" align="center"><img align="center" width= "600" src="https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/EdgeDocuments/EdgeConfigureEndpointsApplication.png" alt="Endpoints"/></p>


### UDP
To configure UDP:

1. Name your endpoint 
2. Click the *Enabled* checkbox to enable your endpoint
3. Select UDP for your Protocol
4. Enter the IP and PORT where the data should be sent.
5. Select the format for which your data will come in: JSON, CSV, or Binary
6. Click *Save and Apply*

<p align="center"><img width="600" src="https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/EdgeDocuments/EdgeUDPEndpoint.png" alt="Endpoints"/></p>


### MQTT

A simple MQTT endpoint without TLS enabled just requires the host/IP and port of the MQTT server and the 'TLS Enabled' set to `0`. Some MQTT endpoints such as AWS IoT require TLS for connections. Here you will need to upload the Certificate, Private Key and Certificate Authority [generated in the AWS console](http://docs.aws.amazon.com/iot/latest/developerguide/create-device-certificate.html).

<p style="text-align:center" align="center"><img align="center" width="600" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-features/EndpointConfiguration-Certificates.png" alt="MQTT Certificates"/></p>

### HTTP 
To configure a HTTP endpoint: 

1. Name your endpoint 
2. Click the *Enabled* checkbox to enable your endpoint
3. Select HTTP for your Protocol
4. Enter the IP and PORT where the data should be sent.
5. You will only be able to receive JSON data for HTTP
6. Add any other features you want to implement
7. Click *Save and Apply*


What you can configure is what Ad-type to indentify the beacon in JSON
* Secure
* Eddystone UID
* iBeacon
* bcId (bluecats identifier)

<p align="center"><img width="600" src="https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/EdgeDocuments/EdgeHTTPEndpoint.png" alt="HTTP"/></p>

### CSV

Example: CSV (for UDP, MQTT)
```
BCAdData,1,E4956E40DFCF,Edge-abc,A0E6F854703A,-63,-62,1480314351666436,02010617FF0401050413012600040F82BD640391F8E602F514C96D0302C4FE
```

| Field            | Example
| ---              | ---
| Message Type     | BCAdData (CSV only)
| Message Version  | 1 (CSV only to track format changes)
| Edge MAC Address | E4956E40DFCF
| Edge Name        | Edge-abc
| BLE MAC Address  | A0E6F854703A
| RSSI             | -63
| RSSI Smooth      | -62
| Timestamp        | 1480314352373689
| AdData           | 0201061AFF4C00021561687109905F443691F8E602F514C96D00040F82BD

- Message Type (CSV only) 
- Message Version  | 1 (CSV only to track format changes)
- EdgeMAC - Hardware MAC address of the Edge
- EdgeName - A user defined name for the Edge
- BeaconMAC - Scanned (public) BT Address of the detected BLE device
- RSSI - Received Signal Strength Indicator in dBm
- RSSI Smooth - Received Signal Strength Indicator in dBm. Signal fluctuations are filtered to provide a more stable value.
- Timestamp - Nanoseconds since Epoch
- AdData - Full advertisement data payload




### JSON 
Example: JSON (for MQTT,UDP)
```
{
    "edgeMAC":"E4956E4CC101",
    "devId":5063816,
    "mac":"98072D06813B",
    "mPow":-67,
    "rssi":-74,
    "rssiSmooth":-72,
    "adMsk":512,
    "channel":37,
    "adType":512,
    "ts":"2017-10-03T18:19:13.230952Z",
    "pBatt":83,
    "btAddr":"98072D06813B",
    "vSett":2,
    "fwUID":"5000006e",
    "adData":"02010611FF04010098072D06813B5000006E02BD53"

}
```
| Field            | Example
| ---              | ---
| Edge MAC Address | E4956E40DFCF
| Device-ID        | 5063816 (ignore) 
| MAC Address      | 98072D06813B
| Measured Power   | -67
| RSSI             | -63
| RSSI Smooth      | -62
| Ad mask          | 512
| Channel          | 37
| Timestamp        | 2017-10-03T18:19:13.230952Z
| Battery Power    | 83
| BLE MAC Address  | 98072D06813B
| Version Setting  | 2
| Firmware UID     | 5000006e
| AdData           | 0201061AFF4C00021561687109905F443691F8E602F514C96D00040F82BD

- EdgeMAC - Hardware MAC address of the Edge
- Device-ID - For internal use, not a constant (ignore)
- MAC Address - The MAC Adress of the Beacon
- Measured Power - Measured power at 1 metre
- RSSI - Received Signal Strength Indicator in dBm
- RSSI Smooth - Received Signal Strength Indicator in dBm. Signal fluctuations are filtered to provide a more stable value.
- Ad Mask - The advertisement mask 
- Channel - The Ad channel the Beacon Ad was received on
- Timestamp - Nanoseconds since Epoch
- Battery Power - Measured Battery power 
- BLE MAC Address - Scanned (public) BT Address of the detected BLE device
- Version Setting - Version Setting the Beacon is on 
- Firmware UID - Firmware unique identifier (Beacon Serial Number) 
- AdData - Full advertisement data payload

For all of the protocols with JSON, you can add additional Key, Values.
For HTTP, you can also add Additional HTTP Headers.

<p style="text-align:center" align="center"><img align="center" style="max-width: 400px;" src="https://s3-us-west-1.amazonaws.com/github-photos/DeveloperDocs/EdgeDocuments/JSONKeyValue.png" alt="JSON Key Value Pairs"/></p>


Example: JSON for HTTP 

```
{
    "edgeMAC":"E4956E4CC101",
     "loc":{},"devices":[
            {
                "mac":"73118A97ACC0",
                "mPow":-67,
                "rssi":-77,
                "rssiSmooth":-77,
                "ts":"2017-10-04T21:37:43.793701Z",
                "pBatt":46
            },
            {
                "mac":"5332F1D19772",
                "mPow":-67,
                "rssi":-83,
                "rssiSmooth":-82,
                "ts":"2017-10-04T21:37:41.099714Z",
                "pBatt":95
            }]
}
```

| Field            | Example
| ---              | ---
| Edge MAC Address | E4956E40DFCF
| Location         | (ignore)
| ---              | ---
| **Devices**      | 
| MAC Address      | 98072D06813B
| Measured Power   | -67
| RSSI             | -63
| RSSI Smooth      | -62
| Timestamp        | 2017-10-03T18:19:13.230952Z
| Battery Power    | 83


- EdgeMAC - Hardware MAC address of the Edge
- Location - Ignore
- Devices- - JSON List of all seen devices
- MAC Address - The MAC Address of the Beacon
- Measured Power - Measured power at 1 metre
- RSSI - Received Signal Strength Indicator in dBm
- RSSI Smooth - Received Signal Strength Indicator in dBm. Signal fluctuations are filtered to provide a more stable value.
- Timestamp - Nanoseconds since Epoch
- Battery Power - Measured Battery power 

### Binary 
Example: UDP or MQTT

```
 3, 1, 193, 76, 110, 149, 228, 193, 233, 14, 45, 7, 152, 1, -74, -71, 127, 38, 56, 52, '\x14ah'
 ```

>*fields are little-endian

#### RSSI Packet: Packet Type 0x0003
##### Header
Offset  | Length | Type      | Field
--------|--------|-----------|-------
0       | 2      | uint16_t  | Packet Type
2       | 6      | uint8_t[] | Edge MAC 
8       | 6      | uint8_t[] | Device MAC
14      | 1      | uint8_t   | Device Type
15      | 1      | int8_t    | RSSI
16      | 1      | int8_t    | Measured Power
17      | 1      | uint8_t   | Sequnce Number
18      | 1      | uint8_t   | BLE Channel
19      | 1      | uint8_t   | Battery Level
20      | 1      | uint8_t   | Payload Length
21      | 1-255  | uint8_t[] | Payload Data




## BlueCats Edge Supported BLE Ad Types

| Protocol | Type | Id | Payload | Proximity | Measurement | BlueCats Management 
| ---      | ---  | --- | ---     | ---       | ---         | --- 
| Apple    | iBeacon | YES | YES  | YES       | For Id      | - 
| Eddystone| UID     | YES | YES  | YES       | For Id      | - 
|          | URL     | - | YES  | YES       | -           | - 
|          | TLM     | - | YES  | -         | -           | - 
|          | EID     | YES*| YES  | -         | -           | -
|          | eTLM    | - | YES  | -         | -           | -
| BlueCats | iBeacon | YES | YES  | YES       | For Id      | YES
|          | Secure  | YES | YES  | YES       | For Id      | YES
|          | Data    | - | YES  | -         | -           | YES
|          | Newborn | YES | YES  | -         | -           | YES
|          | Unconfigured | - | YES  | -         | -           | YES
|          | Management | * | YES  | -         | -           | YES
|          | eManagement | - | YES  | -         | -           | YES
|          | Identifier | YES | YES  | -         | For Id           | YES
|          | eIdentifier | YES | YES  | -         | For Id           | YES
|          | Measurement | - | YES  | -         | YES         | YES
|          | Data | - | YES  | -         | -         | YES



# Apply Filters

When BLE advertisements are received from each scan they can be filtered using one or more rules.

- Only ads with a minimum received signal strength indicator (RSSI)
- BT Address Pattern/Mask e.g. 0007/FFFF would match any beacon with a public Bluetooth address starting with 0007
- Generic BLE Advertisement Pattern/Mask e.g. 000000FF/00000041 for BC Ads

## Testing Your Endpoints
### Example - UDP

Once the Edge has been configured to send messages over UDP to a port and IP, a simple server application can be set up to listen for the messages.

To receive the data we will need to set up a local UDP server to listen on the port configured for the UDP endpoint. Here is an example in Python:

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
2016-11-28 17:25:51.569708  -UDP-  BCAdData,1,E4956E40DFCF,A0E6F854703A,-63,-63,1480314351666436,02010617FF0401050413012600040F82BD640391F8E602F514C96D0302C4FE
2016-11-28 17:25:51.673411  -UDP-  BCAdData,1,E4956E40DFCF,A0E6F854703A,-51,-63,1480314351770220,02010617FF0401050413012600040F82BD640391F8E602F514C96D0302C4FE
2016-11-28 17:25:51.788579  -UDP-  BCAdData,1,E4956E40DFCF,A0E6F854703A,-51,-63,1480314351885285,02010617FF0401050413012600040F82BD640391F8E602F514C96D0302C4FE
2016-11-28 17:25:51.898095  -UDP-  BCAdData,1,E4956E40DFCF,A0E6F854703A,-54,-63,1480314351994814,02010617FF0401050413012600040F82BD640391F8E602F514C96D0302C4FE
2016-11-28 17:25:51.956482  -UDP-  BCAdData,1,E4956E40DFCF,A0E6F854703A,-57,-63,1480314352053244,02010617FF0401050413012600040F82BD640391F8E602F514C96D0302C4FE
2016-11-28 17:25:52.074793  -UDP-  BCAdData,1,E4956E40DFCF,A0E6F854703A,-62,-63,1480314352171590,02010614FF04010997BD34DB5E40A7346BC6F681F1D3D50A
```

#### Using 3rd party UDP servers to view BLE traffic

If you don't have a coding background, then a free UDP server application like [PacketSender](https://packetsender.com/) will show you all the data being sent from the Edge Relay to your computer *(remember to set the IP address and port in [end point configuration](#udp))*.

### Example - MQTT
Once the Edge has been configured to send messages over MQTT to a port and IP, a simple server application can be set up to listen for the messages.

To receive the data we will need to set up a local HTTP server to listen on the port configured for the UDP endpoint. Here is an example in a third party Broker MQTT client called [HiveMQ](http://www.hivemq.com/demos/websocket-client/)

1.We set the Host to the IP address 192.168.8.1 
2. Set the Port one above our port (i.e. our Port is 1883 we put 1884). 
3. Click Connect. 
4. Click Subscribe
5. Add "edge/#"
6. You should see messages coming in

### Example - HTTP 
Once the Edge has been configured to send messages over HTTP to a port and IP, a simple server application can be set up to listen for the messages.

To receive the data we will need to set up a local HTTP server to listen on the port configured for the UDP endpoint. Here is an example in Python:

```
import SimpleHTTPServer
import SocketServer
import logging
import cgi
import json
import time

PORT = 8010

class ServerHandler(SimpleHTTPServer.SimpleHTTPRequestHandler):

    def do_GET(self):
        logging.error(self.headers)
        SimpleHTTPServer.SimpleHTTPRequestHandler.do_GET(self)

    def do_POST(self):
        logging.error(self.headers)
        content_len = int(self.headers.getheader('content-length', 0))
        post_body = self.rfile.read(content_len)
        print "post_body(%s)" % (post_body)
        SimpleHTTPServer.SimpleHTTPRequestHandler.do_GET(self)

Handler = ServerHandler

httpd = SocketServer.TCPServer(("", PORT), Handler)

print "serving at port", PORT
httpd.serve_forever()
```
