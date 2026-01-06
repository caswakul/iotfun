
# NETPIE2020-Experiment1withPostman


>Training material for 2102541 IoT Fundamentals Class at Department of Electrical Engineering, Faculty of Engineering, Chulalongkorn University, Thailand.
>
>Aim: To introduce newcomers to NETPIE2020 with (i) device and group creation, (ii) publish/subscribe of text messages from POSTMAN to emulate of NETPIE devices.


## References

- Piyawat Jomsathan, Online Training: IoT & NETPIE 2020, [https://netpie.io/guide](https://netpie.io/guide) (in Thai language)
- Example Projects of NETPIE, [https://netpie.io/exampleprojects/en](https://netpie.io/exampleprojects/en)


## Let's Start

1. Sign-up and log-in with your username and password at netpie.io
![NETPIE2020-Experiment1-assets/Pasted image 20230116225106.png](NETPIE2020-Experiment1-assets/Pasted%20image%2020230116225106.png)

Here, I enter my email.  You have to use your own email.
![NETPIE2020-Experiment1-assets/Pasted image 20230116225329.png](NETPIE2020-Experiment1-assets/Pasted%20image%2020230116225329.png)

2. Once in NETPIE web, create your own project and enter your project description.
![2createproject.png](NETPIE2020-Experiment1and2withPostman/2createproject.png)

As a new project, it contains no devices and groups yet.
![2createprojectresult.png](NETPIE2020-Experiment1and2withPostman/2createprojectresult.png)

3. Devices within the same group can communicate with each other. So, let us enter menu 'Group' and create new groups. You can name and add description to your new groups. Here, I create 2 groups, named 'myoffice' and 'myhome'. 
![3creategroupMyoffice.png](NETPIE2020-Experiment1and2withPostman/3creategroupMyoffice.png)
![3creategroupMyhome.png](NETPIE2020-Experiment1and2withPostman/3creategroupMyhome.png)
![3creategroupsResult.png](NETPIE2020-Experiment1and2withPostman/3creategroupsResult.png)

4. Enter menu 'Devices'. Create devices. Here, I create 4 devices, 'homeDev1' and 'homeDev2' in group 'myhome'; and  'officeDev1' and 'officeDev2' in group 'myoffice'. For example, for 'homeDev1' in group 'myhome':
![4createdeviceHomedev1.png](NETPIE2020-Experiment1and2withPostman/4createdeviceHomedev1.png)

Completing all 4 devices, you get the following device list:
![4createdevicesResult.png](NETPIE2020-Experiment1and2withPostman/4createdevicesResult.png)

 NETPIE creates for each of your devices the client ID, token, secret. You will need to use these keys to authenticate your devices to ask for an authorised access into NETPIE. For instance, my 'homeDev1' gets assigned the following key:
![4createdeviceHomedev1result.png](NETPIE2020-Experiment1and2withPostman/4createdeviceHomedev1result.png)
 
You can click 'Copy' at each of the key values, to copy the string, which will be used later on with your code or program that implements that device.

5. Use Postman to emulate NETPIE devices. First, download and install Postman. For instance, here I have installed Postman into my computer. 
![5postmandownload.png](NETPIE2020-Experiment1and2withPostman/5postmandownload.png)

Once launching Postman, we get:
![5postmanLaunched.png](NETPIE2020-Experiment1and2withPostman/5postmanLaunched.png)

6. In Postman, add a new MQTT request for 'homeDev1'. 

Click 'New' and 'MQTT':
![6_1newmqtt.png](NETPIE2020-Experiment1and2withPostman/6_1newmqtt.png)

Configure the request to emulate 'homeDev1':
![6_2configHomeDev1.png](NETPIE2020-Experiment1and2withPostman/6_2configHomeDev1.png)

The following input values must be entered:

==MQTT Request Input Values for 'homeDev1' ==

| ==MQTT CLIENT SETTINGS==  | ==INPUT VALUES==                |
| -------------------- | ------------------------------------ |
| MQTT Version         | V3 (3.1.1)                           |
| Host URL (of NETPIE) | broker.netpie.io                     |
| Authorization        | Basic Auth                           |
| MQTT Client Id       | 'Client ID' of 'homeDev1' in NETPIE  |
| User Name            | 'token' of 'homeDev1' in NETPIE      |
*NB: other settings are default*

Click 'Save'. Enter 'Request name' as 'homeDev1' and 'Collection name' as 'MQTT-NETPIE'. Of course, these names are any strings, but they should be meaningfully matched with what we name in NETPIE:
![6_3saveHomeDev1.png](NETPIE2020-Experiment1and2withPostman/6_3saveHomeDev1.png)

Now, if successfully done, then the right pane should display a new collection 'MQTT-NETPIE' with request 'homeDev1'. Click 'Connect' to make a connection from Postman to NETPIE for 'homeDev1' device:
![6_4_connectHomeDev1.png](NETPIE2020-Experiment1and2withPostman/6_4_connectHomeDev1.png)

If all configurations are correct, then Postman should display status 'Connected' for 'homeDev1' device in both Postman and NETPIE:
![6_5HomeDev1ResultConnected.png](NETPIE2020-Experiment1and2withPostman/6_5HomeDev1ResultConnected.png)
![6_6HomeDev1ResultConnectedShown.png](NETPIE2020-Experiment1and2withPostman/6_6HomeDev1ResultConnectedShown.png)


7. Now we have one MQTT client 'homeDev1' already connected successfully with NETPIE. 
![7HomeDev1ConnectedOthersNotyet.png](NETPIE2020-Experiment1and2withPostman/7HomeDev1ConnectedOthersNotyet.png)

Repeat step 6 to add the remaining 3 devices, i.e., 'homeDev2', 'officeDev1' and 'officeDev2'. Once successfully configured and connected, you should see all the 4 devices with status 'Online' in NETPIE and 'Connected' in Postman.
![7allConnected.png](NETPIE2020-Experiment1and2withPostman/7allConnected.png)
![7allConnected2.png](NETPIE2020-Experiment1and2withPostman/7allConnected2.png)

**Note that, in practice, we will have to configure all these devices not in Postman, but rather in the real hardware devices. But for convenience, in this stage, we will use Postman as a convenient device emulator.**


8. Let us try to have these devices send messages to NETPIE. By MQTT, recall that we use the MQTT publish/subscribe communication model. Suppose we want our emulated client 'homeDev1' in Postman to subscribe to a topic, say 'homeAnnouncements' at the MQTT broker service of NETPIE. We can enter the topic text with prefix '@msg/' defined by the NETPIE2020 convention. This prefix is intended in NETPIE for *text* message communications.
![8homeDev1sub.png](NETPIE2020-Experiment1and2withPostman/8homeDev1sub.png)

Suppose 'homeDev1' wants to publish a message to topic 'homeAnnouncements' as well. You can enter:
![8homeDev1sendMsgA.png](NETPIE2020-Experiment1and2withPostman/8homeDev1sendMsgA.png)

You can see the message payload "message A from homeDev1 to homeAnnouncements" as merely a text string, being sent out and then received by 'homeDev1' in Postman.

However, note that you should not see any message being received by 'homeDev2' in Postman:
![8homeDev2notRecvMsgA.png](NETPIE2020-Experiment1and2withPostman/8homeDev2notRecvMsgA.png)
This is because 'homeDev2' has not yet subscribed to the topic 'homeAnnouncements'.


9. Try now to have 'homeDev2' in Postman subsribe to the topic 'homeAnnouncements':
![9homeDev2sub.png](NETPIE2020-Experiment1and2withPostman/9homeDev2sub.png)

Resend another message with payload "message B from homeDev1 to homeAnnouncements" to  the topic 'homeAnnouncements'. Now, both 'homeDev1' and 'homeDev2' should receive this new message:
![9homeDev1sendMsgB.png](NETPIE2020-Experiment1and2withPostman/9homeDev1sendMsgB.png)
![9homeDev2recvMsgB.png](NETPIE2020-Experiment1and2withPostman/9homeDev2recvMsgB.png)


10. A good thing about NETPIE2020 is its centralised managability of created devices. Suppose you want to disallow 'homeDev1' from NETPIE. You can click to toggle from 'enable' to 'disable':
![10.disableHomeDev1.png](NETPIE2020-Experiment1and2withPostman/10.disableHomeDev1.png)

And we now get from NETPIE the 'Offline' status for this device:
![10.disableHomeDev1result.png](NETPIE2020-Experiment1and2withPostman/10.disableHomeDev1result.png)

And in Postman, we notice 'Disconnected' status for 'homeDev1'. If we persist in trying to publish a message again during this 'Disconnected' status for 'homeDev1' from Postman, then we get error messages:
![10.disableHomeDev1cannotSendMsgC.png](NETPIE2020-Experiment1and2withPostman/10.disableHomeDev1cannotSendMsgC.png)

Likewise, a subscribe attempt would also fail:
![10homeDev1subFail.png](NETPIE2020-Experiment1and2withPostman/10homeDev1subFail.png)

From NETPIE, try 'enable' this device once more. And check that publish/subscribe of this device can resume.


---

## Excercise 1A

Recall that we have initially created 4 devices in 2 groups. And we have demonstrated sending messages from only one device 'homeDev1' so far. In a general case, can you try to verify that devices within the **same** group can communicate via pub/sub with each other?


## Exercise 1B

Likewise, can you try to verify that devices from **different** groups cannot communicate via pub/sub with each other?  For instance, you can configure device 'officeDev1' to subscribe to the topic 'homeAnnouncements'. Then you can test whether 'officeDev1' gets the messages that are published to this topic by 'homeDev1'.


## Exercise 1C

Notice that there are other options in MQTT Client Settings that we have not used yet e.g. last-will and retain. Can you try to design your own experiments to test the functionality of those MQTT features. Which of these feautures are supported by NETPIE2020 (and which features are not supported)?


## Exercise 1D

Finally, this is a bit challenging experimental design. Can you try to test the functionality of 'QoS' or 'Quality of Service' settings in MQTT. It can be such a challenge because you might not be easily able to control how messages are lost during the transmission. Use load generator?  Use Wireshark?  Any idea here?  And do not be worried. This exercise 1D will not be hands-on in-class midterm examination.



---
Noted By: C. Aswakul (16 Jan 2023)

1st Edited By: C. Aswakul (6 Jan 2025): Use Postman instead of MQTTbox 
