# Packaging Notes

Created a namespaced scratch org on a 252 preview instance

`sf org create scratch -f config/project-scratch-def.json -d -a 252Preview_NS -y 30 -w 10`

## Base App

Deployed base app

`sf project deploy start -d v1-force-app`

Assign Permission Set

`sf org assign permset --name Event_Manager_LabApp_permset`

Create package from deployed starter code

`sf package create -n kamEventBase -t Managed -v original -r v1-force-app`

Grab the package Id, 0HoWs0000000OOvKAM

Create a package version

`sf package version create -p 0HoWs0000000OOvKAM -k password123 -v original -w 20 -c -f config/project-scratch-def.json`

04tWs00000081GfIAI

## Data Cloud

Grant read/view all perms to CRM objects

- this was done manually

Create data streams for:

- EventData
- Registration

Do not change Data Stream API names at the field or DLO level
Choose Category of Other for Object Details

Create custom DMO's for each

- remove namespace prefix from Object Label and API name, also drop double underscores e.g. `Registration_c_Home`
- Upon save, you should see the full, namespace prefixed api name

Create the data kit, `eventdk`

- Add Data Streams
  - Salesforce CRM Connector
  - Bundle Name `eventbundle`
- Add Data Models

Retrieve Data Kit Metadata
`sf project retrieve start -x dc.xml -r tmp-dc/ --wait 10 --ignore-conflicts`

Create Package 

`sf package create -n kamEventDk -t Managed -v original -r tmp-dc`

Grab the package Id, 0HoWs0000000OQXKA2

Create a package version

`sf package version create -p 0HoWs0000000OQXKA2 -k password123 -v original -w 20 -c -f config/project-scratch-def.json`

04tWs00000085NVIAY

After creating the package version, I noticed that I had a lot of extraneous stuff in the version. That did not cause an issue (surprisingly)

## Customer Org

install base package 

`sf package install -p 04tWs00000081GfIAI -o 252PreviewDCnoNS -w 10 -k password123`

Add Data Cloud Connector Perms for the new CRM objects

Deploy data cloud package 

`sf package install -p 04tWs00000085NVIAY -o 252PreviewDCnoNS -w 10 -k password123`

Create Data Streams for Event Data and Registration

- Data Cloud > Data Streams > New > Salesforce CRM > Data Bundles > eventbundle
- Select all fields (default)
- Deploy
- both data streams will be created along with DLO's and DMOs
