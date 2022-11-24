 [Zurück](Research.md)

Project: [GitLab](https://git.intra.uibk.ac.at/c102371/webseitestudioansteuerung/-/tree/main/)

# Quick Overview
- NextJSProject: Default runs on Port 3000
    - Most of Business logic and index page
- kratos-admin-ui: running on Port 3001
  - Just a Admin-ui for Ory/Kratos, not selfmade
- SelfService: runns on Port 4455
  - Runs SelfServices for all except Login and Registration

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
          "pattern": "^c[0-9]{6}$",
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
This Identity has the Traits: User, the Password is used to identify if it is the right person and gets saved hashed in the db. The only real modification to the preset from ory is the metadata_public that tells us the Admin Status of an User.
Also I Added a Pattern to the Username to fit into the format: c000000

## Ory Kratos Configuration Yaml
Ory Kratos and other services by Ory are configured via yaml files, which get called while the start of the Service

For Ory Kratos the Start-command looks like this:
kratos serve -c ```<Path-to-yaml>```

If selfhosted the DB has to be migrated first with:
::kratos migrate sql "mysql://user:pw@tcp(localhost:3306)/User"

### current configurations + some explanations
``` yaml
version: v0.10.1

dsn: mysql://root:root@tcp(localhost:3306)/User?parseTime=true

serve:
  #for the public Kratos API (No Metadata, some API point are only over the Admin API accessible) 
  public:
    #Port to Run Public API
    base_url: http://127.0.0.1:4433/
    cors:
      enabled: true
      # Cors allowed from main project and the Admin-UI
      allowed_origins:
        - http://localhost:3000/
        - http://localhost:3001/ # Admin-Ui
      allowed_methods:
        - POST
        - GET
        - PUT
        - PATCH
        - DELETE
      allowed_headers:
        - Authorization
        - Cookie
        - Content-Type
      exposed_headers:
        - Content-Type
        - Set-Cookie
  #rules for Admin-API
  admin:
    base_url: http://127.0.0.1:4434/
      
    
# Which Selfservices should be run, where should the get you to and what should be done before, while and after
selfservice:
  default_browser_return_url: http://localhost:3000/
  allowed_return_urls:
    - http://localhost:3000/

  methods:
    password:
      enabled: true

  flows:
    error:
      ui_url: http://127.0.0.1:4455/error

    settings:
      ui_url: http://127.0.0.1:4455/settings
      privileged_session_max_age: 15m

    recovery:
      enabled: true
      ui_url: http://127.0.0.1:4455/recovery

    verification:
      enabled: true
      ui_url: http://127.0.0.1:4455/verification
      after:
        default_browser_return_url: http://127.0.0.1:4455/

    logout:
      after:
        default_browser_return_url: http://localhost:3000/Login

    login:
      ui_url: http://localhost:3000/Login
      lifespan: 10m

    registration:
      lifespan: 10m
      ui_url: http://localhost:3000/Registration
      after:
        password:
          hooks:
            -
              hook: session
# logging
log:
  level: debug
  format: text
  leak_sensitive_values: true

secrets:
  cookie:
    - PLEASE-CHANGE-ME-I-AM-VERY-INSECURE
  cipher:
    - 32-LONG-SECRET-NOT-SECURE-AT-ALL
# algorithms
ciphers:
  algorithm: xchacha20-poly1305
#hashing function
hashers:
  algorithm: bcrypt
  bcrypt:
    cost: 8
#identity with default (if no other identity gets called) and all Schemas that can be used, here with only one User: Benutzer
identity:
  default_schema_id: Benutzer
  schemas:
    - id: Benutzer
      url: base64://ewogICIkaWQiOiAiQmVudXR6ZXIiLAogICJ0aXRsZSI6ICJQZXJzb24iLAogICJ0eXBlIjogIm9iamVjdCIsCiAgInByb3BlcnRpZXMiOiB7CiAgICAidHJhaXRzIjogewogICAgICAidHlwZSI6ICJvYmplY3QiLAogICAgICAicHJvcGVydGllcyI6IHsKICAgICAgICAiVXNlciI6IHsKICAgICAgICAgICJ0eXBlIjogInN0cmluZyIsCiAgICAgICAgICAidGl0bGUiOiAiQy1Db2RlIiwKICAgICAgICAgICJwYXR0ZXJuIjogIl5jWzAtOV17Nn0kIiwKICAgICAgICAgICJvcnkuc2gva3JhdG9zIjogewogICAgICAgICAgICAiY3JlZGVudGlhbHMiOiB7CiAgICAgICAgICAgICAgInBhc3N3b3JkIjogewogICAgICAgICAgICAgICAgImlkZW50aWZpZXIiOiB0cnVlCiAgICAgICAgICAgICAgfSwKICAgICAgICAgICAgICAid2ViYXV0aG4iOiB7CiAgICAgICAgICAgICAgICAiaWRlbnRpZmllciI6IHRydWUKICAgICAgICAgICAgICB9LAogICAgICAgICAgICAgICJ0b3RwIjogewogICAgICAgICAgICAgICAgImFjY291bnRfbmFtZSI6IHRydWUKICAgICAgICAgICAgICB9CiAgICAgICAgICAgIH0KICAgICAgICAgIH0sCiAgICAgICAgICAibWF4TGVuZ3RoIjogMzIwCiAgICAgICAgfQogICAgICB9LAogICAgICAicmVxdWlyZWQiOiBbCiAgICAgICAgIlVzZXIiCiAgICAgIF0sCiAgICAgICJtZXRhZGF0YV9wdWJsaWMiOiB7CiAgICAgICAgInR5cGUiOiAib2JqZWN0IiwKICAgICAgICAicHJvcGVydGllcyI6IHsKICAgICAgICAgICJJc0FkbWluIjogewogICAgICAgICAgICAidHlwZSI6ICJib29sZWFuIiwKICAgICAgICAgICAgImRlZmF1bHQiOiBmYWxzZQogICAgICAgICAgfQogICAgICAgIH0KICAgICAgfSwKICAgICAgImFkZGl0aW9uYWxQcm9wZXJ0aWVzIjogZmFsc2UKICAgIH0KICB9Cn0=
#its possible to also add emails and send emails but not used for this project
courier:
  smtp:
    connection_uri: smtps://test:test@mailslurper:1025/?skip_ssl_verify=true

```
# Ory Oathkeeper
is another Service from Ory thats been used for Usermanagement as it checks headers and configures them acordingly to their source and destination Address.

It also makes sure that only source-destination combination get trough that we first defined with rules in the rules.yaml

How Oathkeeper should run and be configured is written in the oathkeeper.yaml and get read in on Start of the Service which can be done like this:
oathkeeper serve -c ```<path to oathkeeper.yaml>```
 ## Oathkeeper.yaml
 Here are all configuration we want to make to Oathkeeper, it looks like this:
```yaml
serve:
  proxy:
    port: 4450 # run the proxy at port 4450
    cors:
      enabled: true
    
  api:
    port: 4456 # run the api at port 4456
  prometheus:
    port: 9090
    host: localhost
    metrics_path: /metrics
  

# how the access-rules should behave and match patterns
access_rules:
  matching_strategy: regexp
  repositories:
    - file:///C:/Users/c102371/Documents/Blog/nextjsfirsttry/nextjsfirsttry/oathkeeper/rules.yaml

errors:
  fallback:
    - json
  handlers:
    json:
      enabled: true
      config:
        verbose: true
    redirect:
      enabled: true
      config:
        to: https://www.ory.sh/docs

mutators:
  header:
    enabled: true
    config:
      headers:
        X-User: "{{ print .Subject }}"
        # You could add some other headers, for example with data from the
        # session.
        # X-Some-Arbitrary-Data: "{{ print .Extra.some.arbitrary.data }}"
  noop:
    enabled: true
  id_token:
    enabled: true
    config:
      issuer_url: http://localhost:4450/
      jwks_url: file:///jwks.json

authorizers:
  allow:
    enabled: true

authenticators:
  bearer_token:
    enabled: true
    config:
      check_session_url: https://session-store-host
      token_from:
        header: ory_pat_AzRrrvr93n07gTwFDDbNs9biAnrW3JYz
  noop:
    # Set enabled to true if the authenticator should be enabled and false to disable the authenticator. Defaults to false.
    enabled: true

  anonymous:
    enabled: true
    config:
      subject: guest


```
## rules.yaml
here are all rules that can match a specific redirect or source-destiny-pair
eg.:
```yaml
#first the name of the Rule, most of the time this already tells you what the rule does
- id: RuleOne
# which version of Oathkeeper was this written
  version: v0.40.0
  # link that we should get send to
  upstream: 
    url: http://127.0.0.1:4434/admin/identities/ecc393ba-d46f-4fea-b1c6-6edb4
    preserve_host: false
  # incomming request link
  match:
    url: http://127.0.0.1:4450/admin/identities/ecc393ba-d46f-4fea-b1c6-6edbdad60734
    methods:
      - GET
      - DELETE
  authenticators:
    - handler: anonymous
    - handler: noop
    - handler: bearer_token
      config: 
        token_from: 
          header: ory_pat_AzRrrvr93n07gTwFDDbNs9biAnrW3JYz
  authorizer:
    handler: allow
  mutators:
    - handler: noop
  errors:
    - handler: json
    - handler: json

- id: Rule
  version: v0.40.0
  upstream: 
    url: http://127.0.0.1:4434/admin/identities/<.*>
    strip_path: admin/identities/<.*>
  match:
    url: http://localhost:4450/admin/identities/<.*>
    methods:
      - GET
      - DELETE
      - PUT
  authenticators:
    - handler: anonymous
    - handler: noop
    - handler: bearer_token
      config: 
        token_from: 
          header: ory_pat_AzRrrvr93n07gTwFDDbNs9biAnrW3JYz
  authorizer:
    handler: allow
  mutators:
    - handler: noop
  errors:
    - handler: json
    - handler: json
```
# Components
(look into Ory Elements, new way to make login and such)
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

# Pages
A quick summary what which page does

## VideoWithMediaRecorder.tsx
First version of an own Recorder for the Page -> SetState got an infinit set/reload Cycle

## Video.tsx
VideoWithMediaRecorder rework, a basic videocapturer but not recorder. 
With the recordingfeature it started to eat a lot of resources 

## TestComp.tsx
A Site with Slider to Test Component without changing the frontpage

## Registration.tsx
The regisration page, can be accessed via the Login page, one of 2 Pages that are SelfService but in the main project (NextJSProject)

## Login.tsx
The Login Page with Ory integration -> Fields are generated according to the Ory Kratos Identity-Schema

## index.tsx
Main Page of the Website, here are some API-Button, Meta-Buttons(Kratos-API) or testing for features
The Roomselection is also on this page

## error.tsx
A Test Page for the Error functionality

## Admin.tsx
A Page that got created before the Admin-UI

## 404.js
The 404 Error Page

## _app.js
Page that every NextJS App needs, for global style Sheets

## rooms
A Folder for the different Rooms, so every room can have different components depending on room equipment
Current rooms are just test rooms


