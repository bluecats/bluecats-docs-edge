## Flashing new firmware

There are two ways to update the edge firmware, manually uploading a file or via the web UI.

### Manually uploading a file

Download the sysupgrade-compatible image file to your local machine. 

<p align="center"><img width="700px" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-firmware-update/Backup-flash-menu.png" alt="update-firmware-menu"/></p>

Navigate to `System` -> `Backup / Flash Firmware` and then choose image file by clicking the choose file button in the 'Flash new firmware image' section. Uncheck the 'Keep settings' checkbox when flashing a major firmware revision. You can keep it checked for minor firmware updates. Click 'Flash Image' button to proceed.

<p align="center"><img width="700px" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-firmware-update/Flash-file-select.png" alt="upload-file"/></p>

Compare the checksum and the file size with the original file to ensure data integrity. Click 'Proceed' to start the flash process. 

<p align="center"><img width="600px" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-firmware-update/Flash-firmware-proceed.png" alt="proceed"/></p>

Wait a few minutes before you try to reconnect. Depending on your settings, it might be necessary to renew the address of your computer to reach the device again. 

### Using web interface
This requires the edge to be internet connected which you can do by configuring the WIFI as described in the getting started doc. 

<p align="center"><img width="600px" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-firmware-update/connect-manage-menu1.png" alt="Register and Manage"/></p>


Then navigate to `Bluecats` -> `Connect and Manage`. If the 'Device Register Status' is 'Registered' then the edge device is successfully conneected with the Bluecats cloud services. If the status is 'Invalid' or you need to re-register the device, refer [registering the device](getting-started-connect.md#register-with-bluecats-cloud). 

<p align="center"><img width="600px" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-firmware-update/edge-firmware-update.png" alt="Update Firmware"/></p>

Once registerd in bluecats cloud, click 'Update' button in the 'Update to Latest Edge Firmware' section to update the edge to the latest firmware. Wait a few minutes before you try to reconnect. This update will not overwite your settings.

## Updating BlueCats Edge Packages
### Using web interface
There are two ways to update the BlueCats Edge packages to receive updates or after a firmware update. First is via the web GUI. This requires the Edge to be internet connected which you can do by configuring the WIFI as described in the getting started doc. Then you can navigate to `System` -> `Software` and then under the Actions tab there is an input box with 'Download and install package'. You can then paste each of the following links one at a time and hit 'OK'. This will download and install each of the packages.

Contact support@bluecats.com for download urls for latest packages.

Note that this method only supports download via HTTP.

<p align="center"><img width="600px" src="https://s3.amazonaws.com/bluecats-downloads/documentation/bluecats-edge-features/edge-update-package.png" alt="Upload Package"/></p>


### Using SSH

Download the package files to your local computer:

Use secure copy to transfer to the Edge:

    scp path-to-your-files/luci-base.ipk root@192.168.8.1:~
    scp path-to-your-files/luci-app-bluecats.ipk root@192.168.8.1:~

SSH into the Edge:

    ssh root@192.168.8.1

Then install the packages:

    opkg install luci-base.ipk
    opkg install luci-app-bluecats.ipk
