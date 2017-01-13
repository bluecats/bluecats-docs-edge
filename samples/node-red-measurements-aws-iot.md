# Send Measurement Data to AWS IoT
In this sample scenario, the edge relay sends measurement data to AWS IoT mqtt broker and the node-red flow is used to connect to the broker and visualise the data in a dashboard. 

## Edge Relay Configuration

- [Configure MQTT Endpoint](https://github.com/bluecats/bluecats-docs-edge/blob/master/getting-started-edge-applications.md#configure-endpoints) in Edge Relay pointed to the AWS IoT mqtt broker. Enter Host/IP and port of the aws IoT mqtt broker and change TLS Enabled to 1.

<p align="center"><img width="400px" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-samples/edge-endpoint-mqtt-aws.png" alt="Endpoint MQTT"/></p>

- Upload the AWS Certificate files

<p align="center"><img width="400px" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-samples/edge-upload-aws-cert-files.png" alt="AWS CERTS"/></p>

- Configure the Measurement tab to send data in JSON format to the MQTT broker.

<p align="center"><img width="400px" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-samples/edge-measurement-mqtt.png" alt="Upload certs"/></p>

## Node-red configuration

- Install additional nodes
	- [node-red-dashboard](https://flows.nodered.org/node/node-red-dashboard)

	- [node-red-contrib-aws-iot-hub](https://flows.nodered.org/node/node-red-contrib-aws-iot-hub)
	
- Create the node-red flow

	Import the following flow into node-red. 
	
	```
	[{"id":"67d228c3.a03058","type":"debug","z":"7ac725cb.258bd4","name":"","active":true,"console":"false","complete":"false","x":582,"y":55,"wires":[]},{"id":"2211dd99.5882da","type":"json","z":"7ac725cb.258bd4","name":"parseJson","x":242,"y":179,"wires":[["148f3bc8.348d34"]]},{"id":"148f3bc8.348d34","type":"function","z":"7ac725cb.258bd4","name":"getSensorData","func":"var Tilt = {payload:msg.payload.Measurement[0].Data[0].toFixed(1)};\nvar RSSI = {payload:msg.payload.RSSI};\nvar BeaconMAC = {payload:msg.payload.BeaconMAC};\nreturn [Tilt,RSSI,BeaconMAC];","outputs":"3","noerr":0,"x":450,"y":178,"wires":[["987f6287.b07238","c984a3ab.16608"],["c6b7f3ab.7fa778"],["4399659.486cb9c"]]},{"id":"987f6287.b07238","type":"ui_gauge","z":"7ac725cb.258bd4","name":"","group":"8500c325.a7b798","order":0,"width":0,"height":0,"gtype":"compass","title":"Tilt","label":"Tilt","format":"{{value}}°","min":0,"max":"360","colors":["#00B500","#E6E600","#CA3838"],"x":640,"y":162,"wires":[]},{"id":"c984a3ab.16608","type":"ui_chart","z":"7ac725cb.258bd4","name":"Timeline","group":"8500c325.a7b798","order":0,"width":0,"height":0,"label":"Timeline (Last hour)","chartType":"line","legend":"false","xformat":"HH:mm","interpolate":"linear","nodata":"","ymin":"","ymax":"","removeOlder":1,"removeOlderPoints":"","removeOlderUnit":"3600","cutout":"","x":650,"y":204,"wires":[[],[]]},{"id":"c6b7f3ab.7fa778","type":"ui_gauge","z":"7ac725cb.258bd4","name":"RSSI","group":"88911b8a.58cba8","order":0,"width":"2","height":"2","gtype":"gage","title":"RSSI","label":"","format":"{{value}}","min":"-100","max":"0","colors":["#CA3838","#E6E600","#00B500"],"x":641,"y":250,"wires":[]},{"id":"4399659.486cb9c","type":"ui_text","z":"7ac725cb.258bd4","group":"88911b8a.58cba8","order":0,"width":"4","height":"2","name":"","label":"BeaconMAC","format":"{{msg.payload}}","layout":"col-center","x":661,"y":299,"wires":[]},{"id":"708a0e5c.05974","type":"comment","z":"7ac725cb.258bd4","name":"AWS IoT Server","info":"connected to the AWS IoT (MQTT) server ","x":108,"y":24,"wires":[]},{"id":"d1d4d42c.39d3f8","type":"aws-mqtt in","z":"7ac725cb.258bd4","device":"e1399352.0a5ed8","topic":"edge/measurement/#","x":121,"y":77,"wires":[["67d228c3.a03058","2211dd99.5882da"]]},{"id":"8500c325.a7b798","type":"ui_group","z":"","name":"From AWS","tab":"e00d81d8.bafe4","order":1,"disp":false,"width":"8"},{"id":"88911b8a.58cba8","type":"ui_group","z":"","name":"BLE Info","tab":"e00d81d8.bafe4","order":2,"disp":false,"width":"6"},{"id":"e1399352.0a5ed8","type":"aws-iot-device","z":0,"name":"aws","mode":"broker","clientId":"2e35c1c974","region":"us-east-1","awscerts":"/home/chan/.awscerts/"},{"id":"e00d81d8.bafe4","type":"ui_tab","z":"","name":"AWS IoT","icon":"dashboard","order":4}]
	```
	
<p align="center"><img width="400px" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-samples/node-red-aws-flow.png" alt="Node-red aws flow"/></p>
	
- Configure nodes
	- AWS-MQTT-In Node
		- Select ‘Add new aws-iot-device…’
		
		<p align="center"><img width="400px" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-samples/node-red-mqtt-node-add.png" alt="New AWS IoT"/></p>
		
		- Select Type, Region and enter client ID.

		- Upload aws certificate files to a location where the node-red can access.

		- Enter the certificate path in AWS Certs.

		<p align="center"><img width="400px" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-samples/node-red-aws-mqtt-config.png" alt="Configure AWS IoT"/></p>
		
		- Enter the mqtt topic ‘edge/measurement/#’
		
		<p align="center"><img width="400px" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-samples/node-red-aws-config.png" alt="Configure AWS Node"/></p>
		
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

<p align="center"><img width="600px" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-samples/node-red-ui-aws.png" alt="UI aws"/></p>
