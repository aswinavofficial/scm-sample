

### Pre requisites


* Ubuntu 16.04 LTS ( 18.04 not supported )
* Docker Engine: Version 17.03 or higher
* Docker-Compose: Version 1.8 or higher
* Node: 8.9.4 (Important : Other versions are not supported) 
* npm: v5.x
* git: 2.9.x or higher
* Python: 2.7.x


**If installing Hyperledger Composer using Linux, be aware of the following advice:

Login as a normal user, rather than root.
Do not su to root.
When installing prerequisites, use curl, then unzip using sudo.
Run prereqs-ubuntu.sh as a normal user. It may prompt for root password as some of it's actions are required to be run as root.
Do not use npm with sudo or su to root to use it.
Avoid installing node globally as root.**


### Step  1: Pre-requisites Installation



1. Install the dependencies and pre requisites

```sh
$ curl -O https://hyperledger.github.io/composer/latest/prereqs-ubuntu.sh
$ chmod u+x prereqs-ubuntu.sh
$ sudo ./prereqs-ubuntu.sh

```

2. set permission for nvm
```sh
$ sudo chmod 777 -R /home/ubuntu/.nvm
$ sudo chown ubuntu:ubuntu -R /home/ubuntu/.nvm
$ sudo chown -R $USER:$(id -gn $USER) /home/ubuntu/.config
```

3. Install node v8.9.4 using nvm
```sh
$ nvm install 8.9
$ nvm use 8.9.4
$ nvm alias default 8.9.4
```

### Step 2: Composer Tools Installation


1. Essential CLI tools:
```sh
$ npm install -g composer-cli@0.20
```
2. Utility for running a REST Server on your machine to expose your business networks as RESTful APIs:
```sh
$ npm install -g composer-rest-server@0.20
```
3. Useful utility for generating application assets:
```sh
$ npm install -g generator-hyperledger-composer@0.20
```
4. Yeoman is a tool for generating applications, which utilises generator-hyperledger-composer
```sh
$ npm install -g yo
```
5. Browser app for simple editing and testing Business Networks:
```sh
$ npm install -g composer-playground@0.20

```

### Step 3: Install Hyperledger Fabric
This step gives you a local Hyperledger Fabric runtime to deploy your business networks to.

1. Download fabric dev server scripts and run them
```sh
$ mkdir ~/fabric-dev-servers && cd ~/fabric-dev-servers
$ curl -O https://raw.githubusercontent.com/hyperledger/composer-tools/master/packages/fabric-dev-servers/fabric-dev-servers.tar.gz
$ tar -xvf fabric-dev-servers.tar.gz
$ sudo chmod 777  downloadFabric.sh
$ sudo chmod 777 -R fabric-scripts/
$ sudo ./downloadFabric.sh
$ sudo chmod 777 stopFabric.sh
$ sudo chmod 777 startFabric.sh
$ sudo chmod 777 createPeerAdminCard.sh
$ sudo chmod 777 teardownFabric.sh
```
2. Stop previous instances(Skip during fresh installation)
```sh
$ sudo ./stopFabric.sh
$ sudo ./teardownFabric.sh
$ sudo docker kill $(sudo docker ps -q)
$ sudo docker rm $(sudo docker ps -aq)
$ composer card delete -n PeerAdmin@hlfv1
```
3. Start the fabric
```sh
$ sudo ./startFabric.sh
```
4. Create Peer Admin Card (NB: Don't use sudo )
```sh
$ ./createPeerAdminCard.sh 

```

### Project Specific
1. Clone this repo
```sh
$ git clone https://github.com/aswinavofficial/scm-sample.git
$ cd scm-sample
```

2. Get into directory containing business logic
```sh
$ cd ../perishable-network
```
6. Create Business Network Archive(bna) File
```sh
$ composer archive create --sourceType dir --sourceName . -a ../perishable-network.bna
```
7. Install Peer admin card
```sh
$ cd ..
$ composer network install --card PeerAdmin@hlfv1 --archiveFile perishable-network.bna
```
8. Start the network(NB : Double check BNA Version )
```sh
$ composer network start --card PeerAdmin@hlfv1 --networkAdmin admin --networkAdminEnrollSecret adminpw  --file networkadmin.card --networkName perishable-network --networkVersion 0.2.6
```
9. Import Network Admin Card
```sh
$ composer card import --file networkadmin.card
```
10.Expose REST API
```sh
$ composer-rest-server -c admin@perishable-network -n never -w true
```
11. Generate angular app (Can skip this step as it is already generated.If server is changed then you need to generate it again)
```sh
$ yo hyperledger:angular
```
12. Deploy Angular app
```sh
$ cd angular-app
$ npm install
$ npm start
```



```





