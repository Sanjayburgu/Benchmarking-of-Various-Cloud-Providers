# Benchmarking-of-Various-Cloud-Providers
A sample comparison between various public cloud providers when a client wants to deploy Cassandra based system. We Compare and analyze cloud service providers in terms of performance and cost by using Yahoo! Cloud Serving Benchmark(**YCSB**).

## Steps involved in the process of Benchmarking.
1. Creating Instances or Vm's in the cloud for the setup up of cassandra clusters.
2. Connection to these instances.
3. Installing cassandra on these Instances or VM's.
4. Installing YCSB and benchmark Cassandra.

*Note: For the purpose of this Benchmarking we are limiting ourself to three public cloud providers an limiting all the Instance Operating systems to ubuntu.*
* Amazon Web Services.
* Microsoft Azure. 
* Google Cloud Provider.

#### STEP 1: Creating Instances in each cloud provider is deifferent. How to launch Instance in each clod provider is mentioned within each cloud provider details.
  

#### STEP 2: For connecting to these instances, we can use Putty if you are using a Windows operating System or you can directly ssh into the intances if you are Using Linux, Ubuntu or Mac opersating Systems.
* For connecting to Instances in AWS, the following syntax can be used to ssh into linux instance.
  ```
  chmod 0400 your-key-pair.pem (To modify the permissions)
  ssh -i /path/<your-key-pair>.pem ec2-user@<your-InstanceIP>
  ```
  A detailed explanation of how to connect to linux instance from Windows OS in AWS can be found here -> [Connecting to Linux Instance from Windows](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html)
  
* For connecting to Instances in Microsoft Azure, the Easiest way to secure you instance is with help of a password.
  * If you are using windows OS use putty to ssh into the instance and then authenticate with your Username and Password.
  * If you are using Linux/Ubuntu/Mac OS use terminal to ssh into the instance and then authenticate with your Username and Password.

 * For connecting to Instances in GCP, A detailed explanation of how to connect to linux instance can be found here -> [Connecting to Linux Instance from Windows or Linux](https://cloud.google.com/compute/docs/instances/connecting-to-instance#thirdpartytools)
   

#### STEP 3: Installing cassandra on these Instances or VM's.  
After logging into the Intstance you need to install cassandra for setting up the cluster. The proess of installing and seting up the cassandra cluster is given below.
 
1. Oracle Java 8 and JNI are prerequisites for Cassandra v3.
  * To install Java and check wether it is properly installed or not:
```
sudo apt-add-repository ppa:webupd8team/java  
sudo apt-get update  
sudo apt-get install oracle-java8-installer
java -version
```
  * Install JNA using: ```sudo apt-get install libjna-java -y```

2. For Installing Cassandra we need to setup PPA's and keys for Verification. It is done using the following commands.
```
echo "deb http://www.apache.org/dist/cassandra/debian 30x main" | sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list  
echo "deb-src http://www.apache.org/dist/cassandra/debian 30x main" | sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list

gpg --keyserver pgp.mit.edu --recv-keys F758CE318D77295D  
gpg --export --armor F758CE318D77295D | sudo apt-key add -  

gpg --keyserver pgp.mit.edu --recv-keys 2B5C1B00  
gpg --export --armor 2B5C1B00 | sudo apt-key add -  

gpg --keyserver pgp.mit.edu --recv-keys 0353B12C  
gpg --export --armor 0353B12C | sudo apt-key add –
```

3. Now we can Install Cassandra using the following commands:
```
sudo apt-get update
sudo apt-get install Cassandra
```

  * To check if cassandra is running: ```sudo service cassandra status ```

  * To check the nodes: ```sudo nodetool status```

4. configuring Cassandra for Multi-Node setup:
* Stop Cassandra using: ```sudo service cassandra stop```
* Find your ethernet card interface ID using ```ifconfig```, it should be eth(x).

5. We need to edit Cassandra's configuration cassandra.yaml file for configuring cassandra in a Multi-Node setup. ```sudo vim /etc/cassandra/cassandra.yaml```
  * Change the cluster name.
  * Add the IP addresses of the seed nodes.
  * Comment out the listen_address.
  * Add the listen interface.
  * Start the RPC service.
  * Set the RPC interface.
  * Set the endpoint snitch.
  * By editing the file: ```sudo vim /etc/cassandra/cassandra.yaml``` (Shown Below)
  ![yaml file](https://user-images.githubusercontent.com/31011479/29745950-1292be3c-8a7e-11e7-91ad-48c265890fe7.png)
 
* Now we need to delete all Cassandra previous system configurations. ```sudo rm -rf /var/lib/cassandra/data/system/```
* Start Cassandra: ```sudo service cassandra start```
* check the nodes using: ```sudo nodetool status```
The output of the nodetool status gives the list of nodes present in the cluster. A sample Image of 3 node cluster is shown below.
![cassandra nodes](https://user-images.githubusercontent.com/31011479/29745949-129213b0-8a7e-11e7-918d-6fde0ac694b8.png)


