# vra-software-mongodb
vRA Software Component to automate the delivery of a Mongo NoSQL Database Cluster

#Prerequisites

 - [Deploy vRealize Automation (7.2+)](https://my.vmware.com/group/vmware/details?downloadGroup=VRA-720&productId=624) (vRA)
 - Configure a Centos vSphere template image that has the guest agent configured
	 - Use the prepare_template.sh script to configure the linux template, available on https://vra_server:5480/installer
 - Import the [MongoCluster-composite-blueprint.zip](https://github.com/clearascloud/vra-software-mongodb/blob/master/bin/MongoCluster-composite-blueprint.zip) from github and import into vRA using CloudClient
	 - Open CloudClient and login
	 >*vra login userpass* --server vra_server --user vra_user --password vra_password --tenant vra_tenant*
	 *vra content import --path /path/to/MongoCluster-composite-blueprint.zip*


----------


#Overview & Capabilities

 - Uses vRealize Automation to deploy a mongo cluster on 3 nodes.
 - Deploys at least 3 servers in the clusters for quorum (never 2 as we don't want a [splitbrain](https://en.wikipedia.org/wiki/Split-brain_%28computing%29) situation)
 - Downloads & installs mongodb on the 3 nodes
 - Creates a replication set (choose a name at request time, default is "rs0")
 - Creates a shared key file for [internal authentication](https://docs.mongodb.com/v3.0/tutorial/enable-internal-authentication/) required for the replica nodes
	 - The root password is needed to automate the copying of the shared key to /etc folder
 - Creates a user against the "test" database & sets up authorization so only authenticated/authorized users can query the database.

**Composite Blueprint - Mongo Cluster**
![Screenshot](https://github.com/clearascloud/vra-software-mongodb/blob/master/images/Mongo_cluster_composite_blueprint.png)

**Provide mongo details**
![Screenshot](https://github.com/clearascloud/vra-software-mongodb/blob/master/images/Mongo_cluster_software.png)


----------


#Authentication against the database and insert entry

**Authenticate**:
>mongo --port 27017 -u "admin" -p "password"

**Insert Entry on the primary node**:
>db.foo.insert( { x: 1, y: 1 } )


----------


#Useful Mongo Commands

**Validate replication configuration via javascript command:**
>mongo --port 27017 -u "admin" -p "password" --eval "rs.conf()"

**Validate replication status via javascipt command:**
>mongo --port 27017 -u "admin" -p "password" --eval "rs.status()"

**Validate replication status via interactive shell:**
>mongo --port 27017 -u "admin" -p "password"
> 
> show users


#vRealize Automation Overview

**The Software Component**
The single software component "Mongo-3.4" is configured to run on the mongo cluster nodes

**The Composite Blueprint**
The composite blueprint "Mongo Cluster" is setup to deploy a minimum 3 nodes and is wired up to pass request time properties to the 

[For more information on configuring the VRA Blueprint see this clearascloud.com blog entry](https://clearascloud.com/2017/02/14/deploying-mongodb-cluster-with-vrealize-automation-a-how-to-guide/)
