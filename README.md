# Event Management Sample App
This app demonstrates how existing force.com app can be extended for data cloud and Marketing cloud

## Table of Contents

1. [Setup](#Setup)
2. [LightningApp](#LightningApp)
3. [DataCloudApp](#DataCloudApp)
4. [MarketingCloudApp](#MarketingCloudApp)

# About
- In this excecise we will create force.com, data cloud and Marketing cloud packages.

# Prerequisite
- Make sure your DevHub org is enabled for data cloud package creation
- You have linked your namespace org to the DevHub org
- You can use this app as a reference app. Make sure to replace namespace in sfdx-project.json
- You have completed trailheads on 2GP packaging
- You know how to setup Data cloud and Marketing cloud

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
# Create Package CRM
sf package create -n "eventdemoapp core demo Package" -r force-app -t Managed
# Create Package version CRM
sf package version create -w 90 -c -x -p "eventdemoapp core demo Package" -d force-app -f config/project-scratch-def.json 
# package version promote CRM
sf package version promote --package=04tWs000000AsrxIAC
# package version install in subscriber org
/packaging/installPackage.apexp?p0=04tWs000000AsrxIAC
```

# DataCloudApp
### Steps to create Data cloud datakit managed package
```
# Now let's build data cloud package for 2 DLO/DMO for CRM's 2 custom objects
# Configure the data cloud
# First update "Data Cloud Salesforce Connector" permissionset to give read perms (Read, View All, all fields) to both CRM sObjects i.e. EventData, Registration
# In the same org, 
sf project retrieve start --manifest dc.xml --target-org dc1
# Create Package DC
sf package create -n "eventdemoapp DC ext Package" -r dc-app -t Managed
# Create package version DC
sf package version create -w 90 -c -x -p "eventdemoapp DC ext Package" -d dc-app -f config/project-scratch-def.json 
# package version promote DC
sf package version promote --package=04tWs000000Az8fIAC
# package version install in subscriber org
/packaging/installPackage.apexp?p0=04tWs000000Az8fIAC

```

# MarketingCloudApp
### Steps to create Marketing cloud CMS managed package
```
# In the same org, configure the Marketing cloud if not done already. Under CMS create email, landing page and a form
# Get metadata
sf project retrieve start --manifest cms.xml --target-org dc1
# Create package CMS
sf package create -n "eventdemoapp CMS ext Package" -r dc-app -t Managed
# Create package version CMS
sf package version create -w 90 -c -x -p "eventdemoapp DC CMS Package" -d force-app -f config/project-scratch-def.json 
# package version promote DC
sf package version promote --package=04tWs000000Az8fIAC
# package version install in subscriber org
/packaging/installPackage.apexp?p0=04tWs000000Az8fIAC
```
