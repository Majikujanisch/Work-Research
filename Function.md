[Zurück](Research.md)
# Functions

## Writing functions
With the function Node you can write JS Code in NodeRed. Important to know is that most of the time you will read and write into the msg.payload property.
Msg is the main Object going around in the Flow. How Flow and the context flow of NodeRed works has its own chapter [here](Flow.md)
Whenever you add a property to the msg Object, document what it is and for what, it's easy to forget and it could happen a lot.

Almost each function will have this line:
~~~javascript
return msg;
~~~
As seen we always return the complete Object, NodeRed can't send something different than Object, everthing else will give an error, but you can send out another Object than msg.

Constructing a new Object will lose any Information of the msg Object, so always let it run along and give out more than one output.

For debugging purpose ypu can use
~~~javascript
node.warn("Example var= "+ variable);
~~~
But there are more than one way of logging, more in logging.

## Multiple Outputs

I just told you why you should give multiple Outputs (You could also use context storage, but when in Function this is cleaner). This is done by giving out an Array (return [msg, secout];)

## Asynchronus msg sending
To send while the Node is running use node.send(msg)
After you are done sending use node.done() otherwise the node will not know when sending is done.

## logging
~~~javascript
node.log("xxx");
node.warn("yyy");
node.error("zzz");
~~~
Warn and error will be shown in the debug tab while log only lands in the logging.

## Context storage
As already said we can also store data in the Context. In NodeRed we have 3 predefindes scopes for the context access:
- context (Local to the Node, Only in the Node)
- flow (Anywhere in the Flow usable)
- global (Anywhere where the current NodeRed runs)

## Getter and Setter
With flow.get(["field1", "field2", "bsp"]) you will gain a Array with 3 Values acording to the fields in the flow scope called like this. With flow.set(["field1", "field2", "bsp"], [1, "xxx"])
In case a missing value appears the Value will be set to null.

## API References
### node
- node.id : the id of the Function node - added in 0.19
- node.name : the name of the Function node - added in 0.19
- node.outputCount : number of outputs set for Function node - added in 1.3
- node.log(..) : log a message
- node.warn(..) : log a warning message
- node.error(..) : log an error message
- node.debug(..) : log a debug message
- node.trace(..) : log a trace message
- node.on(..) : register an event handler
- node.status(..) : update the node status
- node.send(..) : send a message
- node.done(..) : finish with a message
### context
- context.get(..) : get a node-scoped context property
- context.set(..) : set a node-scoped context property
- context.keys(..) : return a list of all node-scoped context property keys
- context.flow : same as flow
- context.global : same as global
### low
- flow.get(..) : get a flow-scoped context property
- flow.set(..) : set a flow-scoped context property
- flow.keys(..) : return a list of all flow-scoped context property keys
### global
- global.get(..) : get a global-scoped context property
- global.set(..) : set a global-scoped context property
- global.keys(..) : return a list of all global-scoped context property keys
### RED
- RED.util.cloneMessage(..) : safely clones a message object so it can be reused
### env
- env.get(..) : get an environment variable
