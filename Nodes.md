Links to Node-Description:
### Common
- [Inject-Node](#inject-node)
- [Debug-Node](#debug-node)
- [Complete-Node](#complete-node)
- [Catch-Node](#catch-node)
- [Status-Node](#status-node)
- [Link-in-Node](#link-in-node)
- [Link-call-Node](#link-call-node)
- [Link-out-Node](#link-out-node)
- [Comment-Node](#comment-node)

### Function
- [Function-Node](#function-node)
- [Switch-Node](#switch-node)
- [Change-Node](#change-node)
- [Range-Node](#range-node)
- [Template-Node](#template-node)
- [Delay-Node](#delay-node)
- [Trigger-Node](#trigger-node)
- [Exec-Node](#exec-node)
- [Filter-Node](#filter-node)

### Network
- [Mqtt-in-Node](#Mqtt-in-Node)
- [Mqtt-out-Node](#Mqtt-out-Node)
- [Http-in-Node](#Http-in-Node)
- [Http-response-Node](#Http-response-Node)
- [Http-request-Node](#Http-request-Node)
- [Websocket-in-Node](#Websocket-in-Node)
- [Websocket-out-Node](#Websocket-out-Node)
- [Tcp-in-Node](#Tcp-in-Node)
- [Tcp-out-Node](#Tcp-out-Node)
- [Tcp-request-Node](#Tcp-request-Node)
- [Udp-in-Node](#Udp-in-Node)
- [Udp-out-Node](#Udp-out-Node)


### Sequence
- [Split-Node](#split-node)
- [Join-Node](#join-node)
- [Sort-Node](#sort-node)
- [Batch-Node](#batch-node)

### Parser
- [CSV-Node](#csv-node)
- [html-Node](csv-node)
- [json-Node](#csv-node)
- [xml-Node](#csv-node)
- [yaml-Node](#csv-node)


### Storage
- [write-file-Node](#write-file-node)
- [read-file-Node](#read-file-node)
- [watch-Node](#watch-node)


### Analysis
- [aggregator-Node](#aggregator-node)
- [sentiment-Node](#sentiment-node)

### Social
- [email MTA-Node](#Email-MTA-node)
- [Email(in)-Node](#Email-in-node)
- [Twitter-in-Node](#twitter-in-node)
- [email(out)-Node](#email-out-node)
- [Twitter-Out-Node](#twitter-out-node)


### Advanced
- [feedparser-Node](#feedparser-node)


### IOBroker
- [ioBroker-in-Node](#Iobroker-node)
- [iobroker-out-Node](#iobroker-out-node)
- [iobroker-get-Node](#iobroker-get-node)
- [iobroker-get-Object-Node](#Iobroker-get-Object-node)
- [IObroker-list-Node](#iobroker-list-node)

### Operating System
- [OS-Node](#OS-node)
- [Drives-Node](#Drives-node)
- [Uptime-Node](#Uptime-node)
- [CPUs-Node](#cpus-node)
- [Loadavg-Node](#aggregator-node)
- [Memory-Node](#Memory-node)
- [Networkintf-Node](#Networkintf-node)

### Dashboard
- [Button-Node](#button-node)
- Most of these work the same and are self explainatory when the visualization is understood.


## Common
### Inject-Node
![Inject-Node](Bilder/NodeRed/NRIN.jpg)

With this Node you can "inject" a specific message into the flow, this can be once, by clicking the fild left to the node when deployed, repeated, repeated in a specific timeframe or at a specific time.

![Inject-Node fields](Bilder/NodeRed/NRIN2.jpg)

You can define the payload but also other Objectfields can be added, at the bottom you can configurate if or when to repeat the injection.

### Debug-Node
![Debug-Node](Bilder/NodeRed/NRDN1t.jpg)

This node can write to the console on the right of the flow, or to the Debug-console. Here you can check if your Object are built right or if the Object has some mistakes in it.
![Debug-Node-fields](Bilder/NodeRed/NRDN2.jpg)

First you can decide which attribute or field should be printed, then where this should be printed.

### Complete-Node
![Complete-Node](Bilder/NodeRed/NRCN.jpg)

This Node triggers when another node completes it's operation.
No need for pictures or anything, just check the node that should have finished it's action to start this node.

### Catch-Node
![Complete-Node](Bilder/NodeRed/NRCAN.jpg)

Again no big need to explain a lot, it catches appearing errors, you can change which nodes errors should be caught and also if the flow should be forced to continue.

### Status-Node
![Status-Node](Bilder/NodeRed/NRSN.jpg)

With this node you can show specific status information like connected and such in the UI.
### Link-in-Node
![Link-In-Node](Bilder/NodeRed/NRLI.jpg)

This node, together with the Link-Out-node can transfer Data between flows via Node-Red. In the Interface you can see every Link-Out and in the link-out-nodes you can see all link-out-nodes, that's why it's important to give these names.

![Link-in-Interface](Bilder/NodeRed/NRLII.jpg)

### Link-call-Nodes
![Link-call-Node](Bilder/NodeRed/NRLC.jpg)

This can be used to quickly add a function into a flow with just a call. The node calls the Link-Out-Node which you picked and starts at the Link-In-Node of that subflow like this:
![Link-call-ex](Bilder/NodeRed/NRLCE.jpg)

### Link-out-Node
![Link-Out-Node](Bilder/NodeRed/NRLO.jpg)

Now this node is used to push data out of the flow into another flow with a link in Node.
### Comment-Node
![comment-Node](Bilder/NodeRed/NRCO.jpg)

It is what it says, a comment.

## function
### Function-Node
![function-Node](Bilder/NodeRed/NRFN.jpg)

In this Node you can write JS-Code. There are 4 Tabs: Setup for different Modules.  
On Start for whenever the Node gets called.  
On Message for the main code.  
On Stop for all Code that should be done when the Node is done.     
More about writing the code can be found here: [Function-Writing](https://nodered.org/docs/user-guide/writing-functions)
or [here](Function.md)

### Switch-Node
![Switch-Node](Bilder/NodeRed/NRSWN.jpg)

This node works like a Java-Switch-Case, so it looks at a value and decides by that what should happen next.
### Change-Node
![Change-Node](Bilder/NodeRed/NRCHN.jpg)

Most common use for this node is to set the Value of a variable in this scope or context (At least each flow for itself, Global stays global)
It can also be used to change the value, delete it or move it from on Variable to another.

### Range-Node
![Range-Node](Bilder/NodeRed/NRRN.jpg)

Works like a Map in c++, you can take a range of numbers and rescale it to another range. Lets say you have an array with number from 1 to 250, now you want to show it as percentage so you map the second range from 1 to 100.

### Template-Node
![Template-Node](Bilder/NodeRed/TN.jpg)  

Here can text be generated by using the Mustache templating language [More](https://mustache.github.io/mustache.5.html)

Output can be JSON, XAML or just plain text, also the template can be done with plain text

### Delay-Node
![Delay-Node](Bilder/NodeRed/NRDN.jpg)

Just like in Ardiuno Programming the flow can be delayed by a specific amount or just random.

### Trigger-Node
![trigger-Node](Bilder/NodeRed/NRTN.jpg)

This Node triggers a send-CMD then it can wait for/Wait to be reset/resent it every x Timeunit.
Then can send something again as well as being reset if a specific payload comes in or the msg.reset is set.
The first timer can also be extended if a new message arrives or if msg.delay get overridden.

### Exec-Node
![Exec-Node](Bilder/NodeRed/NREN.jpg)

With this node Commands can be set for the CMD-Line, if more than one line is needed create a Script file and run this with the command, also Python skripts can be used.
The Output can be taken after the command ran or while it runs.
It has 3 Output "points", 1. is for standard output, Output+Return code   
Second Output is for standart Error, Error + Return code
Third Output is for the returning Code

### Filter-Node
![Filter-Node](Bilder/NodeRed/NRFIN.jpg)

This Node can be used to update a value only if it's <,> or != the value it was before. Good for not overwriting the same value over and over.

## Network
### Mqtt-in-Node
![Mqtt-in-Node](Bilder/NodeRed/MQIN.jpg)

Not much to say, it does what it says.

### Mqtt-out-Node
![Mqtt-out-Node](Bilder/NodeRed/MQO.jpg)

<Span style="color:red;font-size:50px;">↑</Span> Like  the in-Node

### Http-in-Node
![Http-In-Node](Bilder/NodeRed/HTI.jpg)

With this node you can either send a POST/GET/PUT/DELETE/PATCH to a specific URL inorder to configure the web server.

### Http-response-Node
![Http-response-Node](Bilder/NodeRed/HTR.jpg)

The HTTP Response node sends responses back to requests received from the HTTP Input node.

### Http-request-Node
![Http-Request-Node](Bilder/NodeRed/HTRE.jpg)

Same as the Http-In Node but for APIs and stuff.

### Websocket-in-Node
![Websocket-in-Node](Bilder/NodeRed/WIN.jpg)

Connector for Websockets

### Websocket-Out-Node
![Websocket-out-Node](Bilder/NodeRed/WON.jpg)

Connector for Websockets

### TCP-in-Node
![TCP-IN-Node](Bilder/NodeRed/TIN.jpg)

Connector for TCP communication

### TCP-out-Node
![TCP-Out-Node](Bilder/NodeRed/TOU.jpg)

Connector for TCP communication

### TCP-Resquest-Node
![TCP-Request-Node](Bilder/NodeRed/TRE.jpg)

Connector for TCP communication

### UDP-In-Node
![UDP-In-Node](Bilder/NodeRed/UIN.jpg)

Connector for UDP communication

### UDP-Out-Node
![UDP-Out-Node](Bilder/NodeRed/UOU.jpg)

Connector for UDP communication

## Sequence

### Split-Node
![Split-Node](Bilder/NodeRed/SPL.jpg)

This Node can be used to Split the msg.payload into smaller fields of the msg Object, it can Split String/Buffer, Arrays or Objects.

### Join-Node
![Join-Node](Bilder/NodeRed/JOI.jpg)

This node can be used to put two incoming msg.payload into one field of the msg Object.

### Sort-Node
![Sort-Node](Bilder/NodeRed/SOR.jpg)

This node can be used to sort a specific field of msg or any variable in the Flow (Global and Flow)

### Batch-Node
![Batch-Node](Bilder/NodeRed/BAT.jpg)

This node can be used to create Messagesequences with specific rules. It has 3 different modes which are:
- Group by number of messages
> The Sequence is defined by a specific number of incoming messages. These can overlap, the overlap can also be configureated
- Group by time interval
> The Sequence is defined by a timespan in seconds, it can be configured if the message should also be send empty if no new messages came in
- Concatenate Sequences
> The node stores a amount of messages in a puffer, the length of this can be defined in the nodeMessageBufferMaxLength.
If the Message comes with a set msg.reset property the message gets deleted and not sent.

## Parser
### CSV-Node
![CSV-Node](Bilder/NodeRed/CSV.jpg)

Each parser nodes work about the same but with different attributes to configure. When you look into the interface of the node there is everything you need.


## Storage
### Write-file-Node
![Write-File-Node](Bilder/NodeRed/WFI.jpg)

Can be used to save stuff into a file like a csv file.

### Read-File-Node
![Read-File-Node](Bilder/NodeRed/RFI.jpg)

With this a file created manualy or with the Write-file-Node can be read again and put back into the flow. another way of moving data from flow to flow or from adapter to adapter.

### Watch-Node
![Watch-Node](Bilder/NodeRed/WAT.jpg)

This Node works like a observer, if the file gets changed it looks at these changes. The name of the changes data gets put into msg.payload and msg.filename. the Observerlist is in msg.topic. The typ of the changing file gets saved into msg.type and die size into msg.size.

The file that should be watched has to exist, deleting and recreating does not work with the dependencies.

## Analysis
### Aggregator-Node
![Aggregator-Node](Bilder/NodeRed/AGG.jpg)

It Aggregates msg.payloads that come in in a specific timeframe. The Aggregationmethod can be:
- Average
- geometric mean Value
- harmonic mean Value
- median Value
- Minimum
- Maximum
- Sum of Values

### Sentimen-Node
![Sentiment-Node](Bilder/NodeRed/SEN.jpg)

Can be used to analyse language. More infos on the method behind the node: [Click!](https://github.com/thisandagain/sentiment/blob/master/README.md).

## Social
### Email MTA Node
![Email-MTA-Node](Bilder/NodeRed/EMM.jpg)

This node is to Text emailing with the Mail Transfer Agent, only dor testing as not really safe.


### Email (in) Node
![Email (in) Node](Bilder/NodeRed/EMI.jpg)

This node checks reguraly for emails in a POP3 or IMAP server and redirects these if not shown before.
Subjecttext can be found in msg.Topic, the emailtext can be found under msg.payload. More infos are in the help-tab when using it in NodeRed.

### Twitter in Node
![Twitter in Node](Bilder/NodeRed/TWI.jpg)

This Node can be used to search for tweet on diffrent levels, help can be found in NodeRed for this Node.

### Email (out) Node
![Email (out) Node](Bilder/NodeRed/EMO.jpg)

This Node can send Emails. More in the Help Tab of NodeRed.

### Twitter out Node
![Twitter out Node](Bilder/NodeRed/TWO.jpg)
Send tweets and direct messages. More in the help tab.

## Advanced
### Feedparser Node
![Feedparser Node](Bilder/NodeRed/FEE.jpg)
Observes a RSS/atom-Feed for changes. More info in the Help table.

## ioBroker
### Iobroker-in-node
![Iobroker-in Node](Bilder/NodeRed/IOI.jpg)

With this Node variable from IOBroker can be used in NodeRed as well as monitiored (difference to the IOBroker get node)

### Iobroker-out-node
![Iobroker-out Node](Bilder/NodeRed/IOO.jpg)

With this Node variable from IOBroker can be overwritten.

### Iobroker-Get-node
![Iobroker get Node](Bilder/NodeRed/IOG.jpg)

With this Node variable from IOBroker can be input into the flow.

### Iobroker-Get-object-node
![Iobroker get object Node](Bilder/NodeRed/IOGO.jpg)

With this Node variable from IOBroker can be input into the flow as the msg.payload field.

### Iobroker-Get-list-node
![Iobroker get list Node](Bilder/NodeRed/IOGL.jpg)

With this Node variable from IOBroker can be input into the flow, for IDs.

## Operating System
### OS-Node
![OS Node](Bilder/NodeRed/OS.jpg)

This Node returns:
- Hostname of the operating System
- the OS name
- OS CPU Architecture
- OS release
- Endian of the CPU

### Drives-Node
![Drives Node](Bilder/NodeRed/DRI.jpg)

This Node returns:
- values for size in KB
- value for capazyity between 0 and 1, *100 is the percentage that is full.

### Uptime-Node
![Uptime Node](Bilder/NodeRed/UPT.jpg)

Returns the uptime in seconds.

### CPUs-Node
![CPUs Node](Bilder/NodeRed/CPU.jpg)

Return an Array of Object, each object contains:
- model
- speed (MHz)
- times spend in user, nice, sys, idle, irq in millisec

for each CPU

### Loadavg-Node
![Loadavg Node](Bilder/NodeRed/LOA.jpg)

Returns an array with the 1, 5 and 15 minute load averages.
>The load average is a measure of system activity, calculated by the operating system and expressed as a fractional number. As a rule of thumb, the load average should ideally be less than the number of logical CPUs in the system.

>The load average is a very UNIX-y concept; there is no real equivalent on Windows platforms. That is why this node always returns [0, 0, 0] on Windows.

### Memory-Node
![Memory Node](Bilder/NodeRed/MEM.jpg)

Returns:
- total amount of systen memory in bytes, kilobyte, megabytes or gigabyte
- amount of free system memory in bytes, kilobytes, megabytes or gigabytes
- Memory in use in percentage

### Networkintf-Node
![Networkintf Node](Bilder/NodeRed/NET.jpg)

Return Network interfaces that have an address assigned.

## Dashboard
### Button-Node
![Button Node](Bilder/NodeRed/BUT.jpg)
With this node you can show a button on the dashboard which can send a msg.payload on press or msg receive. More in how to place dashboard nodes can be found in the visualisation.
