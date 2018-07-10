---
typora-copy-images-to: images_Lab1
---

# Lab1: Smart Lamp connection to Watson IoT Platform on IBM Cloud

## Introduction
These practical exercises are intended to illustrate the use of so-called 'intelligent' urban devices, which beyond their primary function, also include communication, telemetry and action capabilities.

## Description of hardware device
The equipment available represents in a reduced model what could be a 'Smart street street lamp'.

The device consists of a Raspberry Pi 3 minicomputer running a Linux flavor called Raspian based on the Debian distribution, to which an Arduino Uno microcontroller is connected via a serial over USB link (blue cable): ![](images_Lab1/SmartLamp.png)

The Raspberry Pi 3 is connected to the internet and the Cloud via a Wi-Fi connection, and will represent the heart of the intelligence of the communicating system: ![](images_Lab1/RaspBerryPi.png)

The Lamp devcice is powered by an Arduino Uno microcontroller unit, and is used to measure sensor values. It is equipped with an ambient light sensor implemented by a LDR (Light Dependent Resistor), a miniature solar panel, and an array of 5 LEDs located below the panel.

The raw sensor values are sent to the Raspberry Pi over the serial link. Commands from the Raspberry Pi are received by the device to set the LED lights values: ![](images_Lab1/LampDevice.png)

The primary function of the street lamp is of course to illuminate by means of a source of light, which will be simulated here by a bar of 5 controllable LEDs.

The Raspberry Pi itself has environmental sensors for temperature, pressure, humidity, whose values ​​are added to the payload of the lamp and sent to the cloud.

## Goal of the lab
The proposed exerices are intended to illustrate how an intelligent street light would behave, firstly to allow tracking of environmental data, but also to implement 'intelligent' behaviors via programmable automatisms, or to propose new functionalities.

## Organisation
The two first lab exercises are broken down into 6 sections:
  Lab 1.1. Getting started and simple connectivity to the cloud and plotting the data collected by the sensors.
  Lab 1.2. Create an account on the IBM Cloud Watson IoT platform.
  Lab 1.3. Connecting the SmartLamp to the IoT platform, defining and cataloging devices
  Lab 2.1. Create a Watson IoT dashboard to view the data.
  Lab 2.2. Creation of an interactive dashboard to control the lighting of the lamp.
  Lab 2.3. Creation of an automatic lamp control mechanism.

# 1. Getting started and connectivity
For the exercise, the lamps are connected on the internet via the Raspberry Pi.

A control software, called gateway runs on the Raspberry Pi and has the role of relaying the measurements taken by the sensors connected to the Arduino to the cloud.

This Control software is developed with a visual programming environment, Node-RED.

Node-RED can run on a variety of hardware types, from linux Single Board Computers such as Raspberry Pi to Cloud Environments.

The Node-RED program running on the gateway has been installed for you, but will require a configuration specific to your Cloud connection.

## Setting up the Node-RED gateway
### Accessing Raspberry's Node-RED
Node-RED is programmed through a web interface accessible from your laptops.

To get the IP adress of your Raspberry PI, click on the small joystick near the ethernet port. The IP address will scroll on the display.

Access your Raspberry Node-RED instance from your web browser using the on 1880 port i.e. http://Raspi_IP_Adress:1880/ ![](images_Lab1/markdown-img-paste-2018040715382670.png)

### Configuring the gateway to send sensor's data on the cloud
The different tabs are programs (flows) that implement a functionality of the gateway. For example, the first tab, `SenseIP`, displays the IP address of the Raspberry on the LED display at system startup.

We will focus on the `Watson IoT` tab which is in charge of transmitting the sensor data to the Cloud: ![](images_Lab1/markdown-img-paste-20180407154409764.png)

* Locate the `event` node at the top of the diagram, and double-click on it to open its parameters: ![](images_Lab1/markdown-img-paste-20180407154753749.png)

* If not already done, select `Device` and `Quickstart`. An identifier must be filled in the `Quickstart Id` field. This identifier is used to recognize the different devices connecting to the Watson IoT platform on IBM Cloud.
NOTE: ___The QuickStart identifier must be unique___, so you may want to select e.g. `000000.XXXXXX` where `X` is the number of your Raspberry Pi as identifier.

* Click on `Done` and once back on the 'flow', on the `Deploy` button at the top right: ![](images_Lab1/markdown-img-paste-20180407155550960.png)

* The status of the `event` node should indicate `connected` preceded by a green dot:
![](images_Lab1/markdown-img-paste-2018040715573861.png)

* We will now be able to view the sensor data on the test cloud environment. Reopen the properties of the `event` node by double-clicking on it and select the button to the right of the field `Quickstart Id` ![](images_Lab1/markdown-img-paste-2018040715595513.png). This opens a new tab where the sensor data is displayed:
  ![](images_Lab1/markdown-img-paste-20180407160138581.png)

* The first field that is plotted is `ts` which is a record of time, so it is normal for it to evolve linearly.

* Navigate to the bottom of the screen and select another field, for example `solar` or `ldr`: ![](images_Lab1/markdown-img-paste-2018040716034615.png)the measurement graph is displayed and you can influence on the sensors (`ldr` or `solar` solar panel) by passing your hand in front to see the variation of the values on the graph.

  Raspberry equipped with a SenseHat (all except number 0) also have a record of temperature, humidity, pressure. One can act on the temperature or humidity by blowing on the sensors. 

* You can also access the plot from for example the browser of your mobile phone.

This illustrates how to connect a sensor to a cloud data collection system.   

## Control of the lamp LEDs from the Raspberry Pi
The Raspberry Pi has been set to send a lighting sequence to the lamp. We will first test this mechanism from Node-RED on the Raspberry Pi:

* Return to Raspberry Pi's Node-RED web interface, and navigate to the tab  `Arduino Streetlight`: ![](images_Lab1/markdown-img-paste-2018040717251711.png)
* The sequence at the bottom of this flow makes it possible to send a command to the lamp. The lamp includes codes to light its various colors, R for Red, B for Blue, G for Green, 0 to 9 for various intensities of White, etc. Click on the pellet to the left of the button `RGBWOY`: 
* The lamp will light in sequence with the colors Red, Green, Blue, White, Orange, Yellow
* By double-clicking on the RGBWOY node, you can modify the parameters and the sequence that will be sent, for example `W9753RGB0`: ![](images_Lab1/markdown-img-paste-20180407173321430.png) N'oubliez pas de validate the changes by clicking on the button `Deploy` ![](images_Lab1/markdown-img-paste-20180407173300420.png)

# 2. IBM Watson & Cloud account creation Introduction
For the moment, we can only plot a few data points on a single sensor at a time, we will now set up a more specific system on IBM Cloud and Watson IoT allowing multiple values to be taken into account simultaneously and to compose a display more A privileged.

To do this, you must first register on the IBM cloud.

## Create an IBM Cloud Account
We will use the shortcuts provided in the screen `QuickStart`.

* Go back to https://quickstart.internetofthings.ibmcloud.com/#/device/27d4e32d.145689/sensor/ and click on the 'CREATE APP' button: ![](images_Lab1/markdown-img-paste-20180407161516446.png)

* Click on the button `Sign up to Create` on the bottom right: ![](images_Lab1/markdown-img-paste-20180407161631662.png)

* On the next screen, enter your email (or alias), Last Name, First Name and Password.

* IMPORTANT : Specify `United States` as country to be able to use beta features of IBM Cloud.
  ![1524140589073](images_Lab1/1524140589073.png)

* Then click on  `Create Account` ![](images_Lab1/markdown-img-paste-20180407161910216.png)

* ![](images_Lab1/markdown-img-paste-20180407162020834.png)

* A confirmation email should be sent shortly, entitled `Action required: Confirm your IBM Cloud account`. Follow the instructions in this message to validate your account on IBM Cloud.

* After having confirmed the creation of the account, ![](images_Lab1/markdown-img-paste-20180407182743424.png)
  proceed to login : ![](images_Lab1/1523881637675.png)

## Creating a Watson IoT platform instance
From the home screen ![](images_Lab1/1523881916245.png)
Click on ![](images_Lab1/1523881735519.png)

In the filter field, type `iot`: ![](images_Lab1/markdown-img-paste-20180407184023734.png)
Then in the `Boilerplate`:
![](images_Lab1/1523881953304.png)

Choose a unique name, for example `guest1-raspi`, keep the default for the other fields![](images_Lab1/1523882082292.png)

the clck on  `Create` ![](images_Lab1/1523882967106.png)

Then wait for the platform to start

# 3. Connecting the SmartLamp to the Cloud Infrastructure
We will now connect the SmartLamp to the Watson IoT platform we created in the previous step.

To do this, we will change the login credentials on the Raspberry Pi, and create a definition for the SmartLamp in Watson IoT platform.

## Access to the Watson IoT platform
We will define the deivce type `SmartLamp` in Watson IoT so that we can then use it:

* In IBM Cloud, access the dashboard from the menu at the top left: ![](images_Lab1/1523883271822.png),   

* then select `Dashboard`: ![](images_Lab1/1523883295204.png)

* locate the Internet of Thing platform service: ![](images_Lab1/1523883380357.png) and select it

* On next screen, click on  `Launch`: ![](images_Lab1/1523883438859.png)

You should arrive on the management console of your Watson IoT Plateform:
![](images_Lab1/1523883578566.png)

## Changing security settings
As this is an exercise, we will allow a simpler configuration of the security requirements of the terminals, by disabling the obligation to have SSL encrypted communications.

* Select  `Security` entry in the menu ![](images_Lab1/1523883626598.png)

* Edit Connection Security parameters: ![](images_Lab1/1523883683062.png)

* Select `TLS Optional` in the security level for scope `default `![](images_Lab1/1523883727575.png)   

Click on OK on the Warning message ![1523883864915](images_Lab1/1523883864915.png)And on the `Save`  button located at the top right postion ![](images_Lab1/1523883814056.png)

## Creating the lamp definition
The lamp sends structured data in a JSON format with attributes corresponding to the values recorded by the various sensors.

Here we will only have one system, but in a real deployment, there may be many different types of hardware (called devices), and you have to tell the cloud platform what these devices are to be able to manage them according to their type.

* Access the terminal management menu: ![](images_Lab1/1523884128703.png)

* There is normally no terminal defined at this point, click on `Add Device` button on the top right position  to add a new device![](images_Lab1/1523884161580.png) 

* Select  `Device Types` tab ![](images_Lab1/1523884401960.png), then `Add Device Type` ![](images_Lab1/1523884710354.png)

* Enter device type name `RaspiLamp`, and an optional description: ![](images_Lab1/1523996709811.png), then on the `Next`  button.

* You can optionnaly add information on the  `Device Information ` tab and validate by pushing the `Done` on the bottom right ![](images_Lab1/1523888409079.png)

* On the next screnn, select `Register Divces`![](images_Lab1/1523888569717.png), then enter `RaspiLampX` as name where  `X` is the number of your lamp.![](images_Lab1/1523888653416.png), then push the `Next` button.

* Click`Next` until  `Security` tab, in the  `Authentication Token`, enter a value that you can easily remember such as the same name as your lamp, i.e. `RaspiLampX`: ![](images_Lab1/1523888776551.png) 
  This token is in fact the password of the terminal and will be used to connect it to the platform in a secure way.

* Click`Next`until the last page and then `Done` ![1523888409079](images_Lab1/1523888409079.png) 
  Please take note of your Organization ID :

  ![1523888876760](images_Lab1/1523888876760.png)

## Registration of your streetlamp on your Watson IoT organization 
We will now modify the characteristics of the lamp connection in the Raspberry Pi to connect to this organization and to use the lamp data.

### Configuring data collection to Watson IoT
* Switch to another tab or window of your browser on the Node-RED interface of your lamp at the address 

  (Remember : To get the IP adress of your Raspberry PI, click on the small joystick near the ethernet port. The IP address will scroll on the display)

  Access your Raspberry Node-RED instance from your web browser on 1880 port i.e. `http://<<YOUR-IP-ADDRESS>>:1880/`

* Select `Watson IoT` tab the open `event` node properties![](images_Lab1/markdown-img-paste-20180408233444878.png)

* Check that `Connect as` is set as `Device`, and select the radio button `Registered` ![](images_Lab1/markdown-img-paste-20180408233601798.png)

* The next line changes to  `Credentials`, and click on the edit button on the right ![](images_Lab1/markdown-img-paste-2018040823371669.png) and enter connection parameters of your lamp (Organization, Device Type, Device ID and Auth Token)
  ![](images_Lab1/markdown-img-paste-2018040823544517.png)

  then click the `Add` button on the top right

* Click on `Done` ![](images_Lab1/markdown-img-paste-20180408234123276.png)   
  then click on  `Deploy` for the changes to be taken into account.

* The dot color of the node should briefly turn yellow or green, then go green:![](images_Lab1/markdown-img-paste-20180408235551410.png)
  The new lamp is now connected

### Configuring reception of commands
In the same way, we will reparametrer the node which receives the commands of the platform Cloud Watson IoT.

* Open the properties of the node`all commands` ![](images_Lab1/markdown-img-paste-20180408235858284.png)

* Change `Conect as` to `Device` and select the credentials from the menu: ![](images_Lab1/markdown-img-paste-20180409000027846.png)
then click on `Done`

* After clicking on `Deploy`, the node dot goes red and then green

### Verification on the Watson IoT platform
* Return to the Watson IoT tab, select `Devices`  ![1523890538465](images_Lab1/1523890538465.png)

  the line of the lamp must have a green dot on the left: ![](images_Lab1/markdown-img-paste-20180409000450612.png)

* Selecting the `RaspiLampX` line the  `Recent Events` tab, we see the data arrive:
* ![](images_Lab1/markdown-img-paste-20180409000622539.png)

### Definition of a device data type
Now that the sensor is plugged in, we will be able to introspect it to know its data format

* Return to the  `Devices` section, then select the type RaspiLamp and finally the tab `Interface` ![](images_Lab1/1523998594979.png)

* Select`Create Interface` ![](images_Lab1/1523998658208.png), the events that have arrived make it possible to know which attributes exist, then`Add Property` ![](images_Lab1/1523998702739.png)

* Select `d` ![](images_Lab1/markdown-img-paste-20180409001745575.png) and then `Next`

* Finaly the `Add` button: ![](images_Lab1/markdown-img-paste-20180409001912906.png), and then validate by pushing `Done` ![](images_Lab1/markdown-img-paste-20180409002009572.png)

# 4. Configuring historical Data Storage
Watson IoT Platform has a built-in mechanism to automatically store the incoming device data to No-SQL Storage in a Cloudant DataBase.

A Cloudant DataBase has been setup as part of the IoT Boilerplate creation. It is used to store the Node-RED configuration and flows.   
We will now configure Watson IoT platform to also store the device payloads.

We enable this feature as early as possible in the day so that we will have enough collected data for use in the Analytics Labs later on.

Switch back to the Watson IoT platform management interface
* Select the `Extensions` tab in the left-side menu: ![](images_Lab1/markdown-img-paste-20180609212846241.png)
* Click `[Setup]` button in the Historical Data Storage pane: ![](images_Lab1/markdown-img-paste-20180609212938862.png)
* Select the existing Cloudant service instance ![](images_Lab1/markdown-img-paste-20180609213257389.png)
* On the next panel, you can select the Bucket Interval, which determines how often the name of the storage database is rolled over. For this lab you may choose any one of the options, e.g. `Month`. You can leave the database name to `default`, then click `[Done]`:  ![](images_Lab1/markdown-img-paste-20180609213546855.png)

* Note: as mentioned, 'When you click Done we'll open a new window to authorize connecting the Cloudant service to your Watson IoT Platform service.', so the connection to Cloudant is validated in a Pop-Up window, make sure you display and validate this pop-up. If you have popup disabled in your browser, it may show up elsewhere on the browser page, e.g. for Chrome in the top bar as ![](images_Lab1/markdown-img-paste-20180609213802882.png)   
Make sure you validate the pop-up!:
![](images_Lab1/markdown-img-paste-20180609213909366.png), you should go through this window at some point: ![](images_Lab1/markdown-img-paste-20180609214013698.png)
* Check that data is logged into the Cloudant DB
* Confirm the connection with the `[Confirm]` button, and verify that the status is now confirmed, such as: ![](images_Lab1/markdown-img-paste-20180609214219273.png)

Since the device is sending data, storing into Cloudant should start immediatelly. We can verify this by looking into the Cloudant Database:
* Go back to the IBM Cloud dashboard: ![](images_Lab1/markdown-img-paste-20180609214610396.png)
* Locate the Cloudant service and select it, then click the `[Launch]` button ![](images_Lab1/markdown-img-paste-20180609214824409.png)
* You are taken to the Cloudant management interface. Select the databases icon ![](images_Lab1/markdown-img-paste-20180609215236491.png). You should be seeing a list of databases, of which the Node-RED storage, and your device storage bucket DB:
![](images_Lab1/markdown-img-paste-20180609215426613.png)
* Select the database bucket for the current month, you will see your documents, one row for each sensor value. Select one of the rows to see the values.
* We will add a search index to yield sensor data directly. On the `search` index, select the menu and `clone`: ![](images_Lab1/markdown-img-paste-20180609222319654.png)
* use `searchdata` as name: ![](images_Lab1/markdown-img-paste-20180609222405554.png)
* Edit the newly created cone index: ![](images_Lab1/markdown-img-paste-20180609222438284.png)
* Change the function to return only `sort`, `deviceId`, `timestamp` and then `ldr`, `solar`, `ts` renamed to `dts`, `temp`, `rawTemp`, `humidity`, `temperature`, `pressure`:
``` javascript
function (doc) {
  if (doc.deviceType && doc.deviceId && doc.eventType && doc.timestamp && doc.data) {
    index("sort", Math.random(), {"store": true});
    index("deviceId", doc.deviceId, {"store": true});
    index("timestamp", doc.timestamp, {"store": true});
    index("ldr", doc.data.d.ldr, {"store": true, "facet":true});
    index("solar", doc.data.d.solar, {"store": true, "facet":true});
    index("dts", doc.data.d.ts, {"store": true});
    index("temp", doc.data.d.temp, {"store": true});
    index("rawTemp", doc.data.d.rawTemp, {"store": true});
    index("temperature", doc.data.d.temperature, {"store": true, "facet":true});
    index("humidity", doc.data.d.humidity, {"store": true, "facet":true});
    index("pressure", doc.data.d.pressure, {"store": true, "facet":true});
  }
}
```
Click ![](images_Lab1/markdown-img-paste-20180609223216201.png) to store.
* Test the index by entering the `*:*` special query on your new `searchindex` index.
* You can also enter a query such as: `solar:234` or range: `ldr:[16 TO 54]`

Note: the historical data can be accessed from Node-RED using a Cloudant node and the search index.

# 5. Configuring the Raspberry Pi as a gateway
In the first section of the lab, we have configured the RaspBerryPi as a single Device.   
It could be configured as a Gateway, which would then send sensor data on behalf of devices.

You can optionally do this now, the steps would be:
* Create a new Gateway type, e.g. `RaspiGateway`
* Create a `RaspiGatewayX` instance (use the deviceId as password)
* Modify the Node-RED flow on the RaspBerryPi so that the device connection type is now Gateway. Modify both the WIoTP in and out nodes.
* The gateway will now send the data on behalf of the device.

# Conclusion
We have now completed the first Lab and setup IoT Data Collection, we will now be able to build data visualization and further analytics.
