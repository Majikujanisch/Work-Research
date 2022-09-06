[Zurück](Research.md)
# Developing flows
## Creating Flows
Burgermenu, Flow, Add
Rename flows:
Burgermenu, Flow, Renames
## Flow Structure
As you go Flows (The single pages) will grow and get uncontrollable, to prevent this there are some key rules to follow and advises to follow.

### Splitting flows
don't just paste all nodes into one single Flow, if there are some logical seperations that can be made, do em in different flows eg. All Flows for one room into one flow or all translations of statuses in one flow. So you always know where to look if something does not work in a spec room or with a spec. Status-output.

### Reusable Flows
As you go u will notice that some Node-Combi are always the same, in such cases you can use SubFlows.
Another way of making reusable Flows are the Link-Nodes, these can link two different flows together like they are one Flow.
What are the differences?
| link nodes  | Subflows |
| ------------- | ------------- |
| Only for start or end of a flow  | usable everywhere you want to like a normal node  |
| connect to more than one other link node | Each cell has its own instance |
| One Instance for all inputs |  |

### Customising SubFlows

When using Subflows you could just always override the msg.topic but you can also use the Subflow Propertys. These are properties that can be set on the subflow instance and appear as environmental variables inside the subflow.
Just use $(MY_TOPIC) and add the Variable to Subflow properties, this can be done on the lefthand top.

## Message Design
All Messages that pass through the flow are JavaScrip Objects. They mostly have a Payload property which is the default property that most nodes work with.

### Working with `msg.payload`
Only thing to know is that creating new Object-propertys are storage heavy so try to always write on the msg.payload if not needed later in the flow.
### Designing message properties
As you go you will notice that you wan't to give more than one value via the msg.payload. This can be done by putting all neede properties into the payload, but as other nodes might need a "clear" Payload this will bring some isues. There is no real workaround for it.

The Propertys reset and parts should'nt be used as they have some special meanings in some nodes.

## Documenting flows
How can be documented?
- by Nodename and no/almost no crossing wires
- Groups to indentify discrete section of the flow
- Moving nodes into SubFlows
- Via comment nodes or via the debug nodes

### Adding port labels
In case of more than one output port labels can be used to identify each output.

### Grouping NODES
This is just to visualize related nodes
![groups](Bilder/NodeRed/Groups.jpg)
This can be done by marking all nodes that should be grouped, hamburgermenu, groups, groups selection
Grouped Nodes can't be configured, so be sure to have everything working before grouping.
