
# Edge Configuration

BlueCats
 - Live View (beacon scan table)
 - Applications
 	- Global BLE scanning rules
 		- RSSI threshold
 		- Advanced
 			- BLE Mac Address Pattern
 			- Advertisement Data Pattern
 	- BLE Packet scanning
 		- Enabled/disabled
 		- Endpoint (MQTT/UDP)
 		- Format (CSV/JSON)
 		- Message throttle
 	- Proximity scanning
 	- Sensor Measurement
 	- BlueCats Beacon Management
 - Endpoints
 	- MQTT
 		- Certificates
 	- UDP
 - Advanced
 	- BLE Configuration
 		- Scan settings
 		- FW updates

# Beacon Configuration

## BlueCats Supported Ad Types

### Apple	

- iBeacon

### Eddystone	

- UID
- URL
- TLM
- EID
- eTLM

### BlueCats	

- iBeacon (iBeacon identifier + BlueCats management)
- Secure (Obscure)
- Data (Deprecated)
- Newborn (Deprecated)
- Unconfigured
- Management
- eManagement
- Identifier
- eIdentifier
- Measurement (Temperature, Accelerometer, Tilt)
- Data

## Scanning BLE Advertisments

### Device Identification

A scanned may be identified by either its manufacturer assigned MAC address (private Bluetooth Address) or a custom identifier included in the advertisement payload. This identifier may be encrypted (BlueCats Verified) or a plain-text/static component of the payload.

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

# Default Edge Scan Filters

When BLE advertisements are received from each scan they can be filtered using one or more rules.

- BT Address Pattern/Mask e.g. 0007/FFFF would match any beacon with a public Bluetooth address starting with 0007
- Generic Payload Pattern/Mask e.g. 000000FF/00000041 for BC Ads
- Include any BlueCats Ad matching BC Team ID
- Include any BlueCats Ad
- Include only selected Ad Type/s
- Only ads with a minimum received signal strength indicator (RSSI)

# Default Edge Message Formats

## Message Format 1: Generic device detection with Signal Strength and Static BT Address

Any BLE packet matching packet filters will be forwarded with the following data:

- EdgeMAC - Hardware MAC address of the Edge
- BeaconMAC - Scanned (public) BT Address of the detected BLE device
- RSSI - Received Signal Strength Indicator in dBm
- Timestamp - Nanoseconds since Epoch
- Payload - Full advertisement data

| Field            | Example
| ---              | ---
| Edge MAC Address | E4956E40DFCF
| BLE MAC Address  | A0E6F854703A
| RSSI             | -53
| Timestamp        | 1480314352373689
| BLE Payload      | 0201061AFF4C00021561687109905F443691F8E602F514C96D00040F82BD

Example: CSV (for UDP)

e.g. 
```
E4956E40DFCF,A0E6F854703A,-63,1480314351666436,02010617FF0401050413012600040F82BD640391F8E602F514C96D0302C4FE
```
Example: Serialised C Struct (for UDP)

Example: JSON (for MQTT,UDP)
```
{
    EdgeMAC:"E4956E40DFCF",
    BeaconMAC:"A0E6F854703A",
    RSSI:-63,
    Timestamp:1480314351666436
    Payload:"02010617FF0401050413012600040F82BD640391F8E602F514C96D0302C4FE"
}
```

## Message Format 2: Device proximity detection with custom identifier and distance estimation

BLE packet matching packet filters will be forwarded with the following data:

- EdgeMAC - Hardware MAC address of the Edge
- EdgeName - User defined description for the Edge
- BeaconMAC - Scanned (public) BT Address of the detected BLE device, or MAC in payload if available
- BeaconIdentifier - iBeacon key, UID, BC Identifier
- RSSI - Received Signal Strength Indicator in dBm
- MPow - Measured power at 1 metre
- Accuracy - Calculated accuracy based on RSSI and measured power
- Timestamp - Nanoseconds since Epoch

Example: CSV (for UDP)

```
E4956E40DFCF,Level 6 North,A0E6F854703A,61687109905F443691F8E602F514C96D00040F82,-63,-63,1.00,1480314351666436
```

Example: JSON (for MQTT,UDP)
```
{
    EdgeMAC:"E4956E40DFCF",
    EdgeName:"Level 6 North",
    BeaconMAC:"A0E6F854703A",
    BeaconIdentifier:"61687109905F443691F8E602F514C96D00040F82",
    RSSI:-63,
    MPow:-63,
    Accuracy:1.00
    Timestamp:1480314351666436
}
```

## Message Format 3: Measurement Data Collect and Forward

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
E4956E40DFCF,Level-6-North,A0E6F854703A,61687109905F443691F8E602F514C96D00040F82,-63,05ABFF,1480314351666436
```

Example: JSON (for MQTT,UDP)
```
{
    EdgeMAC:"E4956E40DFCF",
    EdgeName:"Level 6 North",
    BeaconMAC:"A0E6F854703A",
    BeaconIdentifier:"61687109905F443691F8E602F514C96D00040F82",
    RSSI:-63,
    MeasurementData:[{
    	Type : "01",
	Data : [45.6534535]
    }],
    Timestamp:1480314351666436
}
```

## Messgae Format 4: BlueCats Beacon Health Monitoring

BLE packet matching packet filters will be forwarded with the following data:

- EdgeMAC - Hardware MAC address of the Edge
- EdgeName - User defined description for the Edge
- BeaconMAC - Scanned (public) BT Address of the detected BLE device, or MAC in payload if available
- BeaconIdentifier - iBeacon key, UID, BC Identifier
- RSSI - Received Signal Strength Indicator in dBm
- FirmwareIdentifier - unique identifier for the firmware currently installed on the beacon
- SettingsVersion - incrementing version number of beacon configuration changes (0-255)
- Timestamp - Nanoseconds since Epoch

Example: CSV (for UDP)

```
E4956E40DFCF,Level-6-North,A0E6F854703A,61687109905F443691F8E602F514C96D00040F82,-63,500000A1,13,1480314351666436
```

Example: JSON (for MQTT,UDP)
```
{
    EdgeMAC:"E4956E40DFCF",
    EdgeName:"Level 6 North",
    BeaconMAC:"A0E6F854703A",
    BeaconIdentifier:"61687109905F443691F8E602F514C96D00040F82",
    RSSI:-63,
    FirmwareIdentifier: "500000A1",
    SettingsVersion:13,
    Timestamp:1480314351666436
}
```
