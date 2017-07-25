## Update Bluetooth Firmware

There are two ways to update the edge bluetooth firmware, manually uploading a file or via the web UI. Use manual firmware updates when a connection to the internet and BlueCats Cloud is not available.

### Using Web Interface
This requires the edge to be internet connected which you can do by configuring the WIFI as described in the getting started doc. 

Then navigate to `Bluecats` -> `Connect and Manage`. If the 'Device Register Status' is 'Registered' then the edge device is successfully connected with the Bluecats cloud services. If the status is 'Invalid' or you need to re-register the device, refer [registering the device](getting-started-connect.md#register-with-bluecats-cloud). 

<p align="center"><img width="600px" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-firmware-update/Edge-update-ble-module-cloud.png" alt="Update BLE Firmware"/></p>

Once registered in bluecats cloud, click `Update` button on the `Update to Latest BLE Module Firmware` section to update the edge BLE module to the latest firmware. 


### Manually Uploading a File

#### Downloading the New Firmware
1. Go to our BlueCats App and log in with your BlueCats account https://app.bluecats.com/login
2. On the left-hand side, click on the "Devices" tab
3. Find your Edge and click on the "Settings" or the gear icon in the upper right corner
4. Click and download the latest BLE firmware file to your local machine

<p align="center"><img width="700px" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-firmware-update/EdgeBLEFirmwareDownload.png" alt="Download BLE Firmware"/></p>

#### Upload and update

Navigate to `Bluecats` -> `Conenct and Manage` and then select `Local` tab.

<p align="center"><img width="700px" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-firmware-update/Edge-connect-and-manage.png" alt="edge-connect-and-manage-menu"/></p>

CLick `Get Info` button to request all the available configuration and version information from the BLE module.

<p align="center"><img width="600px" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-firmware-update/Edge-get-ble-module-info.png" alt="get-ble-module-info"/></p>

To update the BLE firmaware to the latest version, select the dowloaded image file from the local machine by clicking the `Choose File` button in the `Upload BLE Module Firmware` section and click `Upload...` button.

<p align="center"><img width="700px" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-firmware-update/edge-upload-ble-firmware-file.png" alt="upload-ble-firmware-file"/></p>

To apply the new version, click `Update` button in the `Apply Uploaded BLE Module Firmware` section. 

<p align="center"><img width="500px" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-firmware-update/Edge-apply-ble-firmware.png" alt="apply-ble-firmware"/></p>


