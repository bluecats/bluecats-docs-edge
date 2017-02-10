# BlueCats Edge Applications - Overview

- [BlueCats Edge Applications](#bluecats-edge-applications---overview)
- [Live View](#live-view)
- [Configure Endpoints](#configure-endpoints)
- [Configure Applications](#configure-applications)
    - [Scan and Forward](#application---scan-and-forward)
    - [Proximity](#application---proximity)
    - [Sensor Measurement](#application---sensor-measurement)
    - [BlueCats Beacon Health](#application---bluecats-beacon-health)
- [Example UDP Server](https://github.com/bluecats/bluecats-docs-edge/blob/master/getting-started-edge-applications.md#example---receiving-data-with-a-simple-udp-server)

BlueCats Edge provides default applications for common BLE scanning use cases. This ranges from simple scan and forward of any BLE advertisments in range of the Edge to detection, filtering and parsing of advertisements ready to apply to proximity, sensor measurement or beacon network monitoring solutions. With the Edge you can:

- Configure network settings for connectivity to local network and the internet. The BlueCats Edge is a Wireless Access Point providing advanced network configuration options if required.
- View beacons in range of the Edge with Live View
- Configure global BLE scan filters. This provides a configurable minimum signal strength threshold for beacon detection or to match a specific Bluetooth address or BLE advertisement format - useful for reducing noise during testing
- Configure default applications. Detect and forward BLE advertisements for these common use cases:
	- Raw BLE advertisement payload scan and forward
	- Scan for beacon proximity for asset or personnel tracking
	- Scan for BlueCats sensor measurement data
	- Scan for BlueCats beacon health data including battery level and configuration status

For each application the endpoint type (UDP or MQTT), message format (CSV or JSON) and message throttle limits for can be independently configured or the application can be disabled if not required.

## Edge Configuration

Once you have [connected to the Edge and configured network settings](getting-started-connect.md) you can manage bluetooth scanning applications on the Edge.

### Live View

Live view provides a simple list of BLE devices detected in range of the Edge along with their current RSSI (signal strength).

<p style="text-align:center" align="center"><img style="max-width: 400px;" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-features/LiveView.png" alt="Live View"/></p>

### Configure Endpoints

To make use of the data collected by the Edge, you will need to send it somewhere. The default Edge applications support two types of endpoint - UDP or MQTT.

<p style="text-align:center" align="center"><img align="center" style="max-width: 400px;" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-features/EndpointConfiguration-Menu.png" alt="Endpoints"/></p>

Configure one or both of the endpoint options. Currently the Edge only supports a single endpoint for each type. Data will only be sent the configured endpoint when an Edge application has been enabled and that endpoint type selected.

### UDP
To configure UDP simply enter the PORT and IP where the data should be sent.

<p align="center"><img width="600px" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-features/EndpointConfiguration.png" alt="Endpoints"/></p>

A simple MQTT endpoint without TLS enabled just requires the host/IP and port of the MQTT server and the 'TLS Enabled' set to `0`. Some MQTT endpoints such as AWS IoT require TLS for connections. Here you will need to upload the Certificate, Private Key and Certificate Authority [generated in the AWS console](http://docs.aws.amazon.com/iot/latest/developerguide/create-device-certificate.html).

<p style="text-align:center" align="center"><img align="center" style="max-width: 400px;" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-features/EndpointConfiguration-Certificates.png" alt="MQTT Certificates"/></p>

## Configure Applications

The BlueCats Edge provides four default applications plus some global BLE device filter rules. One or more of these can be enabled and configured individually.

<p align="center"><img width="600px" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-features/App-Menu.png" alt="Applications"/></p>

### Application - Scan and Forward

<p align="center"><img width="600px" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-features/App-ScanAndForward.png" alt="Scan and Forward"/></p>

Any BLE packet matching packet filters will be forwarded with the following data:

- EdgeMAC - Hardware MAC address of the Edge
- BeaconMAC - Scanned (public) BT Address of the detected BLE device
- RSSI - Received Signal Strength Indicator in dBm
- RSSI Smooth - Received Signal Strength Indicator in dBm. Signal fluctuations are filtered to provide a more stable value.
- Timestamp - Nanoseconds since Epoch
- AdData - Full advertisement data payload

| Field            | Example
| ---              | ---
| Message Type     | BCAdData (CSV only)
| Message Version  | 1 (CSV only to track format changes)
| Edge MAC Address | E4956E40DFCF
| BLE MAC Address  | A0E6F854703A
| RSSI             | -63
| RSSI Smooth      | -62
| Timestamp        | 1480314352373689
| AdData           | 0201061AFF4C00021561687109905F443691F8E602F514C96D00040F82BD

Example: CSV (for UDP)

e.g. 
```
BCAdData,1,E4956E40DFCF,A0E6F854703A,-63,-62,1480314351666436,02010617FF0401050413012600040F82BD640391F8E602F514C96D0302C4FE
```
Example: Serialised C Struct (for UDP)

Example: JSON (for MQTT,UDP)
```
{
    edgeMAC:"E4956E40DFCF",
    beaconMAC:"A0E6F854703A",
    rssi:-63,
    rssiSmooth:-62,
    timestamp:1480314351666436
    adData:"02010617FF0401050413012600040F82BD640391F8E602F514C96D0302C4FE"
}
```

### Application - Proximity
<p align="center"><img width="600px" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-features/App-Proximity.png" alt="Applications - Proximity"/></p>

BLE packet matching packet filters will be forwarded with the following data:

- Message Type (CSV only) 
- Message Version  | 1 (CSV only to track format changes)
- EdgeMAC - Hardware MAC address of the Edge
- EdgeName - User defined description for the Edge
- BeaconMAC - Scanned (public) BT Address of the detected BLE device, or MAC in payload if available
- BeaconIdentifier - iBeacon key, UID, BC Identifier, Private MAC Address
- RSSI - Received Signal Strength Indicator in dBm
- RSSI Smooth - Received Signal Strength Indicator in dBm. Signal fluctuations are filtered to provide a more stable value.
- MPow - Measured power at 1 metre
- Accuracy - Calculated accuracy (estimated distance in metres) based on smooth RSSI and measured power
- Timestamp - Nanoseconds since Epoch

Example: CSV (for UDP)

```
BCProximity,1, E4956E40DFCF,Level 6 North,A0E6F854703A,61687109905F443691F8E602F514C96D00040F82,-63,-62,-62,1.00,1480314351666436
```

Example: JSON (for MQTT,UDP)
```
{
    edgeMAC:"E4956E40DFCF",
    edgeName:"Level 6 North",
    beaconMAC:"A0E6F854703A",
    beaconIdentifiers:[{
    	type: "iBeacon",
	proximityUUID: "61687109905F443691F8E602F514C96D",
	major: 4,
	minor: 1243 },{
    	type: "eddystoneUID",
	namespace: "61687109905F443691F8E602F514C96D",
	instanceID: 4,
	minor: 1243 }],
    rssi:-63,
    rssiSmooth:-62,
    mPow:-62,
    accuracy:1.00
    timestamp:1480314351666436
}
```

### Application - Sensor Measurement

BLE packet matching packet filters will be forwarded with the following data:

- Message Type (CSV only) 
- Message Version  | 1 (CSV only to track format changes)
- EdgeMAC - Hardware MAC address of the Edge
- EdgeName - User defined description for the Edge
- BeaconMAC - Scanned (public) BT Address of the detected BLE device, or MAC in payload if available
- BeaconIdentifier - iBeacon key, UID, BC Identifier
- RSSI - Received Signal Strength Indicator in dBm
- RSSI Smooth - Received Signal Strength Indicator in dBm. Signal fluctuations are filtered to provide a more stable value.
- MeasurementData - sensor measurement received from the beacon
- Timestamp - Nanoseconds since Epoch

Example: CSV (for UDP)

```
BCMeasurement,1,E4956E40DFCF,Level-6-North,A0E6F854703A,61687109905F443691F8E602F514C96D00040F82,-63,-63,0x07,[22.50]:[0.050:0.050:0.050]:[270.00:90.00],1480314351666436
```

Example: JSON (for MQTT,UDP)
```
{
    edgeMAC:"E4956E40DFCF",
    edgeName:"Level 6 North",
    beaconMAC:"A0E6F854703A",
    beaconIdentifiers:[{
    	type: "iBeacon",
	proximityUUID: "61687109905F443691F8E602F514C96D",
	major: 4,
	minor: 1243 }],
    rssi:-63,
    rssiSmooth:-63,
    measurements:[{
    	type : 1,
	data : [22.50]
    },{
    	type : 2,
	data : [0.050,0.050,0.050]
    },{
        type : 4,
	data : [270.00, 90.00]

    }],
    timestamp:1480314351666436
}
```

### Application - BlueCats Beacon Health

BLE packet matching packet filters will be forwarded with the following data:

- EdgeMAC - Hardware MAC address of the Edge
- EdgeName - User defined description for the Edge
- BeaconMAC - Scanned (public) BT Address of the detected BLE device, or MAC in payload if available
- BeaconIdentifier - iBeacon key, UID, BC Identifier
- RSSI - Received Signal Strength Indicator in dBm
- BatteryLevel - Current battery level %
- FirmwareIdentifier - unique identifier for the firmware currently installed on the beacon
- SettingsVersion - incrementing version number of beacon configuration changes (0-255)
- Timestamp - Nanoseconds since Epoch

Example: CSV (for UDP)

```
BCManagement, E4956E40DFCF,Level-6-North,A0E6F854703A,61687109905F443691F8E602F514C96D00040F82,-63,87,500000A1,13,1480314351666436
```

Example: JSON (for MQTT,UDP)
```
{
    edgeMAC:"E4956E40DFCF",
    edgeName:"Level 6 North",
    beaconMAC:"A0E6F854703A",
    beaconIdentifiers:[{
    	type: "iBeacon",
	proximityUUID: "61687109905F443691F8E602F514C96D",
	major: 4,
	minor: 1243 }],
    rssi:-63,
    rssiSmooth:-63,
    batteryLevel: 87,
    firmwareIdentifier: "500000A1",
    settingsVersion:13,
    timestamp:1480314351666436
}
```

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


## Using Beacon Detection Data

### Beacon Identification

A scanned beacon may be identified by either its manufacturer assigned MAC address (private Bluetooth Address) or a custom identifier included in the advertisement payload. This identifier may be encrypted (BlueCats Verified) or a plain-text/static component of the payload.

- MAC - BT Address for any beacon where BT address privacy is disabled
- MAC - Included in payload in BC Ads including Secure (obfuscated), Newborn, Unconfigured, Management, eManagement (encrypted)
- Custom Identifer (Included in payload by iBeacon, Eddy-UID, BC-Identifier, BC-eIdentifier)

MAC or Custom Identifier included in the payload may be either encrypted or plain/static

### Proximity / Location Data

- RSSI (Received Signal Strength Indicator)
- MeasuredPower / Tx Power
- Accuracy (calculated from RSSI and MPower)

### Sensor Measurement Data

- Temperature
- Accelerometer (real-time sampling)
- Accelerometer (motion event)
- Tilt (Degrees from vertical)

### BC Management

- Remaining battery
- Settings version
- Firmware Identifier

# Apply Global BLE Scan Filters

When BLE advertisements are received from each scan they can be filtered using one or more rules.

- Only ads with a minimum received signal strength indicator (RSSI)
- BT Address Pattern/Mask e.g. 0007/FFFF would match any beacon with a public Bluetooth address starting with 0007
- Generic BLE Advertisement Pattern/Mask e.g. 000000FF/00000041 for BC Ads

## Example - Receiving data with a simple UDP server

Once the Edge has been configured to send messages over UDP to a port and IP, a simple server applicaiton can be set up to listen for the messages.

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

## Using 3rd party UDP servers to view BLE traffic

If you don't have a coding background, then a free UDP server application like [PacketSender](https://packetsender.com/) will show you all the data being sent from the Edge Relay to your computer *(remember to set the IP address and port in [end point configuration](#udp))*.
