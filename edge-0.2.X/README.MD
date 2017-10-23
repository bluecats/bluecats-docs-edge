# Getting Started with BlueCats Edge

### Connecting

- [Connecting](getting-started-connect.md#connecting-to-the-edge)
- [Logging In](getting-started-connect.md#logging-in)
- [Connecting to WIFI](getting-started-connect.md#connecting-to-internet-via-wifi)
- [Register the device with BlueCats Cloud for monitoring and updates](getting-started-connect.md#register-with-bluecats-cloud)

### Configuring Endpoints

- [BlueCats Edge Endpoints](https://github.com/bluecats/bluecats-docs-edge/blob/master/edge-0.2.X/getting-started-edge-endpoints.md#bluecats-edge-endpoints---overview)
- [Live View](https://github.com/bluecats/bluecats-docs-edge/blob/master/edge-0.2.X/getting-started-edge-endpoints.md#live-view)
- [Configure Endpoints](https://github.com/bluecats/bluecats-docs-edge/blob/master/edge-0.2.X/getting-started-edge-endpoints.md#configure-endpoints)
    - *Endpoint Types*
        - [UDP](https://github.com/bluecats/bluecats-docs-edge/blob/master/edge-0.2.X/getting-started-edge-endpoints.md#udp)
        - [MQTT](https://github.com/bluecats/bluecats-docs-edge/blob/master/edge-0.2.X/getting-started-edge-endpoints.md#mqtt)
        - [HTTP](https://github.com/bluecats/bluecats-docs-edge/blob/master/edge-0.2.X/getting-started-edge-endpoints.md#http)
    - *Data Types*
        - [CSV](https://github.com/bluecats/bluecats-docs-edge/blob/master/edge-0.2.X/getting-started-edge-endpoints.md#csv)
        - [JSON](https://github.com/bluecats/bluecats-docs-edge/blob/master/edge-0.2.X/getting-started-edge-endpoints.md#json)
        - [Binary](https://github.com/bluecats/bluecats-docs-edge/blob/master/edge-0.2.X/getting-started-edge-endpoints.md#binary) 
- [Example UDP Server](getting-started-edge-applications.md#example---receiving-data-with-a-simple-udp-server)
- [Example MQTT](https://github.com/bluecats/bluecats-docs-edge/blob/master/edge-0.2.X/getting-started-edge-endpoints.md#example---mqtt)
- [Example HTTP](https://github.com/bluecats/bluecats-docs-edge/blob/master/edge-0.2.X/getting-started-edge-endpoints.md#example---http)

### Updating Firmware and Packages
- [Flashing  New Firmware](edge-update-firmware.md#flashing-new-firmware)
- [Update Bluetooth Firmware](edge-update-bluetooth-firmware.md)
- [Update BlueCats Packages](edge-update-firmware.md#updating-bluecats-edge-packages)

### Troubleshooting

- [View Logs](edge-troubleshooting.md#viewing-local-logs)
- [Reset Edge to Default Firmware](edge-troubleshooting.md#resetting-edge-to-default-firmware-when-locked-out)

# Sample Integrations using Node-Red or AWS IoT

#### - Getting started with Node-red
- [About Node-red](https://github.com/bluecats/bluecats-docs-edge/blob/master/samples/node-red-gettingstarted.md#about)
- [Installation](https://github.com/bluecats/bluecats-docs-edge/blob/master/samples/node-red-gettingstarted.md#installation)
- [Runnings](https://github.com/bluecats/bluecats-docs-edge/blob/master/samples/node-red-gettingstarted.md#running)
- [Adding nodes](https://github.com/bluecats/bluecats-docs-edge/blob/master/samples/node-red-gettingstarted.md#adding-nodes)
- [Creating Node-red flows](https://github.com/bluecats/bluecats-docs-edge/blob/master/samples/node-red-gettingstarted.md#creating-node-red-flows)

#### - Send measurement data over UDP
- [Edge Relay Configuration](https://github.com/bluecats/bluecats-docs-edge/blob/master/samples/node-red-measurements-over-udp.md#edge-relay-configuration)
- [Node-red Configuration](https://github.com/bluecats/bluecats-docs-edge/blob/master/samples/node-red-measurements-over-udp.md#node-red-configuration)
- [UI Dashboard](https://github.com/bluecats/bluecats-docs-edge/blob/master/samples/node-red-measurements-over-udp.md#ui-dashboard)

#### - Send Proximity Data to a MQTT Broker
- [Edge Relay Configuration](https://github.com/bluecats/bluecats-docs-edge/blob/master/samples/node-red-proximity-over-mqtt.md#edge-relay-configuration)
- [Node-red Configuration](https://github.com/bluecats/bluecats-docs-edge/blob/master/samples/node-red-proximity-over-mqtt.md#node-red-configuration)
- [UI Dashboard](https://github.com/bluecats/bluecats-docs-edge/blob/master/samples/node-red-proximity-over-mqtt.md#ui-dashboard)

#### - Send Measurement Data to AWS IoT
- [Getting Started with AWS IoT](https://github.com/bluecats/bluecats-docs-edge/blob/master/samples/node-red-measurements-aws-iot.md#getting-started-with-aws-iot)
- [Edge Relay Configuration](https://github.com/bluecats/bluecats-docs-edge/blob/master/samples/node-red-measurements-aws-iot.md#edge-relay-configuration)
- [Node-red configuration](https://github.com/bluecats/bluecats-docs-edge/blob/master/samples/node-red-measurements-aws-iot.md#node-red-configuration)
- [UI Dashboard](https://github.com/bluecats/bluecats-docs-edge/blob/master/samples/node-red-measurements-aws-iot.md#ui-dashboard)
