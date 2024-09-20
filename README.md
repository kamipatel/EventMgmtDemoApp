# Event Management Sample App
This app demonstrates how existing force.com app can be extended for data cloud and Marketing cloud

## Table of Contents

1. [Setup](#Setup)
2. [LightningApp](#LightningApp)
3. [DataCloudApp](#DataCloudApp)
4. [MarketingCloudApp](#MarketingCloudApp)


# Setup
### Authenticate devhub
- Sign in with YOUR_DEVHUB_ORG_USERNAME
- sf org login web --alias devhub --set-default
- sf config set target-dev-hub=REPLACE_WITH_YOUR_DEVHUB_ORG_USERNAME

# LightningApp
### Steps to create Lightning app managed package
```
# Create scratch org for CRM + DC
sf org create scratch -f config/project-scratch-def.json -a dc1 --duration-days 30 
# In CRM Create 2 custom objects i.e.
EventData, Registration 
# Now let's build core package for 2 custom objects and retrieve CRM sObjects (along with other needed artifacts e.g. permset, app etc)
sf project retrieve start --manifest package.xml --target-org dc1
# package create CRM
sf package create -n "eventdemoapp core demo Package" -r force-app -t Managed
# package version CRM
sf package version create -w 90 -c -x -p "eventdemoapp core demo Package" -d force-app -f config/project-scratch-def.json 
    # when needed add -skipancestorcheck flag to above
# package version promote CRM
sf package version promote --package=04tWs000000AsrxIAC
# package version install in subscriber org
/packaging/installPackage.apexp?p0=04tWs000000AsrxIAC
```

# DataCloudApp
### Now let's build data cloud package for 2 DLO/DMO for CRM's 2 custom objects
### Configure the data cloud
### First update "Data Cloud Salesforce Connector" permissionset to give read perms (Read, View All, all fields) to both CRM sObjects i.e. EventData, Registration
```
# In the same org, 
sf project retrieve start --manifest dc.xml --target-org dc1
# package create DC
sf package create -n "eventdemoapp DC ext Package" -r dc-app -t Managed
# package version DC
sf package version create -w 90 -c -x -p "eventdemoapp DC ext Package" -d dc-app -f config/project-scratch-def.json 
    # when needed add -skipancestorcheck flag to above. Also "ancestorVersion": "HIGHEST" if needed
# package version promote DC
sf package version promote --package=04tWs000000Az8fIAC
# package version install in subscriber org
/packaging/installPackage.apexp?p0=04tWs000000Az8fIAC

