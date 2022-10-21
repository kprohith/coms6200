# COMS6200
SDN Network Statistics and Visualization using [Onos](https://opennetworking.org/onos/) and the [ELK](https://www.elastic.co/what-is/elk-stack) Stack

# Setup and Configuration for running Onos and Mininet

1.) Download the ```SDNFinal.ova``` VM Image
```bash
mkdir ~/sdn-viz && cd ~/sdn-viz
wget https://www.dropbox.com/s/wwzilgm3g3kxebr/SDNFinal.ova?dl=0
```

2.) Import the VM Image into VirtualBox

3.) Start the VM and login with the following username and password
```bash
username: sdn
password: sdn
```

4.) Build Onos 
```bash
cd ~/onos
bazel run onos-local 
```
Onos has successfully started if you see the following message
```bash
Updated node 127.0.0.1 state to READY 
```
OR

5.) Use the Onos helper script to start Onos
```bash
start-onos-local
```
6.) In a new terminal, clone the repo
```bash
cd `
git clone https://github.com/kprohith/coms6200/
```

7.) Create an Onos app
```bash
cd ~/OnosApps
 ~/onos/tools/dev/bin/onos-create-app  
 ```
 Specify the app properties as follows
 ```bash
 Define value for property 'groupId': org.hub.app
 Define value for property 'artifactId': hub
 Define value for property 'version' 1.0-SNAPSHOT: : 1.10.0
 Define value for property 'package' org.hub.app
```
 8.) Replace hub/pom.xml and src/main/java/org/hub/app/AppComponenet.java with the files from the repo
 ```bash
    cp ~/coms6200/pom.xml ~/OnosApps/hub/pom.xml
    cp ~/coms6200/AppComponent.java ~/OnosApps/hub/src/main/java/org/hub/app/AppComponent.java
```
9.) Build the app
```bash
cd ~/OnosApps/hub
mvn clean install
```
10.) Start Mininet
```bash
sudo mno --topo tree,3,2 --mac
```
11.) Sign up for an [ElasticCloud](https://cloud.elastic.co/home) account and create a new ElasticCloud deployment
12.) Make a note of the initial configuration details for the deployment - the username and password are show on deployment creation. These arwe your credentials for ```cloud_auth```.
13.) Make a note of the ```cloud_id``` bvalue from the deployment details page.

14.) Install logstash
```bash
sudo apt-get install logstash
```
15.) Copy logstash configuration file
```bash
sudo cp ~/coms6200/logstash.conf /etc/logstash/conf.d/logstash.conf
```
16.) Modify ```cloud_id``` and ```cloud_auth``` in ```logstash.conf``` to your own credentials.

17.) Start logstash
```bash
sudo systemctl start logstash.service
```

18.) From a new terminal, Install and activate the app
```bash
onos-app localhost install! target/hub-1.10.0.oar
```
The app has been successfully installed if you see the following message
```bash
| 226 - org.hub.app.hub - 1.10.0 | Starting app
| 226 - org.hub.app.hub - 1.10.0 | Writing header

19.) Check the log file
```bash
cd ~
tail data.csv
```
20.) Go to the [ElasticCloud](https://cloud.elastic.co/home) deployment page and click on the ```Discover``` tab. You should see the data from the log file.
You can import visualization using the dashboard.json file.
