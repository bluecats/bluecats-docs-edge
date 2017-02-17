# Send Proximity Data to a MQTT Broker
In this sample scenario, the edge relay sends proximity data to a mqtt broker (i.e. Mosquitto, HiveMQ, RabbitMQ etc.) and the node-red flow connects to the broker and visualise the data in a dashboard. 

## Edge Relay Configuration

- [Configure MQTT Endpoint](https://github.com/bluecats/bluecats-docs-edge/blob/master/getting-started-edge-applications.md#configure-endpoints) in Edge Relay pointed to the MQTT Broker. Enter Host/IP and port of the mqtt broker. 

<p align="center"><img width="400px" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-samples/edge-endpoint-mqtt.png" alt="Endpoint MQTT"/></p>

- Configure the Proximity tab to send data in JSON format to the MQTT endpoint.

<p align="center"><img width="400px" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-samples/edge-mqtt-proximity.png" alt="proximity MQTT"/></p>

## Node-red Configuration

 - Install additional nodes
	 - Install [node-red-dashboard](https://flows.nodered.org/node/node-red-dashboard) if it is not installed before.

 - Create the node-red flow
 
 Import the following flow into node-red. 

```
[{"id":"bc67f65f.eb515","type":"mqtt in","z":"5fc9b0ef.72bed8","name":"","topic":"edge/proximity/#","qos":"0","broker":"cea40d3a.39a2a","x":112,"y":225,"wires":[["a8198562.a04748"]]},{"id":"a8198562.a04748","type":"json","z":"5fc9b0ef.72bed8","name":"parseJson","x":234,"y":132,"wires":[["49ce8117.0afdd8","b481072c.83f188"]]},{"id":"49ce8117.0afdd8","type":"function","z":"5fc9b0ef.72bed8","name":"getProximityData","func":"var mPow = {payload:msg.payload.mPow};\nvar RSSI = {payload:msg.payload.rssi};\nvar Accuracy = {payload:msg.payload.accuracy};\nvar BeaconMAC = {payload:msg.payload.beaconMac};\nreturn [RSSI,mPow,Accuracy,BeaconMAC];","outputs":"4","noerr":0,"x":445,"y":131,"wires":[["241b07ef.6a892"],["683d531f.2b52fc"],["5cd94f68.3dce58"],["7a1cb2a7.98df3c"]]},{"id":"241b07ef.6a892","type":"ui_gauge","z":"5fc9b0ef.72bed8","name":"RSSI","group":"1e9d8c5a.6cc944","order":0,"width":"4","height":"4","gtype":"gage","title":"RSSI","label":"","format":"{{value}}","min":"-99","max":"0","colors":["#CA3838","#E6E600","#00B500"],"x":736,"y":134,"wires":[]},{"id":"683d531f.2b52fc","type":"ui_gauge","z":"5fc9b0ef.72bed8","name":"Power","group":"1e9d8c5a.6cc944","order":0,"width":"4","height":"4","gtype":"gage","title":"Power","label":" ","format":"{{value}}","min":"-100","max":"0","colors":["#CA3838","#E6E600","#00B500"],"x":735,"y":174,"wires":[]},{"id":"5cd94f68.3dce58","type":"ui_text","z":"5fc9b0ef.72bed8","group":"1e9d8c5a.6cc944","order":0,"width":"4","height":"1","name":"","label":"Accuracy : ","format":"{{msg.payload}} m","layout":"row-left","x":758,"y":216,"wires":[]},{"id":"7a1cb2a7.98df3c","type":"ui_text","z":"5fc9b0ef.72bed8","group":"529d298c.60aa08","order":0,"width":"4","height":"1","name":"","label":"BeaconMAC","format":"{{msg.payload}}","layout":"col-center","x":759,"y":256,"wires":[]},{"id":"b481072c.83f188","type":"debug","z":"5fc9b0ef.72bed8","name":"","active":true,"console":"false","complete":"false","x":437,"y":41,"wires":[]},{"id":"cea40d3a.39a2a","type":"mqtt-broker","z":"","broker":"localhost","port":"1883","clientid":"","usetls":false,"compatmode":true,"keepalive":"60","cleansession":true,"willTopic":"","willQos":"0","willRetain":"false","willPayload":"","birthTopic":"","birthQos":"0","birthRetain":"false","birthPayload":""},{"id":"1e9d8c5a.6cc944","type":"ui_group","z":"","name":"Proximity","tab":"a0abcf06.06445","order":2,"disp":true,"width":"6"},{"id":"529d298c.60aa08","type":"ui_group","z":"","name":"BLE Info.","tab":"a0abcf06.06445","order":2,"disp":false,"width":"6"},{"id":"a0abcf06.06445","type":"ui_tab","z":"","name":"Proximity","icon":"dashboard","order":1}]
```

<p align="center"><img width="400px" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-samples/node-red-mqtt-flow.png" alt="UDP flow"/></p>


  - Configure nodes
    - MQTT Node
      - In the Server, select ‘Add new mqtt-broker…’.

      <p align="center"><img width="400px" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-samples/node-red-mqtt-node-add.png" alt="Add MQTT"/></p>

      - Add Server host/ip and port of the mqtt-broker.

      <p align="center"><img width="400px" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-samples/node-red-mqtt-node-new.png" alt="New MQTT"/></p>
      
      - Enter the mqtt topic ‘edge/proximity/#’.
      
      <p align="center"><img width="400px" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-samples/node-red-mqtt-node-config.png" alt="Config MQTT"/></p>
     
    - Function Node
    
      Function node extracts Power, RSSI, Accuracy, BeaconMAC, Major and Minor from the payload and send them to the output nodes. Refer Edge Relay [documentation](getting-started-edge-applications.md#application---proximity) for the data structures.
      
      ```
      var mPow = {payload:msg.payload.mPow};
      var RSSI = {payload:msg.payload.rssi};
      var Accuracy = {payload:msg.payload.accuracy};
      var BeaconMAC = {payload:msg.payload.beaconMac};
      return [RSSI,mPow,Accuracy,BeaconMAC];
      ```
      
## UI Dashboard

UI Dashboard is available at *http://{node-red-ip}:1880/ui*

<p align="center"><img width="600px" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-samples/node-red-ui-proximity.png" alt="UI measurement"/></p>
