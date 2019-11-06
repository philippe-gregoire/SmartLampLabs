---
typora-copy-images-to: images_Lab2
---

# Lab 2: Smart Lamp data visualization with Watson IoT Platform on IBM Cloud

## Introduction
In this lab, we will use two different techniques to visualize the IoT sensor data collected on the Watson IoT platform.

We will first use the real-time insights dashboard provided by the platform, and then use an external application, in this case Node-RED running as a Cloud Foundry application on the IBM Cloud.

### Prerequisite
This lab assumes that the RaspiLamp types, schemas and devices of Lab 1 have been setup.

# 1. Creating a dashboard on Watson IoT Platform
We will now create a graphical interface to plot the data of the various sensors of the lamp.

* Select  `Dashboard` menu ![](images_Lab2/1523998911937.png)
* Click on the big empty rectangle to add a table ![](images_Lab2/markdown-img-paste-2018040900083802.png)
* remplir les données ![](images_Lab2/1523998997379.png)   
  then `Next` and `Submit`

* Open the newly created table: ![](images_Lab2/1523999055382.png)

* Click `Add New Card` to add items to the table ![](images_Lab2/1523999102686.png)

### Adding a gauge
* For example, add a brightness gauge ![](images_Lab2/markdown-img-paste-20180409002220784.png)
* ![](images_Lab2/1523999175170.png)
* ![](images_Lab2/markdown-img-paste-20180409002307504.png)
* ![](images_Lab2/1523999849043.png)
* ![](images_Lab2/1523999879960.png)
* Check that the type is `Number`
* Choose size and design of your gauge: ![](images_Lab2/markdown-img-paste-20180409002509262.png)

### Adding a multi-value graph

* Choose a card `Line chart`: ![](images_Lab2/1524000008596.png)
* Add two datasets, one for `ldr`:
  ![](images_Lab2/markdown-img-paste-20180409002826552.png) 
* Another on for `solar`:![](images_Lab2/markdown-img-paste-20180409002922604.png)
* Choose a size chart `L`: ![](images_Lab2/markdown-img-paste-2018040900301994.png)
* The values of the two sensors will be displayed on the same diagram
  ![](images_Lab2/markdown-img-paste-20180409003200740.png)

You can create maps for other sensor values.

# 2. Creating an interactive dashboard with Node-RED
We will now create a dashboard to act on the lamp. For this we will use Node-RED running as a WebApp on the IBMCloud.

## Starting Node-RED
* Return to the IBM Cloud Dashboard (https://console.bluemix.net/dashboard/apps)
* You must have an application running;![](images_Lab2/1524000182212.png)Select it
* In the  `Routes` menu select the first one : ![](images_Lab2/1524000243666.png)
* The node-RED setup screen is displayed, click `Next` then![](images_Lab2/markdown-img-paste-20180409003923652.png) forcez la configuration non-sécurisée ![](images_Lab2/markdown-img-paste-20180409004045750.png) ![](images_Lab2/markdown-img-paste-20180409004027446.png)
* Press `Next` and then `Finish`
* Finally, click ![](images_Lab2/markdown-img-paste-20180409004132691.png), which opens the Node-RED flow editor.

## Creating a "send command" flow
*  `Flow 1` predefined does not interest us necessarily, we will create another thanks to the + top right; ![](images_Lab2/markdown-img-paste-20180409004351277.png)

* From the palette on the left side of the screen, add a  `ibmiot` send node ![](images_Lab2/markdown-img-paste-20180409004752235.png). Warning, there are two similar nodes, this one has an entry on the left, and it comes from the drawer `output`

* Add an `Inject` node ![](images_Lab2/markdown-img-paste-2018040900492133.png) and wire them: ![](images_Lab2/markdown-img-paste-20180409004937154.png)

* modify the properties of the injected node to send a string of characters: ![](images_Lab2/markdown-img-paste-20180409005047469.png)
like RGB for instance

* Change`ibmiot` node parameters to match your lamp:![](images_Lab2/markdown-img-paste-20180409005538516.png)
  * `Authentication` should be `Bluemix Service`
  * `Output Type` must be `**Device Command**`
  * `Format` must be `text`,

* Activation of the entry (left pellet of the node) ![](images_Lab2/markdown-img-paste-20180409005903936.png) of the `inject` node should turn on your lamp.

* **Note that this interface is accessible from the internet** now, which was not the case in the first part.

## Adding a dashboard
### Setup
* From the menu at the top right, select `Manage Palette`: ![](images_Lab2/markdown-img-paste-20180409010132557.png)

* Toggle on the tab `Install` ![](images_Lab2/markdown-img-paste-20180409010214760.png)

* Type `node-red-dash` in the search field ![](images_Lab2/markdown-img-paste-20180409010320867.png)

* Click on the `Install` button ![](images_Lab2/markdown-img-paste-20180409010347436.png) 
This added a series of nodes in the left palette, under the category `Dashboard`:
 ![](images_Lab2/markdown-img-paste-20180409010533590.png)

### Creating an interactive command dashboard
* From the palette, add  a `Text input` node and wire it to  `IBM iot`: ![](images_Lab2/markdown-img-paste-20180409010749234.png)

* Change node settings: Create a group: ![](images_Lab2/markdown-img-paste-20180409011008846.png)
* Then a `tab` ![](images_Lab2/markdown-img-paste-20180409011031580.png) ![](images_Lab2/markdown-img-paste-20180409011138289.png)
* Accept defaults or change names to your liking
* Validate and deploy, then click on the table launch from the new tab `dashboard` on the top right: ![](images_Lab2/markdown-img-paste-20180409010859224.png)

* The dashboard is displayed: ![](images_Lab2/markdown-img-paste-20180409011314233.png)
* You can now interactively change the color of the lamp by typing for example W or a sequence

### Adding gauges
We can decorate this dashboard of various displays:
* Add 3 nodes from the palette:
  * `ibmiot` with do on right ![](images_Lab2/markdown-img-paste-2018040901174958.png) 
  * `change` ![](images_Lab2/markdown-img-paste-20180409011831566.png)
  * `gauge` ![](images_Lab2/markdown-img-paste-20180409011854979.png)

* Configure the nodes as follows:
  * `ibm IoT`, modify`Authentication`: ![](images_Lab2/markdown-img-paste-20180409012042963.png)
  * The  `change` node is used to extract the incoming message data to the  `gauge ` node format, we extract here the value of `ldr`: ![](images_Lab2/markdown-img-paste-20180409012142496.png)
  * For the gauge, we will change the display limit: ![](images_Lab2/markdown-img-paste-20180409012346167.png)

* Wire the nodes together: ![](images_Lab2/markdown-img-paste-20180409012809402.png)then deplo ![](images_Lab2/markdown-img-paste-20180409012836750.png)

* When we go back to the dashboard, we now see a gauge that changes when the sensor is more or less lit: ![](images_Lab2/markdown-img-paste-20180409012859368.png)

**This interface is now exposed directly on the internet**, you can connect to its public URL from a smartphone for example.

### Be creative!
You can change the dashboard at your convenience!

# 3. Control and automation of the SmartLamp
Last step, we will program a simple automation mechanism that changes the color of the lamp according to the ambient light level.

## Color change based on ambient light
* Add a `switch` node ![](images_Lab2/markdown-img-paste-2018040901385136.png)and set it with the button `Add` ![](images_Lab2/markdown-img-paste-20180409013214616.png)as follow: ![](images_Lab2/markdown-img-paste-20180409013628216.png) 
* The node now has 3 outputs. Add 3 `change` nodes ![](images_Lab2/markdown-img-paste-20180409013912387.png), and configure each one for a given R G B color:![](images_Lab2/markdown-img-paste-20180409013810516.png)
* Wire as follow ![](images_Lab2/markdown-img-paste-20180409014018692.png) and deploy

The lamp will change color depending on the illumination of the LDR sensor.

# 4. Lab Stretch Goal
For this section of the Lab, you will be tasked with a creating a more complex Dashboard, without precise instructions but just a functional and visual description, and a few hints.


The goal now will be to create a more elaborate dashboard, which will look like the following: ![](images_Lab2/markdown-img-paste-20180609203630961.png)
* The line chart graph shows the value of both the `solar` and `ldr` sensors
* The set of buttons on the left will send a single W,R,G,B,0 command to the RaspiLamp
* The text entry below will send a command string when the `[Set]` button is clicked.
* The Clear button will reset the line graph
* The 4 dial gauges below will reflect the current value of the LEDs.

Implementation Hints:
* To set multiple values on the line graph, each sensor value must be passed in `msg.payload` with a `msg.topic` set to the sensor ID. You can use a `change` node for that, e.g.: ![](images_Lab2/markdown-img-paste-20180609201840581.png)
* The LEDs colors are in the `d.leds`, as a WRGB field, with an octet for each element of the LED. You will need a `function` node with multiple outputs to split the LED colors into 4 outputs: ![](images_Lab2/markdown-img-paste-2018060920160023.png)
* To clear the line graph, you must send it a `msg.payload` with an empty array:![](images_Lab2/markdown-img-paste-20180609202111982.png)

## Be creative!
On this model, you can design different dashboards or automations loops variations to your liking!

# END of Lab 2