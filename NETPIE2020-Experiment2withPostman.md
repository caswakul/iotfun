
# NETPIE2020-Experiment2withPostman


>Training material for 2102541 IoT Fundamentals Class at Department of Electrical Engineering, Faculty of Engineering, Chulalongkorn University, Thailand.
>
>Aim: To introduce newcomers to NETPIE2020 with (i) device schema (ii) publish/subscribe of 'data'  between Postman-emulated devices and NETPIE broker (iii) NETPIE dashboard.


## References

- Piyawat Jomsathan, Online Training: IoT & NETPIE 2020, [https://netpie.io/guide](https://netpie.io/guide) (in Thai language)
- Example Projects of NETPIE, [https://netpie.io/exampleprojects/en](https://netpie.io/exampleprojects/en)


## Let's Continue

1. Sign-up and log-in with your username and password at netpie.io. Once in NETPIE web, create your own project and enter your project description. Here, we create project 'smartMobility2025' for intelligent transportation system application. 
![p2_1createproj.png](NETPIE2020-Experiment1and2withPostman/p2_1createproj.png)
![p2_1createprojSuccess.png](NETPIE2020-Experiment1and2withPostman/p2_1createprojSuccess.png)

2. Enter menu 'Group' and create a new group. You can name and add description to your new groups. Here, I create 1 group, named 'samyan' to keep our devices installed at Samyan intersection. 
![p2_2createGroup.png](NETPIE2020-Experiment1and2withPostman/p2_2createGroup.png)
![p2_2createGroupSuccess.png](NETPIE2020-Experiment1and2withPostman/p2_2createGroupSuccess.png)

3. Enter menu 'Devices'. Create devices. Here, we will create 2 devices, 'queue_sensor' and 'signal_controller' in this group. 

- *queue_sensor* is an example sensor that publishes **data** to NETPIE. The sensor can report the current queue lengths at an intersection. For instance, there are **200** vehicles in-queue in the **north-south** direction of intersection, and **100** vehicles in-queue in the **east-west** direction of intersection.

- *signal_controller* is an example controller that gets data from NETPIE and takes corresponding necessary control actions to the physical world. For instance, the controller is responsible for switching the traffic signal lights between (**green**, **red**) and (**red**, **green**) for (**north-south**, **east-west**) directions of intersection.


4. **Create 'queue_sensor' in NETPIE**

In NETPIE, we create 'queue_sensor' as follows.
![p2_4createSensor.png](NETPIE2020-Experiment1and2withPostman/p2_4createSensor.png)

Since 'queue_sensor' sends 'data' that would not be only 'text', in NETPIE, we will define 'Schema' for the expected structure of incoming data payload. Click 'Schema' and switch from the Editor Mode 'Tree' to 'Code'. 
![p2_4schemaSensorTree.png](NETPIE2020-Experiment1and2withPostman/p2_4schemaSensorTree.png)
![p2_4schemaSensorCode.png](NETPIE2020-Experiment1and2withPostman/p2_4schemaSensorCode.png)

Device schema format must be in **JSON (JavaScript Object Notation)**. While there are many websites trying to describe JSON format, I personally find the following **original JSON inventor**'s post at [https://www.json.org](https://www.json.org) by *Douglas Crockford* the most concise and programmably meaningful. Check it out at your convenience. But, for the time being, we will define the schema of 'queue_sensor' as follows. And thanks to the readability of JSON, we can try here to explain the intended meaning.

```json
{
  "additionalProperties": false,
  "properties": {
    "vehicles_in_NSqueue": {
      "type": "number"
    },
    "vehicles_in_EWqueue": {
      "type": "number"
    }
  }
}
```

This sensor may represent a CCTV-AI (video camera with digital signal processing software). The software can keep track of the vehicle movement from the camera and determine the number of vehicles in queues. The software can count the number of queueing vehicles, which is a *number*, for each of vehicle flow directions arriving at the considered intersection. The direction can be either "north-south" or "east-west".
Here, "vehicles_in_NSqueue" defines the number of vehicles in north-south queue. And "vehicles_in_EWqueue" defines the number of vehicles in east-west queue. These "vehicles_in_NSqueue" and "vehicles_in_EWqueue" form the main "properties" that we expect of the data from this sensor.

Note that "additionalProperties" is here set to *false*. So, if 'queue_sensor' tries to send any other data values, in addition to "vehicles_in_NSqueue" and "vehicles_in_EWqueue", then NETPIE will neglect those other data values and keep only the data values specified by "vehicles_in_NSqueue" and "vehicles_in_EWqueue". 

Otherwise, if "additionalProperties" is set to *true*, then NETPIE will keep all other received data values as well. With that said, to prevent unnecessary confusion on stored data values, 'queue_sensor' is expected reasonably to send only data values whose syntax is well defined in the schema. That is the purpose of having the schema after all.

Copy the JSON text and paste into the code box defining schema. Click 'SAVE'.
![p2_4schemaSensorCodeEnter.png](NETPIE2020-Experiment1and2withPostman/p2_4schemaSensorCodeEnter.png)

Try switching from 'Code' to 'Tree'. As a tree, we can naturally try clicking to hide or unhide JSON elements:
![p2_4schemaSensorTreeRes.png](NETPIE2020-Experiment1and2withPostman/p2_4schemaSensorTreeRes.png)


5.  **Create 'queue_sensor' in Postman**

In Postman, we create 'queue_sensor' as usual.
![p2_5postmanSensorAdd.png](NETPIE2020-Experiment1and2withPostman/p2_5postmanSensorAdd.png)

Here, we save the created 'queue_sensor' in the collection 'MQTT-NETPIE'.
![p2_5postmanSensorSave1.png](NETPIE2020-Experiment1and2withPostman/p2_5postmanSensorSave1.png)
![p2_5postmanSensorSave2.png](NETPIE2020-Experiment1and2withPostman/p2_5postmanSensorSave2.png)

And after connecting succesfully, we have the following.
![p2_5postmanSensorSaveRes.png](NETPIE2020-Experiment1and2withPostman/p2_5postmanSensorSaveRes.png)


6. **Publish data from 'queue_sensor' to NETPIE**

Instead of using a user-specified topic (like in the first experiment), we will use a special topic of NETPIE, called '@shadow/data/update'. This special topic is used for the device to update its own status with NETPIE. The updated status will be persistent, time-stamped once receiving at NETPIE, and kept as a time series data (called **feed**). 

For this 'queue_sensor' to publish its *data* message, try publishing the first message from this sensor as follows. 

```JSON
{
    "data": {
        "vehicles_in_NSqueue": 200,
        "vehicles_in_EWqueue": 100
    }
}
```
![p2_6SensorSend.png](NETPIE2020-Experiment1and2withPostman/p2_6SensorSend.png)

If your *data* message is sent out successfully, then we should see in Postman:
![p2_6SensorSendRes1.png](NETPIE2020-Experiment1and2withPostman/p2_6SensorSendRes1.png)

If the message arrives successfully at NETPIE, then you should be able to see the latest data values shown in 'Shadow' of 'queue_sensor' in NETPIE:
![p2_6SensorSendRes2.png](NETPIE2020-Experiment1and2withPostman/p2_6SensorSendRes2.png)

This special topic '@shadow/data/update' in NETPIE is used to store the latest received data values from the 'queue_sensor'. If a new data message is published by the 'queue_sensor' and that message arrives at NETPIE, then the 'Shadow' data values of this 'queue_sensor' will be updated correspondingly. You can try this out by yourself by re-adjusting the data message payload, publish the message out, and check what NETPIE stores at the 'Shadow' of 'queue_sensor'.

As an example, after a few times of updating the shadow data values, in Postman, we have:
![p2_6SensorSendFeed.png](NETPIE2020-Experiment1and2withPostman/p2_6SensorSendFeed.png)

Of course, you would have different trial results, but that is alright.

In NETPIE, click 'Feed' in 'queue_sensor'. 
![p2_6feed.png](NETPIE2020-Experiment1and2withPostman/p2_6feed.png)

Then click 'Add datatag' to add the following data tags. 
![p2_6feedNS.png](NETPIE2020-Experiment1and2withPostman/p2_6feedNS.png)
![p2_6feedEW.png](NETPIE2020-Experiment1and2withPostman/p2_6feedEW.png)

You should now see the plot of time series data kept in the shadow of 'queue_sensor'. Here is an example:
![p2_6feedRes.png](NETPIE2020-Experiment1and2withPostman/p2_6feedRes.png)


7. **Create 'signal_controller' in NETPIE and Postman**

In NETPIE, we create 'signal_controller' as follows.
![p2_7createSigCon1.png](NETPIE2020-Experiment1and2withPostman/p2_7createSigCon1.png)

Define the following schema of 'signal_controller':

```json
{
  "additionalProperties": false,
  "properties": {
    "green_in_NSqueue": {
      "type": "boolean"
    },
    "green_in_EWqueue": {
      "type": "boolean"
    }
  }
}
```
![p2_7createSigCon2schema.png](NETPIE2020-Experiment1and2withPostman/p2_7createSigCon2schema.png)

Note that we use "boolean" type to specify **true** or **false** status for opening **green** traffic signal light for the waiting queues in the north-south or east-west directions. Here, if the light is not **green**, then the light is **red**. There is no **yellow** light, for convenient simplicity.

In Postman, we create 'signal_controller' as usual and click 'Save' to save 'signal_controller' in collection 'MQTT-NETPIE'. Note that we configure 'signal_controller' to subscribe to topic "@msg/signal_controller_listen". This topic name is meant here for publishing any messages to 'signal_controller' device. The messages can serve as input commands to switch the traffic signal lights between (**green**, **red**) and (**red**, **green**) for (**north-south**, **east-west**) directions of intersection.
After this configuration, we should have the following.
![p2_7postmanSigCon.png](NETPIE2020-Experiment1and2withPostman/p2_7postmanSigCon.png)


8. **Create NETPIE Dashboard**

Finally, we will create a NETPIE dashboard for GUI displays of all created devices. 

Click 'Create':
![p2_8createDashboard1.png](NETPIE2020-Experiment1and2withPostman/p2_8createDashboard1.png)

Enter dashboard name and description. Then click 'SAVE':
![p2_8createDashboard2.png](NETPIE2020-Experiment1and2withPostman/p2_8createDashboard2.png)

After entering a dashboard name and description, click 'SAVE'. Then we get:
![p2_8createDashboardRes.png](NETPIE2020-Experiment1and2withPostman/p2_8createDashboardRes.png)

Click to enter the dashboard.

In the dashboard, we first need to add sources of data to be displayed. Click 'Settings' and 'Add source':
![p2_8DashboardSettingsAddsource1.png](NETPIE2020-Experiment1and2withPostman/p2_8DashboardSettingsAddsource1.png)

We have 2 data sources from 2 NETPIE devices. 

First, let us add 'queue_sensor' by entering type, device and select priviledges that allow dashboard to "Read Shadow" and "Read Feed":
![p2_8DashboardSettingsAddsource2.png](NETPIE2020-Experiment1and2withPostman/p2_8DashboardSettingsAddsource2.png)

Click "SAVE" and we now have:
![p2_8DashboardSettingsAddsource3.png](NETPIE2020-Experiment1and2withPostman/p2_8DashboardSettingsAddsource3.png)

Second, let us add 'signal_controller' by entering type, device and select priviledges that allow dashboard to "Read Shadow", "Read Feed", and "Publish Message":
![p2_8DashboardSettingsAddsource4.png](NETPIE2020-Experiment1and2withPostman/p2_8DashboardSettingsAddsource4.png)

Click "SAVE" and we now have:
![p2_8DashboardSettingsAddsource5.png](NETPIE2020-Experiment1and2withPostman/p2_8DashboardSettingsAddsource5.png)

9. **Add Panel for North-South Devices of Intersection**

We organise our data display into 2 separate panels. One for north-south and the other for east-west devices of Samyan intersection. Let us add first north-south panel.
![p2_9addPanel1.png](NETPIE2020-Experiment1and2withPostman/p2_9addPanel1.png)

Enter title as 'north-south' and click 'DONE'.
![p2_9addPanel2.png](NETPIE2020-Experiment1and2withPostman/p2_9addPanel2.png)

At the panel, click the top plus sign "+" to add a new widget for 'queue_sensor' to display the number of vehicles in-queue by a *gauge*. Enter widget settings and click 'Done':
![p2_9addPanel3.png](NETPIE2020-Experiment1and2withPostman/p2_9addPanel3.png)

At the panel, click the top plus sign "+" to add a new widget for 'signal_controller' to display signal light status by *indicator*. Enter widget settings and click 'Done':
![p2_9addPanel4.png](NETPIE2020-Experiment1and2withPostman/p2_9addPanel4.png)

Now, we should see the following panel:
![p2_9addPanel5.png](NETPIE2020-Experiment1and2withPostman/p2_9addPanel5.png)

So far, the panel simply shows the sensor data or status of controller. Now, we will add another widget to control the 'signal_controller' from NETPIE dashboard. 

At the panel, click the top plus sign "+" to add a new widget for 'signal_controller' by *button*. Enter the following widget settings. Note importantly that we will use "ONCLICK ACTION" to define a callback function that will be executed once the button is clicked. Since this callback function is quite lengthy, click "EDITOR" to open a new code entering box:
![p2_9addPanel6.png](NETPIE2020-Experiment1and2withPostman/p2_9addPanel6.png)

Into that code entering box, copy and paste the following code and click "Close".

```script
#["signal_controller"].publishMsg("signal_controller_listen","onNS_offEW"); 
#["signal_controller"].writeShadow("green_in_NSqueue","true");
#["signal_controller"].writeShadow("green_in_EWqueue","false");
```

![p2_9addPanel7.png](NETPIE2020-Experiment1and2withPostman/p2_9addPanel7.png)

First line publishes a message "onNS_offEW" to the topic "signal_controller_listen". In real implamentation, when the 'signal_controller' hardware device receives this message, the device can treat it as an input command. For instance, "onNS_offEW" means "setting the traffic signal lights to be (**green**, **red**) for (**north-south**, **east-west**) directions of intersection "

Second and third lines update the shadow data of 'signal_controller' accordingly. 

Now click "Done" to finish this 'button' configuration:
![p2_9addPanel8.png](NETPIE2020-Experiment1and2withPostman/p2_9addPanel8.png)

This finishes 'north-south' panel. Click 'Save'. Importantly, if you forget to click 'Save' here, then all panel changes made will be ignored once you leave this dashboard instance. Remind yourself to always click 'Save'.
![p2_9addPanel9.png](NETPIE2020-Experiment1and2withPostman/p2_9addPanel9.png)

To test the button, try to click the button once:
![p2_9addPanel10.png](NETPIE2020-Experiment1and2withPostman/p2_9addPanel10.png)

You should see the status of shadow for the light to be 'green', as expected from the entered callback function:
![p2_9addPanel11.png](NETPIE2020-Experiment1and2withPostman/p2_9addPanel11.png)

The device 'signal_controller' should also show its shadow update accordingly:
![p2_9addPanel13.png](NETPIE2020-Experiment1and2withPostman/p2_9addPanel13.png)

And at Postman, we should see incoming message from the published message of callback function:
![p2_9addPanel12.png](NETPIE2020-Experiment1and2withPostman/p2_9addPanel12.png)


10. **Duplicate and Modify Panel for East-West Devices of Intersection**

After you are happy with the panel for north-south devices, this final step is simple. We will duplicate and modify the panel for east-west devices. 
![alt p2_10dupPanel1.png](NETPIE2020-Experiment1and2withPostman/p2_10dupPanel1.png)

Finishing the new panel modification, here is the final picture:
![p2_10dupPanel2.png](NETPIE2020-Experiment1and2withPostman/p2_10dupPanel2.png)

Now it is time to verify that everything works. Move on to Exercises.


---

## Excercise 2A

Verify that your dashboard functions perfectly as intended. You can try the following steps:

- Click "On EW" button.

- Observe dashboard light status alternation between red and green

- Observe 'signal_controller' shadow data update

- Observe incoming messages to Postman-emulated 'signal_controller'.


## Exercise 2B

NETPIE 2020 comes equipped with other nice features, e.g., feed, trigger and event-hook. You are encouraged to try these feature out. 


## Exercise 2C

Later on, in the next IoT Fun part that introduces IoT hardwares, you can try out coding in C for programming your hardware boards and devices to interact with NETPIE. There, it will be generally flexible on how you should be able to extend nice and practical features. For instance, so far, the signal controller is controlled **manually** at NETPIE dashboard. In practice, you can implement an **automatic feedback control** based on real-time inputs from 'queue_sensor' readings. You can also try to apply a machine learning model to forecast the sensor readings. You will learn more on this part after midterm exam.


For the time being, Have FUN experimenting on what you are capable of. 

You can refer to a standard NETPIE 2020 documentation at 

[https://docs.netpie.io/en/overview-netpie.html](https://docs.netpie.io/en/overview-netpie.html)

Check also the free quota you have at

[https://docs.netpie.io/en/quota-netpie.html](https://docs.netpie.io/en/quota-netpie.html)


---
Noted By: C. Aswakul (17 Jan 2023)

1stEd By: C. Aswakul (8 Jan 2025)
- Use Postman instead of MQTTbox 
- Redesign whole experimental context of sensor and controller 
- Elaborate in details for NETPIE dashboard experiment

