
# NETPIE2020-Experiment2


>Training material for 2102541 IoT Fundamentals Class at Department of Electrical Engineering, Faculty of Engineering, Chulalongkorn University, Thailand.
>
>Aim: To introduce newcomers to NETPIE2020 with (i) device schema (ii) publish/subscribe of 'data'  from MQTTbox emulator of NETPIE devices (iii) dashboard.


## References

- Piyawat Jomsathan, Online Training: IoT & NETPIE 2020, [https://netpie.io/guide](https://netpie.io/guide) (in Thai language)
- Example Projects of NETPIE, [https://netpie.io/exampleprojects/en](https://netpie.io/exampleprojects/en)


## Let's Continue

1. Sign-up and log-in with your username and password at netpie.io. Once in NETPIE web, create your own project and enter your project description. Here, we create project 'smartMobility2022' for intelligent transportation system applications. 

![NETPIE2020-Experiment2-assets/Pasted image 20230117182733.png](NETPIE2020-Experiment2-assets/Pasted%20image%2020230117182733.png)

![NETPIE2020-Experiment2-assets/Pasted image 20230117182806.png](NETPIE2020-Experiment2-assets/Pasted%20image%2020230117182806.png)


2. Enter menu 'Group' and create a new group. You can name and add description to your new groups. Here, I create 1 group, named 'rama4road' to keep our sensors installed on Rama-4 road network around Chula. 

![NETPIE2020-Experiment2-assets/Pasted image 20230117183155.png](NETPIE2020-Experiment2-assets/Pasted%20image%2020230117183155.png)

![NETPIE2020-Experiment2-assets/Pasted image 20230117183225.png](NETPIE2020-Experiment2-assets/Pasted%20image%2020230117183225.png)


3. Enter menu 'Devices'. Create devices. Here, we will create 3 devices, 'register', 'sensor' and 'controller' in this group. 
- *register* is responsible for keeping tracks of last-will messages published by all other devices when their status values are changed from online (connected) to offline (disconnected). 
- *sensor* is an example sensor that publishes **data** to NETPIE.
- *controller* is an example controller that gets data from NETPIE and takes corresponding necessary control actions, e.g., changing traffic signal light phase number (to specify which vehicle flows would be allowed green light at a road junction).


4. **Create Register**

In NETPIE, we create *register* as follows.
![NETPIE2020-Experiment2-assets/Pasted image 20230117184744.png](NETPIE2020-Experiment2-assets/Pasted%20image%2020230117184744.png)

In MQTTBox, we create *register* as follows. After the *register* is connected to NETPIE, we configure *register* to subscribe to topic '@msg/rama4DMlogs'. This topic name is meant here for device management (DM) logs where last-will messages from sensor and controller devices would be published. Of course, the topic can be any string meaningful to your own project, but the string must start with '@msg/' so NETPIE knows the messages published here are 'text'.

![NETPIE2020-Experiment2-assets/Pasted image 20230117185257.png](NETPIE2020-Experiment2-assets/Pasted%20image%2020230117185257.png)
![NETPIE2020-Experiment2-assets/Pasted image 20230117185537.png](NETPIE2020-Experiment2-assets/Pasted%20image%2020230117185537.png)


5. **Create Sensor in NETPIE**

In NETPIE, we create *sensor* as follows.
![NETPIE2020-Experiment2-assets/Pasted image 20230117190409.png](NETPIE2020-Experiment2-assets/Pasted%20image%2020230117190409.png)

Since *sensor* sends 'data' that would not be only 'text', in NETPIE, we will define 'Schema' for the expected structure of incoming data payload. Click 'Schema' and switch from the Editor Mode 'Tree' to 'Code'. 

![NETPIE2020-Experiment2-assets/Pasted image 20230117190521.png](NETPIE2020-Experiment2-assets/Pasted%20image%2020230117190521.png)

![NETPIE2020-Experiment2-assets/Pasted image 20230117191125.png](NETPIE2020-Experiment2-assets/Pasted%20image%2020230117191125.png)

Device schema format must be in **JSON (JavaScript Object Notation)**. While there are many websites trying to describe JSON format, I personally find the following **original JSON inventor**'s post at [https://www.json.org](https://www.json.org) by *Douglas Crockford* the most concise and programmably meaningful. Check it out at your convenience. But, for the time being, we will define the schema of *sensor* as follows. And thanks to the readability of JSON, we can try here to explain the intended meaning.

```json
{
  "additionalProperties": false,
  "properties": {
    "vehicle_speed": {
      "operation": {
        "store": {
          "ttl": "7d"
        }
      },
      "type": "number"
    },
    "vehicle_type": {
      "operation": {
        "store": {
          "ttl": "7d"
        }
      },
      "type": "string"
    }
  }
}
```

This sensor may represent a CCTV-AI (video camera with digital signal processing software). The software can keep track of the vehicle movement from the camera. The software can detect the vehicle speed in km/hr, which is a *number*. Based on the detectable shape and size of vehicle, the software can classify the vehicle as, e.g., either 'car' or 'motorcycle', which is a *string*. These "vehicle_speed" and "vehicle_type" form the main "properties" that we expect of the data from this sensor.  Once NETPIE gets the data, NETPIE performs the operation to store the value in NETPIE for 7 days, as configured by "ttl" or *time to live* equal to "7d".

Note that "additionalProperties" is here set to *false*. So, if *sensor* tries to send any other data values, in addition to "vehicle_speed" and "vehicle_type", then NETPIE will neglect those other data values and keep only the data values specified by "vehicle_speed" and "vehicle_type". 

Otherwise, if "additionalProperties" is set to *true*, then NETPIE will keep all other received data values as well. With that said, to prevent unnecessary confusion on stored data values, *sensor* is expected reasonably to send only data values whose syntax is well defined in the schema. That is the purpose of having the schema after all.

Copy the JSON text and paste into the code box defining schema. Click 'SAVE'.

![NETPIE2020-Experiment2-assets/Pasted image 20230117201354.png](NETPIE2020-Experiment2-assets/Pasted%20image%2020230117201354.png)

Try switching from 'Code' to 'Tree'. As a tree, we can naturally try clicking to hide or unhide JSON elements:

![NETPIE2020-Experiment2-assets/Pasted image 20230117201626.png](NETPIE2020-Experiment2-assets/Pasted%20image%2020230117201626.png)


6.  **Create Sensor in MQTTBox**

In MQTTBox, we create *sensor* as follows. 

![NETPIE2020-Experiment2-assets/Pasted image 20230117202642.png](NETPIE2020-Experiment2-assets/Pasted%20image%2020230117202642.png)

Note that we configure *sensor* last-will message 'sensor is dying' to be published to topic '@msg/rama4DMlogs'. 

Specify the topic '@shadow/data/update' for this *sensor* to publish its *data* message. Try publishing the first message from this sensor as follows. 

```JSON
{
  "data": {
    "vehicle_speed": 50,
    "vehicle_type": "car"
  }
}
```

![NETPIE2020-Experiment2-assets/Pasted image 20230117203948.png](NETPIE2020-Experiment2-assets/Pasted%20image%2020230117203948.png)

If your *data* message arrives successfully at NETPIE, then you should be able to see the latest data values shown in 'Shadow' of *sensor* in NETPIE:

![NETPIE2020-Experiment2-assets/Pasted image 20230117204143.png](NETPIE2020-Experiment2-assets/Pasted%20image%2020230117204143.png)

This special topic '@shadow/data/update' in NETPIE is used to store the latest received data values from the *sensor*. If a new data message is published by the *sensor* and that message arrives at NETPIE, then the 'Shadow' data values of this *sensor* will be updated correspondingly. You can try this out by yourself by re-adjusting the data message payload, publish the message out, and check what NETPIE stores at the 'Shadow' of *sensor*.


7. **NETPIE Dashboard**

Finally, create a NETPIE dashboard for GUI displays.

![NETPIE2020-Experiment2-assets/Pasted image 20230117204617.png](NETPIE2020-Experiment2-assets/Pasted%20image%2020230117204617.png)

After entering a dashboard name and description, click 'SAVE'. Then we get:
![NETPIE2020-Experiment2-assets/Pasted image 20230117204738.png](NETPIE2020-Experiment2-assets/Pasted%20image%2020230117204738.png)

Click to enter the dashboard.

We have *sensor* sending its latest data values to store into its 'Shadow' with NETPIE. Next, so, click menu 'Setting', click 'Add device' to add this *sensor* device as a data source for the dashboard. And specify the priviledges as 'read shadow'. 

![NETPIE2020-Experiment2-assets/Pasted image 20230117212555.png](NETPIE2020-Experiment2-assets/Pasted%20image%2020230117212555.png)

Click 'SAVE' and we get:

![NETPIE2020-Experiment2-assets/Pasted image 20230117212632.png](NETPIE2020-Experiment2-assets/Pasted%20image%2020230117212632.png)

Click menu 'Dashboard' and click '+ Add panel'. Click the plus '+' sign of the panel:

![NETPIE2020-Experiment2-assets/Pasted image 20230117212959.png](NETPIE2020-Experiment2-assets/Pasted%20image%2020230117212959.png)

Configure what to display in this panel. Select appropriate 'TYPE' according to the data type to be displayed naturally. Enter all other required input for the widget. Note for VALUE input, click '+DEVICE' and click subsequently any needed drop-down information to dive into the value to be displayed by the widget. Here, as an example, the inputs are for displaying "vehicle_type" as text in the widget.

![NETPIE2020-Experiment2-assets/Pasted image 20230117213139.png](NETPIE2020-Experiment2-assets/Pasted%20image%2020230117213139.png)

![NETPIE2020-Experiment2-assets/Pasted image 20230117213522.png](NETPIE2020-Experiment2-assets/Pasted%20image%2020230117213522.png)

Click 'SAVE' to finish the widget configuration. Then, we can see:

![NETPIE2020-Experiment2-assets/Pasted image 20230117213623.png](NETPIE2020-Experiment2-assets/Pasted%20image%2020230117213623.png)

Likewise, add another panel and configure to display "vehicle_speed". 

![NETPIE2020-Experiment2-assets/Pasted image 20230117213743.png](NETPIE2020-Experiment2-assets/Pasted%20image%2020230117213743.png)

Finally, the dashboard shows the latest shadow data values of *sensor*. You can try again publishing a new data message with different values for "vehicle_speed" and "vehcle_type", as well as notice the change update at the created dashboard here.

![NETPIE2020-Experiment2-assets/Pasted image 20230117213827.png](NETPIE2020-Experiment2-assets/Pasted%20image%2020230117213827.png)


---

## Excercise 2A

Recall that we have initially created 3 devices here, i.e., *sensor*, *register*, and *controller*. And we have demonstrated above how only one device can send its data values into NETPIE, which can then be displayed in the dashboard.

You are encouraged to design your own experiment that involves the meaningful usage scenario of all these 3 devices ?


## Exercise 2B

Notice that we can configure systematically last-will messages of created devices. Here, can you try to test again the device registry functionality in keeping track of device status changes. 


## Exercise 2C

NETPIE 2020 comes equipped with other nice features, e.g., feed, trigger and event-hook. You are encouraged to try these feature out. And also, later in the next IoT Fun part, introducing IoT hardwares, you can try out coding in C, etc., for programming your hardware boards and devices to interact with NETPIE. 

You can refer to a standard NETPIE 2020 documentation at 

[https://docs.netpie.io/en/overview-netpie.html](https://docs.netpie.io/en/overview-netpie.html)

Check also the free quota you have at

[https://docs.netpie.io/en/quota-netpie.html](https://docs.netpie.io/en/quota-netpie.html)

Have FUN!! 



---
Noted By: C. Aswakul (17 Jan 2023)