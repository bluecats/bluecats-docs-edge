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

## Messgae Format 4: BlueCats Beacon Health Monitoring

# Endpoint Configuration



# Asset Tracking and Monitoring: BlueCats Locate + BlueCats Sense

## BlueCats Locate

### Primary Use Case

- Locate a smartphone relative to BLE zone (edge or fixed beacon)
- Locate an asset relative to a BLE zone (edge or smartphone + nearest fixed beacon)

### Outputs
- Real-time event stream for entry, exit of zone/s
- Point-in-time (historical) visualisation of smartphone/asset on floorplan or zone map
- Historical data of zone entry/exit

### Components
- Configuration Platform (Web Manager, Reveal, API)
- BlueCats Locate Edge App
- BlueCats Locate SDK (either configuration of existing SDK or extension)
- BlueCats Locate Data Stream (Default AWS Pipepline - web hooks, data download, dashboard)

## BlueCats Sense

### Primary Use Case
- Report regular readings of sensor measurements broacast by a beacon
- Report threshold events when a sensor passes basic rules (outside normal range + window)

### Components
Configuration Platform (Web Manager, Reveal, API)
BlueCats Sense Edge App
BlueCats Sense SDK (either configuration of existing SDK or extension)
BlueCats Sense Data Stream (Default AWS Pipepline - web hooks, data download, dashboard)

### Outputs
Real-time data stream of measurement values
Real-time data stream of threshold events
Dashboard of point-in-time (historical) values
Historical data of zone entry/exit

# Further Details
## BlueCats Locate

- Mobile SDKs to detect and report on assets (GPS + nearby fixed beacon)
- Edge beacons to create proximity zones in key fixed-point locations
- Fixed location beacons to mark additional location checkpoints (table number, areas outside of Edge range)
- Mobile beacons (assets) attached to people or high-value assets

The BlueCats Edge will report on detected beacons, reporting Entry and / or exit and proximity

1. Reporting interval e.g. every 1 hour
2. Signal smoothing algorithm (maybe basic option increase / decrease smoothing)
3. Proximity events to report (entry, exit or dwell)
4. Proximity 'inside' thresholds (accuracy | RSSI | dwell time)

```
<appid>/<edgelocation>/<adtype>/<id>/proximity
{
	"rssi":-67,
	"accuracy":2.43,
	"mPow":-65
}

<appid>/<edgelocation>/<adtype>/<id>/proximity/enter
{
	"rssi":-67,
	"accuracy":2.43,
	"mPow":-65
}

<appid>/<edgelocation>/<adtype>/<id>/proximity/dwell
{
	"rssi":-67,
	"accuracy":2.43,
	"mPow":-65,
	"duration":60,
	"cumulativeDuration":180
}

<appid>/<edgelocation>/<adtype>/<id>/proximity/exit
{
	"rssi":-67,
	"accuracy":2.43,
	"mPow":-65,
	"duration":300
}

<appid>/<deviceid>/<adtype>/<id>/proximity
{
	"rssi":-67,
	"accuracy":2.43,
	"mPow":-65
}

<appid>/<deviceid>/<adtype>/<id>/proximity/enter
{
	"rssi":-67,
	"accuracy":2.43,
	"mPow":-65
}

<appid>/<deviceid>/<adtype>/<id>/proximity/exit
{
	"rssi":-67,
	"accuracy":2.43,
	"mPow":-65,
	"duration":300
}
```

## BlueCats Sense
### Use cases
1. Periodic status update of a sensor value from a beacon e.g. graph fridge temperature with hourly updates
2. Immediate reporting of sensor data outside of threshold range e.g. send an alert when fridge temperature above 4deg C for more than 1 minute

The BlueCats Edge will periodically report sensor values detected from beacons. Default configuration options include:

1. Regular (heartbeat) reporting interval e.g. every 1 hour
2. Use latest value or data aggregation such as Min, Max or Mean and window e.g. 1 minute
3. Measurement type/s (specify types to include/ignore)
4. Measurement threshold
5. Beacon ID type/s to report e.g. report with BC SecureID, EddyUID etc as beacon id

### Measurement Fields

- AppID (edge or SDK). AppID is authorised by API credentials and partitions data
- DeviceID (Edge or Smartphone)
- Measurement Name, Type, Data, Unit

Configuration

```
<appid>/location123/beaconadkey/measurements
{
	"measurements" : [
	  {
	  	"name" : "temp"
		"type" : 1,
	  	"data" : 28.4943
	  	"unit" : "C"
	  },
	  {
		"name" : "aX",
		"type" : 2,
	  	"data" : 0.5,
	  	"unit" : "G"
	  },
	  {
		"name" : "aY",
		"type" : 3,
	  	"data" : 0.5,
	  	"unit" : "G"
	  },
	  {
		"name" : "aZ",
		"type" : 4,
	  	"data" : 0.5,
	  	"unit" : "G"
	  }
	]
}

<appid>/<edgelocation>/<idtype>/<id>/measurements/temp
{
	"type" : 67,
	"data" : 28.4943,
	"unit" : "C"
}

<appid>/<edgelocation>/<idtype>/<id>/measurements/temp/threshold
{
	"type" : 67,
	"data" : 42.78,
	"unit" : "C"
}
```

## Supported Ad Types

- BlueCats Secure ID (BlueCats Secure v2)
- Eddystone UID
- iBeacon
- Measurement (temp, accel. etc.)

## Platform Features
### BlueCats Cloud + Management Apps

- Configure beacons


