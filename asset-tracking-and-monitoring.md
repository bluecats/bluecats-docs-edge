# Beacon Configuration

## All BlueCats Beacon Supported Ad Types

### Apple	

- iBeacon

### Eddystone	

- UID
- URL
- TLM
- EID
- eTLM

### BlueCats	

- iBeacon (iBeacon identifier + BlueCats beacon hea
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

## Scanned Device Information

### Device Identification

A scanned may be identified by either its manufacturer assigned MAC address (private Bluetooth Address) or a custom identifier included in the advertisement payload. This identifier may be encrypted (BlueCats Verified) or a plain-text/static component of the payload.

- MAC - BT Address for any beacon where BT address privacy is disabled
- MAC - Included in payload in BC Ads including Secure (obfuscated), Newborn, Unconfigured, Management, eManagement (encrypted)
- Custom Identifer (Included in payload by iBeacon, Eddy-UID, BC-Identifier, BC-eIdentifier)

MAC or Custom Identifier included in the payload may be either encrypted or plain/static

### Proximity / Location

- RSSI (Received Signal Strength Indicator)
- MeasuredPower / Tx Power

### Sensor	

- Temperature
- Accelerometer (real-time sampling)
- Accelerometer (motion event)

### BC Management

- Remaining battery
- Settings version
- Firmware Identifier



# Common Edge Use Cases (Personas?)



## Use Case 1: Generic device detection with Signal Strength and Static BT Address


## Use Case 2: Device detection with custom identifier and distance estimation

## Use Case 3: Measurement Data Collect and Forward

## Use Case 4: BlueCats Beacon Health Monitoring

# End Point Options

# Throttling (MQTT)




Filtering and Throttling (Global)

- BLE MAC Pattern/Mask e.g. to include only MAC addresses BC000401/0000FFFF
- Advertisement Pattern/Mask e.g. to include only BlueCats Advertisements 00000401/0000FFFF
- RSSI Thresold e.g. -80
- Accuracy Threshold e.g. 3.0 (would rely on measured power, RSSI and estimated distance algorithm running on Edge)
- Throttle Window (seconds) e.g. 5. Will send one message per detected device within the throttle window
- Throttle Device Limit e.g. 10. Will send detection message of up to 10 different devices within the Throttle Interval.
- Advanced: Throttle Grouping algorthm (average, max, min, last, first). Applies to RSSI. In future potentially parsed/calculated values such as accuracy, sensor data

Message Format:

Raw:

Serialized:

CSV:

JSON:


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


