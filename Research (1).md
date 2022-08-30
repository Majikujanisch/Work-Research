
# Research
## table of content
- [Good to Know](##good%to%know)
- [First Steps](#First%Steps)
- [Problems](#Problems)
- [Node-Red](#Node-Red)
## Good to Know
### KNX
It's a well known facility bus. It knows Aktors (like little bridges that give the power to the light/mic/etc. which react to telegrams that come via the second connection of the bus:
~~~
            |¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯Datenübertragung (Telegram, von Sensor o. PC)
     Ø-----|Aktor|----Strom-----
            |________________       
~~~
With one Bus you can have 15 "Floors", 15 "Rooms" or "Lines" and 255 Users.
### Packet Strucktur (German):

>     Oktet aus Kontrollbyte mit Priorisierung (00 Systemfunktion, 10 Alarmfunktion, 01 Hohe Prio, 11 norm Prio)
>     2 Byte für die Quelladresse (<xxxx><xxxx><xxxxxxxx>, <Bereich><Linie><Teilnehmer>)
>     2 Byte für die Zieladresse, Im ersten Bit steht drin ob unicast oder Multicast, die nächsten 4 sind für die Hauptgruppe. 3 Bit für die Mittelgruppe und wieder 4 für die Untergruppe.
>     1 Byte für das DRL-Byte (Destination-adress-flag, Routing-Counter, length), erstes Bit ist für die physische oder Gruppenadresse, 3 Bit werden als Counter genutzt, ist dieser über 7 wird das Paket
>     beliebig oft weitergeschickt, ist dieser 6 wird das Packet bei jedem Linien- und Bereichkoppler dekrementiert, bei 0 Wird dieses verworfen
>     4 Bit der DRL werden für die Länge -2 verwendet (in Bytes)
>     Byte 6 und 7 sind für Transport Protokole und die Application Protokol(DPT oder EIS) sowie ein Teil der Daten
>     Byte 8...n-1 sind die Daten
>     der letze Byte beinhaltet die Checksum (invertiertes, Bitweise XOR-Verknüpfung aller vorher gesendeter Pakete)
### IO-Broker
IOBroker is like a bridge between the diffrent scripts, busses, users, etc. It has adapter for various features such as KNX, Node-Red, Blockly, VIS, Weatherstations, various personal Assistants (Alexa).
It also has an own "datacoordination" so you can capsulate each adapter and don't have to juggle with variables between adapters.


## First Steps
### Download IOBroker
IOBroker can be installed to Windows via installationwizard found here: [Wizard](https://www.iobroker.net/?page_id=4284&lang=de#de/download)

For Docker and RASP PI installations wizard can also be found on the webside above.

### Update IOBroker instance
After the download you have a IOBroker instance with an depricated Admin, NodeJS, Controller and other adapter. To download the KNX-Adapter we need a newer version of the JS-Controller, in order to do so we need to upgrade NodeJS first.
First we need to stop the current IOBroker instance in the IOBroker CMD. This CMD can be opened vie the searchbar. Just type IOBroker and choose IOBroker(Instancename) CMD.
To stop you type:
`Iobroker stop`
2022-08-29 08:39:17,058 INFO  - Stopping the service with id 'iobroker(test)' **right output**

Now we can download a new version of NodeJS, preferable 16.x [link to NodeJS](https://nodejs.org/en/download/)
[link to test](Test)

Now we change the name of the current NodeJS in the IOBroker instance directory to "NodeJSBackUp" or anything else. After that you can insert the just downloaded NodeJS. The most important step now is to coüy the nodevars.bat from the NodeJSBackUp into the new NodeJS folder. This file is for the IOBroker CMD commands.
Now we have to run the installation wizard again and choose the secon option and "repair" the instance. The File wanted can be found under /IOBroker/instanceName/iobroker-data

In order to update the adapter via Webinterface we have to Update the JS-Controller in the IOBroker CMD.
First try IObroker Upgrade self, if it's not working try `npm install iobroker.js-controller@3.2`.
Then try `IOBroker Upgrade Self` again
To again just type `Iobroker start`
2022-08-29 09:05:07,156 INFO  - Starting the service with id 'iobroker(test)' **Right Output**

Now you can update all other Adapter via the Webinterface.

# Problems
### IOBroker CMD Commands not Working
Most times this is the result of a missing nodevars.bat after updating NodeJS. This could be found in the BackUp-folder of NodeJS or in a freshly set up IOBroker instance. Even if it's working after pasting the nodevars.bat into the NodeJS-Folder you should rerun the Installer on the second option.![Installationoptionen](installer.jpg)

# Node-Red
## Whats it good for?
With Node-Red you can theoratically schedule tasks but also build an interface which then starts tasks. It's flow based as well as Webbased.
## Nodes and Stuff
What are Nodes? - Nodes are everything you can connect with wires, or everything that brings something into the flow, does something or brings something out of the flow.

These Nodes can be connected which basically tells the data-object where to go next. The standard name for this object is msg, if you want to send one or more data to next node you use the msg.payload field.

- [Inject-Node](###Inject-Node)
### Inject-Node
![Inject-Node](NRIN.jpg)
With this Node you can "inject" a specific message into the flow, this can be once, by clicking the fild left to the node when deployed, repeated, repeated in a specific timeframe or at a specific time.
![Inject-Node fields](NRIN2.jpg)
You can define the payload but also other Objectfields can be added, at the bottom you can configurate if or when to repeat the injection.
## First Example - multiplication of two variable put in by the User
This is a good example to analyze the variable concept of Node-Red as well as a bit of Visualization via Node-Red.
![components](NRE1.jpg)

# TODO
- [ ] nodes erklären
- [ ] Visualisierung mit groups usw erklären
- [ ] Dateiverkehr zwischen node
- [ ] Dateiverkehr zwischen Adaptern
Ctrl+Shift+M
