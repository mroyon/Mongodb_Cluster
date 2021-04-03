MongoDB Enterprise Replication and Failover Step by step

Hello, welcome to my channel. In this video, I tried to show how to implement "MongoDB Master/Slave replication" from the scratch and addition to this, also showed how fail over managed.

The steps are in details and in sequence to deploy a successful case.  How to setup haproxy has been covered in other video and also how to join the ubuntu server in windows AD. please check the other video for detail. 
My main target is to deploy such environment so I can use these mongoDB server for asp.net core web app application log management.

Starting with 4 vm ubuntu 
 1. mongo-a 192.168.1.126
 2. mongo-b 192.168.1.127
 3. mongo-c 192.168.1.128
  ---------------------------------------------------
 Step sequences:
Things to do:
     1. mongo-a :- mongo-a.royonlab.local - IP: 192.168.1.126
	2. mongo-b :- mongo-b.royonlab.local - IP: 192.168.1.127
	3. mongo-c :- mongo-c.royonlab.local - IP: 192.168.1.128 - previous vdo [join ubuntu with windows AD...]
	
Lets start!!!

Things to do:
Steps:
		1. deploy mongo db on each server
			a. server mongo-a: 
				1. sudo apt-get update
				2. sudo apt-get upgrade
				3. deploy Mongo DB Enterprise
					a. need to add Mongo DB Enterprise package list
						1. sudo wget -qO - https://www.mongodb.org/static/pgp/server-4.2.asc | sudo apt-key add - 
						2. echo "deb [ arch=amd64 ] http://repo.mongodb.com/apt/ubuntu bionic/mongodb-enterprise/4.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-enterprise.list
						[ref url: https://docs.mongodb.com/manual/tutorial/install-mongodb-enterprise-on-ubuntu/]
					b. update repo
						1. sudo apt-get update
					c. install Mongo DB Enterprise
						1. sudo apt-get install mongodb-enterprise
				4. sudo systemctl enable mongod
				5. sudo systemctl daemon-reload
				6. sudo service mongod start
				7. sudo service mongod status netstat -plntu
				8. check from client [using robo3T studio trail edition]
					[ref url: https://robomongo.org/]
				#### now editing the conf file for 1. ip binding, 2, security (optional, but we will), 3. replica set
				9. Edit config for IP binding
					a. sudo nano /etc/mongod.conf
						1. IP set to 192.168.1.126
					b. 	sudo service mongod restart
					c. client check OK.
				10. Security
					1. before we enable the security, will create a user with root privil on the "admin"
						a. connect to mongo-a: mongo --host mongo-a.royonlab.local
						b. use admin
						c. db.createUser( {user: "mongoadmin",pwd: "Asdf1234",roles: [ "root" ]}); --OK
					2. Create security keyfile on mongo-a [taking this as primary server]
						a. sudo openssl rand -base64 741 > keyfile
						b. sudo mkdir -p /opt/mongodb
						c. sudo cp keyfile /opt/mongodb
						d. sudo cp keyfile Document/ [to share on the other vm]
						e. sudo chown mongodb:mongodb /opt/mongodb/keyfile
						f. sudo chmod 0600 /opt/mongodb/keyfile
						g. sudo chmod 777 Document/keyfile [so we can share it to the other mongo server]
					3. edit mongo config enable security
						a. sudo nano /etc/mongod.conf
						b. edit
							1. security:
							2. keyFile: /opt/mongodb/keyfile
							3. authorization: enabled
						c. sudo service mongod restart
						d. client check with authentication [mongoadmin/Asdf1234] --OK
			b. server mongo-b: 
			c. server mongo-c: 
				1. follow the steps from 1 to 9
				2. will not create the user in these two server
			d. Start Replica config
				1. mongo-a: edit mongo config	
					a. sudo nano /etc/mongod.conf
						1. replication:  replSetName: mongodb-rs
				2. 	mongo-b: edit mongo config	
					a. sudo nano /etc/mongod.conf
						1. replication:  replSetName: mongodb-rs
				3. mongo-c: edit mongo config	
					a. sudo nano /etc/mongod.conf
						1. replication:  replSetName: mongodb-rs
				4. add the replica member to mongo-a
					a. rs.initialise()
					b. rs.add("mongo-b.royonlab.local:27017")
					c. rs.add("mongo-c.royonlab.local:27017")
				5. Replication test --OK
				6. Fail Over Test  
					a. when mongo-a is down, mongo-b has the primary status and mongo-c is seconday
					b. when mongo-b is down, mongo-a has the primary status and mongo-c is seconday
				

You can download the config files @
Github: https://github.com/mroyon/Mongodb_Cluster
upcoming video: ASP.Net Core Web App application log management using mongodb and serilog.

Software used:
1. VM workstation 14 [https://www.vmware.com/products/workstation-pro.html]
2. Bandicam: [https://www.bandicam.com/]
3. Ubuntu 18.04 [https://releases.ubuntu.com/18.04/]
4. Microsoft Server 2016 [https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2016]

––––––––––––––––––––––––––––––
Walk Around by Roa https://soundcloud.com/roa_music1031
Creative Commons — Attribution 3.0 Unported — CC BY 3.0
Free Download / Stream: https://bit.ly/walk-around-roa
Music promoted by Audio Library https://youtu.be/BimtUhUirnw
––––––––––––––––––––––––––––––


#mongodb-enterprise
#mongodbcluster
#mongodbfailover
#mongodbreplication


mongodb,mongo, mongodb replication,Ubuntu,AD,Linux,Tutorial,Step by step,mongo db cluster deploy ubuntu,Mongodb failover,Mongo DB failover,Mongo db Master/Slave Replication,Application log database,Failover,Mongo Db master-slave replication with failover,MongoDB Cluster failover ubuntu,highly available Mongo DB,cluster
