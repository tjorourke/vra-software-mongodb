# vra-software-mongodb
vRA Software Component to automate the delivery of a Mongo NoSQL Database Cluster

#Prerequisites

 - [Deploy vRealize Automation (7.2+)](https://my.vmware.com/group/vmware/details?downloadGroup=VRA-720&productId=624) (vRA)
 - Configure a Centos vSphere template image that has the guest agent configured
 - Import the mongodb zip file from github and import into vRA.

 #Overview & Capabilities
 - Deploys at least 3 servers in the clusters for quorum (never 2 as we don't want a [splitbrain](https://en.wikipedia.org/wiki/Split-brain_%28computing%29) situation)
 - Downloads & installs mongodb on the 3 nodes
 - Creates a replication set (choose a name at request time, default is "rs0")
 - Creates a user & sets up authentication

![Screenshot](https://github.com/clearascloud/vra-software-mongodb/blob/master/images/MongoDb-Screenshot.png)
