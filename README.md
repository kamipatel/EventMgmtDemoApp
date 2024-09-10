# Authenticate devhub
sf org login web --alias devhub --set-default
sf config set target-dev-hub=

# past orgs
b1

# just dc
sf org create scratch -f config/project-scratch-def.json -a cms1 --duration-days 30 

sf org create scratch -f config/project-scratch-def.json -a dc1 --duration-days 30 

sf org create scratch -f config/project-scratch-def.json -a dc2 --duration-days 30 
00DOy0000087CW5

sf org create scratch -f config/project-scratch-def.json -a t1 --duration-days 30 

sf org open -u cms1

# deploy base 
sf project deploy start --manifest package.xml --target-org dc2

sf project deploy start --manifest package.xml --target-org t1

# Assign permset
sf org assign permset --name Event_Manager_LabApp_permset --target-org b1

# retriever form 
sf project retrieve start --manifest cms.xml --target-org cms1

# retriever base 
sf project retrieve start --manifest package.xml --target-org a2
sf project retrieve start --metadata CustomObject -r force-app --target-org b1

# retriever dc 
sf project retrieve start --manifest dc.xml --target-org dc1

sf project retrieve start --metadata DataSource DataSrcDataModelFieldMap DataSourceBundleDefinition DataSourceObject DataStreamTemplate DataPackageKitObject 
DataPackageKitDefinition --target-org b1


# package create
-- DC
sf package create -n "eventdemoapp DC Package" -r dc-app -t Managed
0HoWt00000001ybKAA

# package version
-- DC
sf package version create -w 90 -c -x -p "eventdemoapp DC Package" -d dc-app -f config/project-scratch-def.json 


sf package version create  -d data-app -w 100


/packaging/installPackage.apexp?p0=04tWs0000006yf7IAA

{!'Basic ' & BASE64ENCODE(BLOB($Credential.BillingServiceCredential.username & ':' & $Credential.BillingServiceCredential.password))}