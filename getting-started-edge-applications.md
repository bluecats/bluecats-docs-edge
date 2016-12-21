# BlueCats Edge Applications - Overview

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

<img align="center" style="max-width: 400px;" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-features/LiveView.png" alt="Live View"/>

### Configure Endpoints

To make use of the data collected by the Edge, you will need to send it somewhere. The default Edge applications support two types of endpoint - UDP or MQTT.

<img align="center" style="max-width: 400px;" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-features/EndpointConfiguration-Menu.png" alt="Endpoints"/>

Configure one or both of the endpoint options. Currently the Edge only supports a single endpoint for each type. Data will only be sent the configured endpoint when an Edge application has been enabled and that endpoint type selected.

### UDP
To configure UDP simply enter the PORT and IP where the data should be sent.

<img align="center" style="max-width: 400px;" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-features/EndpointConfiguration.png" alt="Endpoints"/>

A simple MQTT endpoint without TLS enabled just requires the host/IP and port of the MQTT server and the 'TLS Enabled' set to `0`. Some MQTT endpoints such as AWS IoT require TLS for connections. Here you will need to upload the Certificate, Private Key and Certificate Authority [generated in the AWS console](http://docs.aws.amazon.com/iot/latest/developerguide/create-device-certificate.html).

<img align="center" style="max-width: 400px;" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-features/EndpointConfiguration-Certificates.png" alt="MQTT Certificates"/>

## Configure Applications

The BlueCats Edge provides four default applications plus some global BLE device filter rules. One or more of these can be enabled and configured individually.

![Applications](https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-features/App-Menu.png "Applications")

### Application - Scan and Forward

![Applications - Scan and Forward](https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-features/App-ScanAndForward.png "Applications - Scan and Forward")

Any BLE packet matching packet filters will be forwarded with the following data:

- EdgeMAC - Hardware MAC address of the Edge
- BeaconMAC - Scanned (public) BT Address of the detected BLE device
- RSSI - Received Signal Strength Indicator in dBm
- Timestamp - Nanoseconds since Epoch
- AdData - Full advertisement data

| Field            | Example
| ---              | ---
| Edge MAC Address | E4956E40DFCF
| BLE MAC Address  | A0E6F854703A
| RSSI             | -53
| Timestamp        | 1480314352373689
| BLE AdData       | 0201061AFF4C00021561687109905F443691F8E602F514C96D00040F82BD

Example: CSV (for UDP)

e.g. 
```
BCAdData, E4956E40DFCF,A0E6F854703A,-63,1480314351666436,02010617FF0401050413012600040F82BD640391F8E602F514C96D0302C4FE
```
Example: Serialised C Struct (for UDP)

Example: JSON (for MQTT,UDP)
```
{
    EdgeMAC:"E4956E40DFCF",
    BeaconMAC:"A0E6F854703A",
    RSSI:-63,
    Timestamp:1480314351666436
    AdData:"02010617FF0401050413012600040F82BD640391F8E602F514C96D0302C4FE"
}
```

### Application - Proximity
![Applications - Proximity](https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-features/App-Proximity.png "Applications - Proximity")

BLE packet matching packet filters will be forwarded with the following data:

- EdgeMAC - Hardware MAC address of the Edge
- EdgeName - User defined description for the Edge
- BeaconMAC - Scanned (public) BT Address of the detected BLE device, or MAC in payload if available
- BeaconIdentifier - iBeacon key, UID, BC Identifier, Private MAC Address
- RSSI - Received Signal Strength Indicator in dBm
- MPow - Measured power at 1 metre
- Accuracy - Calculated accuracy (estimated distance in metres) based on RSSI and measured power
- Timestamp - Nanoseconds since Epoch

Example: CSV (for UDP)

```
BCProximity, E4956E40DFCF,Level 6 North,A0E6F854703A,61687109905F443691F8E602F514C96D00040F82,-63,-63,1.00,1480314351666436
```

Example: JSON (for MQTT,UDP)
```
{
    EdgeMAC:"E4956E40DFCF",
    EdgeName:"Level 6 North",
    BeaconMAC:"A0E6F854703A",
    BeaconIdentifier:{
    	Type: "iBeacon",
	ProximityUUID: "61687109905F443691F8E602F514C96D",
	Major: 4,
	Minor: 1243 },
    RSSI:-63,
    MPow:-63,
    Accuracy:1.00
    Timestamp:1480314351666436
}
```

### Application - Sensor Measurement

BLE packet matching packet filters will be forwarded with the following data:

- EdgeMAC - Hardware MAC address of the Edge
- EdgeName - User defined description for the Edge
- BeaconMAC - Scanned (public) BT Address of the detected BLE device, or MAC in payload if available
- BeaconIdentifier - iBeacon key, UID, BC Identifier
- RSSI - Received Signal Strength Indicator in dBm
- MeasurementData - sensor measurement received from the beacon
- Timestamp - Nanoseconds since Epoch

Example: CSV (for UDP)

```
BCMeasurement, E4956E40DFCF,Level-6-North,A0E6F854703A,61687109905F443691F8E602F514C96D00040F82,-63,01,05ABFF,1480314351666436
```

Example: JSON (for MQTT,UDP)
```
{
    EdgeMAC:"E4956E40DFCF",
    EdgeName:"Level 6 North",
    BeaconMAC:"A0E6F854703A",
    BeaconIdentifier:{
    	Type: "iBeacon",
	ProximityUUID: "61687109905F443691F8E602F514C96D",
	Major: 4,
	Minor: 1243 },
    RSSI:-63,
    Measurement:{
    	Type : "01",
	Data : [0.5,1.0,0.5]
    },
    Timestamp:1480314351666436
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
    EdgeMAC:"E4956E40DFCF",
    EdgeName:"Level 6 North",
    BeaconMAC:"A0E6F854703A",
    BeaconIdentifier:"61687109905F443691F8E602F514C96D00040F82",
    RSSI:-63,
    BatteryLevel: 87,
    FirmwareIdentifier: "500000A1",
    SettingsVersion:13,
    Timestamp:1480314351666436
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

- BT Address Pattern/Mask e.g. 0007/FFFF would match any beacon with a public Bluetooth address starting with 0007
- Generic Payload Pattern/Mask e.g. 000000FF/00000041 for BC Ads
- Include any BlueCats Ad matching BC Team ID
- Include any BlueCats Ad
- Include only selected Ad Type/s
- Only ads with a minimum received signal strength indicator (RSSI)


