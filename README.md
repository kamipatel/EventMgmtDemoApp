
# CI
SELECT COUNT(eventapppkg__Registration__dlm.eventapppkg__eventapppkg_Contact_c__c) AS eventapppkg__total_registration__c, eventapppkg__Registration__dlm.eventapppkg__Event__c AS eventapppkg__event__c FROM eventapppkg__Registration__dlm GROUP BY eventapppkg__event__c

SELECT COUNT(eventapppkg__Registration__dlm.eventapppkg__eventapppkg_Contact_c__c) AS otal_registration__c, eventapppkg__Registration__dlm.eventapppkg__Event__c AS event__c FROM eventapppkg__Registration__dlm GROUP BY event__c

04t5Y000001MkFa
04t5Y000001MkFf
04t5Y000001MkH2

# devhubs
kamlesh.patel-j2ek@force.com (eventmgrapp)
ns@248.kam (eventapppkg)
kam@jeremypbo.org (jhddc)
kamlesh.patel-vrzx@force.com (eventmanageapp)

# Authenticate devhub
sf org login web --alias devhub --set-default
sf config set target-dev-hub=kamlesh.patel-j2ek@force.com

# just dc
sf org create scratch -f config/project-scratch-def.json -a a2 --duration-days 30 
sf org open -u a2

# deploy base 
sf project deploy start --manifest package.xml --target-org a2
sf project retrieve start --manifest package.xml --target-org a2


# Assign permset
sf org assign permset --name Event_Manager_LabApp_permset --target-org a2


# retriever base 
sf project retrieve start --metadata CustomObject -r force-app --target-org t3

# retriever dc 
sf project retrieve start --metadata CustomObject:eventapppkg__EventData__dlm CustomObject:eventapppkg__EventReview__dlm	 CustomObject:eventapppkg__Registration__dlm  DataSource DataSrcDataModelFieldMap DataSourceBundleDefinition DataSourceObject DataStreamTemplate DataPackageKitObject DataPackageKitDefinition DataCalcInsightTemplate  --target-org t3


 sf project retrieve start -r dc-app --metadata CustomObject:eventapppkg__EventData__dlm  DataSource DataSrcDataModelFieldMap DataSourceBundleDefinition DataSourceObject DataStreamTemplate DataPackageKitObject DataPackageKitDefinition DataCalcInsightTemplate  --target-org t3

sf project retrieve start --metadata CustomObject:eventapppkg__EventData__dlm  DataSource DataSrcDataModelFieldMap DataSourceBundleDefinition DataSourceObject DataStreamTemplate DataPackageKitObject DataPackageKitDefinition DataCalcInsightTemplate  --target-org t2

# package create
-- Core
sf package create -n "eventlabapp core Package" -r force-app -t Managed
0Hoaj00000005SjCAI
-- DC
sf package create -n "eventlabapp DC Extension Package" -r dc-app -t Managed
0Hoaj00000005R7CAI

# package version
-- Core
sf package version create -w 30 -c -x -p "eventlabapp core Package"
-- DC
sf package version create -w 90 -c -x -p "eventlabapp DC Extension Package" -f config/project-scratch-def.json --skipancestorcheck -n 2.0.0.1
 

-- sf package version create -w 90 -c -x -p "eventlabapp DC Extension Package" -f config/project-scratch-def.json --skipancestorcheck -n 2.0


# package version prompte
-- Core
sf package version promote --package 04taj00000012HVAAY
-- DC
sf package version promote --package 04taj00000012RBAAY

# install
-- Core
/packaging/installPackage.apexp?p0=04taj00000012HVAAY
--DC
/packaging/installPackage.apexp?p0=04taj00000012RBAAY

# sc1 deploy force-app
sf project retrieve start --metadata CustomObject -r dc-app --target-org t2

-- DC
/packaging/installPackage.apexp?p0=04taj00000011LRAAY

individual id in WebEng
ssot__IndividualId__c
2f8bccb3-36ff-42aa-98e0-b18bbcb99779


ssot__Individual__dlm
ssot__Id__c = 003Ws0000010tNJIAY

contact point email  with party
ssot__Id__c
003Ws0000010tNJIAY

select ssot__PagePublicTitleName__c as pagetitle, user.Email__c as email, ssot__IdentificationNumber__c, webeng.ssot__IndividualId__c as individualId, individual.ssot__FirstName__c as firstname, individual.ssot__LastName__c as lastName from ssot__WebsiteEngagement__dlm as webeng, ssot__PartyIdentification__dlm as party, UserDMO__dlm as user, ssot__AccountContact__dlm contact, ssot__ContactPointEmail__dlm as email, ssot__Individual__dlm as individual where 
webeng.ssot__IndividualId__c = party.ssot__PartyId__c and 
party.ssot__IdentificationNumber__c = user.Id__c and 
user.ContactId__c=contact.ssot__Id__c and 
contact.ssot__Id__c = email.ssot__Id__c and 
webeng.ssot__IndividualId__c = individual.ssot__Id__c


select webeng.ssot__IndividualId__c as individualId, individual.ssot__Id__c  from ssot__WebsiteEngagement__dlm as webeng, ssot__Individual__dlm as individual, ssot__PartyIdentification__dlm as party, ssot__AccountContact__dlm contact where webeng.ssot__IndividualId__c = individual.ssot__Id__c and individual.ssot__Id__c= party.ssot__PartyId__c and contact.ssot__Id__c =party.ssot__IdentificationNumber__c 


select webeng.ssot__IndividualId__c as individualId, individual.ssot__Id__c  from ssot__WebsiteEngagement__dlm as webeng, ssot__Individual__dlm as individual, ssot__PartyIdentification__dlm as party where webeng.ssot__IndividualId__c = individual.ssot__Id__c and individual.ssot__Id__c= party.ssot__PartyId__c
