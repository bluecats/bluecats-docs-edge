# Send data over UDP
In this sample scenario, the edge relay sends measurement data over UDP to a node-red machine and visualise the data in a dashboard. 

## Edge Relay Configuration

 - [Configure UDP Endpoints](https://github.com/bluecats/bluecats-docs-edge/blob/master/getting-started-edge-applications.md#configure-endpoints) in Edge Relay pointed to node-red machine. The IP Address has to be the ip of node-red machine. 
 
 <p align="center"><img width="400px" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-samples/edge-endpoint-udp-config.png" alt="Endpoint UDP"/></p>
 
 - Configure the Sensor Measurement tab to send measurement data in JSON format over UDP to the node-red machine. 

##Node-red Configuration

 - Install additional nodes
	 - [node-red-dashboard](https://flows.nodered.org/node/node-red-dashboard)
	 
	 The UI interface is available at *http://{node-red-ip}:1880/ui*
 
 - Create the node-red flow
 
 Import the following flow into node-red. 
 
 ```
 [{"id":"4e5cae96.51313","type":"udp in","z":"1b4c871f.75c709","name":"","iface":"","port":"9942","ipv":"udp4","multicast":"false","group":"","datatype":"utf8","x":132,"y":60,"wires":[["5087a883.f08f38"]]},{"id":"5087a883.f08f38","type":"json","z":"1b4c871f.75c709","name":"parseJson","x":233,"y":142,"wires":[["7cd20af0.f62754","f6fc3656.3047e8"]]},{"id":"7cd20af0.f62754","type":"debug","z":"1b4c871f.75c709","name":"","active":false,"console":"false","complete":"payload","x":413,"y":44,"wires":[]},{"id":"6db443d8.5bb9b4","type":"ui_gauge","z":"1b4c871f.75c709","name":"Tilt","group":"2691acbb.e86cdc","order":0,"width":0,"height":0,"gtype":"compass","title":"Tilt","label":"Tilt","format":"{{value}}°","min":0,"max":"360","colors":["#00B500","#E6E600","#CA3838"],"x":668,"y":136,"wires":[]},{"id":"2a6ea909.a4469e","type":"ui_chart","z":"1b4c871f.75c709","name":"Timeline","group":"2691acbb.e86cdc","order":0,"width":0,"height":0,"label":"Timeline (Last hour)","chartType":"line","legend":"false","xformat":"HH:mm","interpolate":"linear","nodata":"","ymin":"","ymax":"","removeOlder":1,"removeOlderPoints":"","removeOlderUnit":"3600","cutout":"","x":678,"y":182,"wires":[[],[]]},{"id":"f6fc3656.3047e8","type":"function","z":"1b4c871f.75c709","name":"getMeasurementData","func":"var Tilt = {payload:msg.payload.Measurement[0].Data[0].toFixed(1)};\nvar RSSI = {payload:msg.payload.RSSI};\nvar BeaconMAC = {payload:msg.payload.BeaconMAC};\nreturn [Tilt,RSSI,BeaconMAC];","outputs":"3","noerr":0,"x":451,"y":138,"wires":[["6db443d8.5bb9b4","2a6ea909.a4469e"],["605e2e31.57fd18"],["51df72d7.a1a70c"]]},{"id":"605e2e31.57fd18","type":"ui_gauge","z":"1b4c871f.75c709","name":"RSSI","group":"cfc5ea1d.e3279","order":0,"width":"3","height":"3","gtype":"gage","title":"RSSI","label":"","format":"{{value}}","min":"-100","max":"0","colors":["#CA3838","#E6E600","#00B500"],"x":670,"y":230,"wires":[]},{"id":"51df72d7.a1a70c","type":"ui_text","z":"1b4c871f.75c709","group":"cfc5ea1d.e3279","order":0,"width":"4","height":"2","name":"","label":"BeaconMAC","format":"{{msg.payload}}","layout":"col-center","x":690,"y":279,"wires":[]},{"id":"2691acbb.e86cdc","type":"ui_group","z":"","name":"From UDP","tab":"9be44e64.8405e","order":1,"disp":false,"width":"8"},{"id":"cfc5ea1d.e3279","type":"ui_group","z":"","name":"BLE Info","tab":"9be44e64.8405e","order":2,"disp":false,"width":"6"},{"id":"9be44e64.8405e","type":"ui_tab","z":"","name":"Measurement","icon":"dashboard","order":2}]
 ```
 
<p align="center"><img width="400px" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-samples/node-red-udp-flow.png" alt="UDP flow"/></p>
 
 
- Configure nodes
	- UDP Node
	
		Configure the udp node to listen to udp messages on the designated port and output as a string.
		
		 <p align="center"><img width="400px" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-samples/node-red-udp-node-config.png" alt="UDP node config"/></p>
		
	- Function Node
	
		Function node extracts Tilt, RSSI and BeaconMAC from the payload and send them to the output nodes. Refer Edge Relay documentation for the data structures.

		```
		var Tilt = {payload:msg.payload.Measurement[0].Data[0].toFixed(1)};
		var RSSI = {payload:msg.payload.RSSI};
		var BeaconMAC = {payload:msg.payload.BeaconMAC};
		return [Tilt,RSSI,BeaconMAC];
		```
		
## UI Dashboard

UI Dashboard is available at *http://{node-red-ip}:1880/ui*

<p align="center"><img width="600px" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-samples/node-red-ui-measurement.png" alt="UI measurement"/></p>
