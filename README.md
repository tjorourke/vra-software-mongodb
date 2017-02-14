# vra-software-mongodb
vRA Software Component to automate the delivery of a Mongo NoSQL Database Cluster

#Prerequisites

 - [Deploy vRealize Automation (7.2+)](https://my.vmware.com/group/vmware/details?downloadGroup=VRA-720&productId=624) (vRA)
 - Configure a Centos vSphere template image that has the guest agent configured
	 - Use the prepare_template.sh script to configure the linux template, available on https://vra_server:5480/installer
 - Import the mongodb zip file from github and import into vRA using CloudClient
	 - Open CloudClient and login
	 >*vra login userpass* --server vra_server --user vra_user --password vra_password --tenant vra_tenant*
	 *vra content import --path /path/to/mongodb.zip*


----------


#Overview & Capabilities

 - Uses vRealize Automation to deploy a mongo cluster on 3 nodes.
 - Deploys at least 3 servers in the clusters for quorum (never 2 as we don't want a [splitbrain](https://en.wikipedia.org/wiki/Split-brain_%28computing%29) situation)
 - Downloads & installs mongodb on the 3 nodes
 - Creates a replication set (choose a name at request time, default is "rs0")
 - Creates a user against the "test" database & sets up authorization so only authenticated/authorized users can query the database.

![Screenshot](https://github.com/clearascloud/vra-software-mongodb/blob/master/images/MongoDb-Screenshot.png)


----------


#Authentication against the database and insert entry

**Authenticate**:
>mongo --port 27017 -u "admin" -p "password" --authenticationDatabase "test"

**Insert Entry**:
>db.foo.insert( { x: 1, y: 1 } )


----------


#Useful Mongo Commands

**Validate replication configuration via javascript command:**
>mongo --port 27017 -u "admin" -p "password" --authenticationDatabase "admin" --eval "rs.conf()"

**Validate replication status via javascipt command:**
>mongo --port 27017 -u "admin" -p "password" --authenticationDatabase "admin" --eval "status()"

**Validate replication status via interactive shell:**
>mongo --port 27017 -u "admin" -p "password" --authenticationDatabase "admin"
> 
> show users
