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
- [ ] not done
- [read-file-Node](#read-file-node)
- [ ] not done
- [watch-Node](#watch-node)
- [ ] not done

### Analysis
- [ ] not done

### Social
- [ ] not done

### Advanced
- [ ] not done

### IOBroker
- [ ] not done

### Operating System
- [ ] not done

### Polymer
- [ ] not done

### Dashboard
- [ ] not done

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
