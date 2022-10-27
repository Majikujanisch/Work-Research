 [Zurück](Research.md)

Project: [GitLab](https://git.intra.uibk.ac.at/c102371/webseitestudioansteuerung/-/tree/main/)

# Quick Overview
- NextJSProject: Default runs on Port 3000
- kratos-admin-ui: running on Port 3001
- SelfService: runns on Port 4455

# Getting Started

To download the selfhosted Kratos and Oathkeeper (AccessControl, Proxy) you'll need Scoop. Get at least Kratos, Oathkeeper would only be needed if you want to send a request to the AdminApi of kratos for the authentication and authorization.

To Start the instance (and the Port) type: 

kratos serve -c  {PATH TO KRATOS.yaml in NextJSProject} 

As Kratos needs a DB i used mysql to have something, then you have to type:

kratos migrate sql "mysql://USER:PASSWORD@tcp(localhost:3306)/DB"
 
before the serve command
for Oathkeeper the same (there is also a yaml for oathkeeper in NextJSProject)

Now start the NextJSProject (can be done in Package.json via right clicking on the 4 Line or  by typing next dev in the cli when in the folder). Its important to start this before the AdminUi as this has also port 3000 as default. (maybe can be configured in the AdminUI Folder somewhere)

Then you can Start either the AdminUI first or the SelfService.

## setting environmental variables

these can be set in the .env.local and can be used via process.env.VARIABEL

in my setup i had in SelfService an .env.local with the content: 
ORY_SDK_URL=http://localhost:4433/
in AdminUI:

KRATOS_ADMIN_URL = http://127.0.0.1:4434 

KRATOS_PUBLIC_URL = http://127.0.0.1:4433

in NextJSProject: 

ORY_SDK_URL= http://localhost:4433/
ORY_ACCESS_TOKEN="TOKEN"
LOG_LEAK_SENSITIVE_VALUES=true
KRATOS_PUBLIC_URL = http://127.0.0.1:4433

# Ory
What is Ory? Ory is a software infrastructure provider building a global zero-trust network for humans, robots, devices, and software services. Ory develops open-source software on GitHub and publishes open standards such as the Ory Permission Language. The Ory Network uses cloud-native open-source technologies (Kubernetes, Crossplane, Cockroach, Linux, Ory) and standards (OAuth 2.0/2.1, OpenID Connect, MITREid, WebAuthn, TOTP, FIDO3) to deliver a low-latency, planet-scale zero-trust infrastructure. Ory combines centuries of open-source, security, operational, and industry expertise with a user-centric and security-first mindset. (https://www.ory.sh/docs/welcome)

As already said we need ory for the User Management, thus we use mostly Ory Kratos (Oathkeeper only if Admin-calls have to be made from the page itself and not via the Admin-UI)

## Ory Kratos
Ory Kratos is an API-first identity and user manager

## identitys
Ory Kratos calls user instances identitys, these can be defined by the identity-schemas which can be configured in the kratos.yaml

An Identity always has an id, state, credentials, Schema_id and traits

the current Identity-Scheme for "Benutzer" looks like this: 
```json
{
  "$id": "Benutzer",
  "title": "Person",
  "type": "object",
  "properties": {
    "traits": {
      "type": "object",
      "properties": {
        "User": {
          "type": "string",
          "title": "C-Code",
          "ory.sh/kratos": {
            "credentials": {
              "password": {
                "identifier": true
              },
              "webauthn": {
                "identifier": true
              },
              "totp": {
                "account_name": true
              }
            }
          },
          "maxLength": 320
        }
      },
      "required": [
        "User"
      ],
      "metadata_public": {
        "type": "object",
        "properties": {
          "IsAdmin": {
            "type": "boolean",
            "default": false
          }
        }
      },
      "additionalProperties": false
    }
  }
}
```
This Identity the Traits: User, the Password is used to identify if it is the right person and gets saved hashed in the db. The only real modification to the preset from ory is the metadata_public that tells us the Admin Status of an User.


# Components
Here is a quick explaining how the custom components should work and how to use em in code.

## APILoginButton
TODOs (Some components are just fast done, so some stuff would have to be done to implement it safely)
 - const bearer as Environmental variable
 - Put does not work, use Metadatabutton for changes
 - get the apilink out of the component props

This component can be used to DELETE User or to get a User, not really used in the side, but to test Ory Kratos.

APILoginButton(apilink, typeofReq, text, req = null, id = null)
  - apilink: Link to the API
  - typeofReq: DEL or GET up to now
  - text: Text of the button that should be displayed
  - req: For the tests with the V0alpha2Api SDK
  - id: For the test with the V0alpha2Api SDK

### Example
```JSX
<APIButton
          apilink="http://localhost:4450/admin/identities/6b381821-8226-4550-bb1a-c70cf7b930e7"
          typeofReq="DEL"
          text = "GET User Port 4450">
          </APIButton>
```

## Dropdown
This component can be used to chose the room, in which the lights should be controlled. You can give it a list of items (eg: list of room Objects), an Id, what should happen on an selected item change, defaultItem, ...

Dropdown(item, defaultSelectedItem, onSelectedItemChange)
  - item: List of Objects, that should be choosable
  - defaultSelectedItem: When reloading, which item should be displayed
  - onSelectedItemChange: When the selected Item changes this action happens

### Example
```JSX
<Dropdown
              items={items}
              id="room-switch"
              onSelectedItemChange={handleStateChange}
              defaultSelectedItem={items[0]}/>
//with 
const items = [
  {
    label:"RAUM AUSWAHL",
    value: "0"
  },
  {
      label: "Raum 1",
      value: "1"
  },
  {
      label: "Raum 2",
      value: "2"
  },
  {
      label: "Raum 3",
      value: "3"
  },
];
//and
const handleStateChange = useCallback((onSelectedItemChange) => setSelectedRoom(onSelectedItemChange.selectedItem.value) ,[]);
```

## DropdownItem
This component gets used in the Dropdown - component. This is mostly needed if you want to change the look of the Dropdown.
   


## MetadataButton
TODOs (Some components are just fast done, so some stuff would have to be done to implement it safely)
 - const bearer as Environmental variable
 - isAdmin if differ so only if user is admin, the put or del can be used (del also if the user deleting is the same as the user that should be deleted, deleting of own acc)
 - typofReq - Switch workover


This component is used to change Metadata, in the current project we only have the meatdata_public: isAdmin, this field tells us, if the identity is an admin and is allowed to see the button to the Admin side (Where a identity check, similar to the index side, should check if the current session really is a adminsession)
MetdadataButton(metadata, isAdmin , schemaId, state, username, apilink, typeofReq, text, id)
  - metadata: the metadata that should be updated to the identity
  - isAdmin: currently tells if the metadata should be added to the public or the admin metadata field (Admin can only be read by admin, public also by the nonadmin apis)
  - schemaId: Id of the KratosIdentity
  - state: state of the identity (Active or inactive)
  - username: username of the identity
  -  apilink: link where the request should be sent to
  - typeofReq: GET, DEL, PUT, other methods would have to be implemented
  - text: Text that should be displayed on button
  - id: id of the identity that should be changed

### Example
```JSX
          <MetaButton
          apilink="http://localhost:4450/admin/identities/39704c0f-8b9b-4bc3-8049-0483d55e9268"
          typeofReq = "PUT"
          text = "PUT"
          metadata = {{"isAdmin": "true"}}
          schemaId = "Benutzer"
          state = "active"
          username = "testbro"
          isAdmin = {false} //will be written to the public metadata
          id = "631b7dda-04d0-4273-823a-3ec3ac0e91c5">
          </MetaButton>

```

## Numberpad

This Component returns a numberpad inorder to put in a password on a device, that does not have a keayboard.
Numberpad()

# IOBroker
There are also components explicitly for the communication to the IOBroker Instance. 

## Switch
TODOs
- adjust it to the actual name of the IOBroker knx parts, only a placeholder link (to a made up datapoint)
- integrate the lightnumber to control multiple lights within a room 

This component should simulate a light Switch, so if a datapoint is true, it gets set to false and vice versa.
Switch(room, text, lightnumber)
- room: the room that the light sits in
- text: text that should be displayed on the button
- lightnumber: the light that should be controlled if more than one light is in a room (not implemented yet, 27.10.22)
  
### Example

```JSX
          <Switch room="2" text="lampe test switch"></Switch>

```



