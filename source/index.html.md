---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - ruby
  - python
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---
# Bruce
### What is Bruce?
Bruce puts forward Redfish aggregation function that allows northbound clients to interact with the system and manage southbound infrastructure by using Redfish compliant APIs. By offering a plug-in framework, Bruce allows for managing equipment with different level of Redfish compliance and even equipment that do not know how to speak Redfish.
### How do I install Bruce?
Let's get started.
### Prerequisites
Here is a list of software/packages you need:
- 	Ubuntu 18.04 or above
- 	JDK 1.8.60
- 	GIT
- 	CURL
- 	GO Lang 
- 	RedisDB
- 	RedisJson
- 	RedisSearch
- 	CONSUL (service registry)
- 	KAFKA (message bus)
- 	TLS certificates in Kafka


 >**_NOTE:_**
    * Make sure there is enough disk space to install these software/packages.
    * If your system is on a private network, consider setting proxy for the  downloads to work.
  

 See **Setting up dependencies section** for instructions on setting up each sofware/package. 
 
### Deploying Bruce
 Use the following commands to install Bruce.
 
 >_**Note:**_
  *You need to be in the user home directory. Make sure to navigate here before you begin installation.
  
1. ```sh
   $ git clone https://github.hpe.com/Bruce/OpenRAFT.git -b development
   ```
2. ```sh
   $ cd OpenRAFT/install
   ```
3. ```sh
   ./deploy_bruce.sh <your_tls_certificate_directory_full_path> 
   ```
    Replace the placeholder, "<your_tls_certificate_directory_full_path>" with the path where you saved the certificates.(E.g: "**/var/tmp/certificate**").
    
  Here is a list of tasks getting done while Bruce is getting installed:
 - Killing Bruce processes that are already running .
 - Setting up GO enviornment paths and directories.
 - Cloning the BRUCE repository from **github.hpe.com** to location **${HOME}/BRUCE**
 - Generating certificates and private keys for secure communication between a plugin and Bruce services.
 - Copying certificates,keys and configuration files of Bruce services to **${HOME}/BRUCE/bin** directory.
 - Editing configuration files: Adding the location of generated keys and certificates.
 - Building BRUCE binaraies.
 - Checking if CONSUL, KAFKA & RedisDB are already in running state before starting these services.
 - Populating RedisDB with default vaules.
 - Starting Bruce services: Bruce API service is configured on **9091** port and Redfish-Plugin is configured on **9099** port.
 - Sending CURL command to create an admin session with BRUCE.
 
  >**_NOTE:_**
    * Deployment log file and Bruce services log files are created and saved in this path: **${HOME}/BRUCE_log**.
    
 
 
### Setting up dependencies
 

 ## GIT
 Use the following command to install GIT:
 ```sh
 $ sudo apt install git
 ```
 ## CURL
  Use the following command to install CURL:
 ```sh
 $ sudo apt install curl
 ```
 ## GO Lang
 Use the following commands to install GO Lang:
  ```sh
 $ wget https://dl.google.com/go/go1.12.5.linux-amd64.tar.gz -P /var/tmp
 ```
  ```sh
 $ sudo tar -C /usr/local -xzf /var/tmp/go1.12.5.linux-amd64.tar.gz
 ```
 ```sh
 $ export PATH=$PATH:/usr/local/go/bin
 ```
 ## RedisDB
 Use the following commands to install RedisDB:
 1. ```sh
 	$ sudo apt-get install build-essential
    ```
    You will be prompted to choose whether to continue or not. Enter 'Y'.
    
 2. ```sh
 	$ wget -c http://download.redis.io/redis-stable.tar.gz -P /var/tmp
    ```
3. ```sh
 	$ sudo tar –c /var/tmp –xzf /var/tmp/redis-stable.tar.gz
    ```
4. ```sh
 	$ cd /var/tmp/redis-stable
    ```
5. ```sh
 	$ sudo make install
    ```
6. ```sh
 	$ sudo utils/install_server.sh 
    ```
     - You will be prompted to select the Redis port for this Redis server instance. Press Enter to select the default port( **6379**).
     - Press Enter for the prompts that follow to select default values for Redis config file name, Redis log file name, the data directory for this instance, and the Redis executable path.  
     - Press Enter to continue and you have the first instance of Redis server up and running. 
  7. You can check if the Redis server is running with the command below.
     ```sh
 	 $  sudo /etc/init.d/redis_6379 status 
     ```
 8. Install the second instance of Redis server on port **6380** by running the below command.
    ```sh
 	 $ sudo utils/install_server.sh 
     ```
      - You will be prompted to select the Redis port for this Redis server instance. Enter **6380**.
     - Press Enter for the prompts that follow to select default values for Redis config file name, Redis log file name, the data directory for this instance, and the Redis executable path.  
     - Press Enter to continue and you have the second instance of Redis server up and running. 
     - To check if this server instance is running, use the command mentioned in step 7 by replacing "/etc/init.d/redis_6379" with "/etc/init.d/redis_6380 "
> **_NOTE:_**
The default port on which Redis server listens is **6379**. This port is used for persistent or on-disk storage.
The Redis port **6380** is used for in-memory storage.


## RedisJson
Use the following commands to install RedisJson:
 1. ```sh
 	$ cd /var/tmp
    ```
    
 2. ```sh
 	$ git clone https://github.com/RedisJSON/RedisJSON.git
    ```
3. ```sh
 	$ cd RedisJSON
    ```
4. ```sh
   $ sudo make
    ```
    You can find the compiled RedisJson module library file (rejson.so) inside the folder "src" (src/rejson.so).
    
5. Stop the first instance of Redis server by running the command below. 
    ```sh
 	$ /etc/init.d/redis_6379 stop
    ```
6. Open the file "**6379.conf**" to edit using the following command.
   ```sh
 	$ sudo vim /etc/redis/6379.conf
    ```
7. Add below line at the end of the opened file and save.
     ```sh
 	loadmodule /usr/lib/rejson.so
    ```
8. Start the first instance of Redis server by running the below command.
     ```sh
 	$ sudo /etc/init.d/redis_6379 start 
    ```
9. Stop the second instance of Redis server using this command:
   ```sh
   $ /etc/init.d/redis_6380 stop
   ```
10. Open the file "**6380.conf**" to edit  using the following command.
    ```sh
    $ sudo vim /etc/redis/6380.conf
    ```
11. Add below line at the end of the opened file and save.
    ```sh
 	loadmodule /usr/lib/rejson.so
    ```
12. Start the second instance of Redis server by running the below command.
    ```sh
 	 $ sudo  /etc/init.d/redis_6380 start
    ```
13. Check if the second instance of Redis server is running :
    ```sh
 	$ sudo /etc/init.d/redis_6380 status 
    ```
## RedisSearch
Use the following commands to install RedisSearch:
1. ```sh
   $ sudo apt-get install cmake
    ```
      You will be prompted to choose whether to continue or not. Enter Y.
    
 2. ```sh
 	cd /var/tmp
    ```
3. ```sh
   $ git clone https://github.com/RediSearch/RediSearch.git
    ```
4. ```sh
   $ cd RediSearch
    ```
5. ```sh
   $ sudo make
    ```
   You can find the compiled RedisJson module library file (redisearch.so) inside the folder "src" (src/redisearch.so).
   
6. ```sh
   $ sudo cp src/redisearch.so /usr/lib/redisearch.so
    ```
7. Stop the first instance of Redis server using this command:
   ```sh
   $ /etc/init.d/redis_6379 stop
   ```
8. Open the file "**6379.conf**" to edit  using the following command.
    ```sh
    $ sudo vim /etc/redis/6379.conf
    ```
9. Add below line at the end of the opened file and save.
    ```sh
 	loadmodule /usr/lib/redisearch.so
    ```
10. Start the first instance of Redis server by running the below command.
    ```sh
 	$ sudo  /etc/init.d/redis_6379 start 
    ```
11. Check if the first instance of Redis server is running :
    ```sh
 	$ sudo /etc/init.d/redis_6379 status 
    ```
12. Stop the second instance of Redis server using this command:
   ```sh
   $ /etc/init.d/redis_6380 stop
   ```
13. Open the file "**6380.conf**" to edit  using the following command.
    ```sh
    $ sudo vim /etc/redis/6380.conf
    ```
14. Add below line at the end of the opened file and save.
    ```sh
 	loadmodule /usr/lib/redisearch.so
    ```
15. Start the second instance of Redis server by running the below command.
    ```sh
 	$ sudo  /etc/init.d/redis_6380 start 
    ```
16. Check if the second instance of Redis server is running :
    ```sh
 	$ sudo /etc/init.d/redis_6380 status 
    ```

## CONSUL
Use the following commands to install CONSUL:

1. ```sh
   $ sudo apt-get update
    ```
2. ```sh
   $ sudo apt-get install unzip
    ```
3. ```sh
   $ wget https://releases.hashicorp.com/consul/1.5.3/consul_1.5.3_linux_amd64.zip -P /var/tmp
    ```
4. ```sh
   $ sudo unzip /var/tmp/consul_1.5.3_linux_amd64.zip -d /usr/local/bin
    ```
5. 	Create a user for Consul using the following command.You will be prompted for a new password; type the password. 
    When prompted for user information such as full name, room number, phone number and more, press Enter to select default values for all the fields.
    ```sh
    $ sudo adduser consul
    ```
    
     You will be prompted to validate the information provided. Enter 'Y'.
    
6. ```sh
   $ consul --version
   ```
7. ```sh
   $ consul -autocomplete-install
   ```
8. ```sh
   $ complete -C /usr/local/bin/consul consul
   ```
9. ```sh
   $ sudo mkdir /opt/consul
   ```
10. ```sh
    $ sudo chown consul:consul /opt/consul
    ```
   
11. Add the user to the **sudo** group using the below command.
    ```sh
    $ sudo usermod -aG sudo consul
    ```
12. ```sh
    $ sudo mkdir -p /etc/consul.d/server
    ```
13.  Create a file called "**config.json**" inside "server" directory  (/etc/consul.d/server)  and open it to edit using the following commands.
       ```sh
        $ cd  /etc/consul.d/server
        $ sudo vim  config.json file 
     ```
14. Add the below content in the **config.json** file and save. Replace the placeholder, "your_server_ip" with your **actual server ip** for the fields "**bind_addr**" and "**client_addr**".
    ```sh
    {
    	"bind_addr": "your_server_ip",
    	"client_addr": "your_server_ip",
    	"datacenter": "dc1",
    	"data_dir": "/opt/consul",
    	"encrypt": "EXz7LFN8hpQ4id8EDYiFoQ==",
    	"log_level": "INFO",
    	"enable_syslog": true,
    	"enable_debug": true,
    	"node_name": "ConsulServer1",
    	"ui": true,
    	"server": true,
    	"bootstrap": true,
    	"bootstrap_expect": 1,
    	"leave_on_terminate": false,
    	"skip_leave_on_interrupt": true,
    	"rejoin_after_leave": true
      }
    ```
15. Create a file called "**consul.service**" inside "system" directory (/etc/systemd/system) and open it to edit using the following command.
     ```sh
     $ sudo vim /etc/systemd/system/consul.service
     ```
16. Add the below content in the "**consul.service**" file and save
    ```sh
    [Unit]
    Description=Consul server process
    Requires=network-online.target
    After=network-online.target
    [Service]
    Type=simple
    Restart=on-failure
    ExecStart=/usr/local/bin/consul agent -dev -ui
    ExecReload=/usr/local/bin/consul reload
    KillMode=process
    KillSignal=SIGTERM
    User=consul
    Group=consul
    LimitNOFILE=65536
    [Install]
    WantedBy=multi-user.target
    ```
17. Create a file called "**consulconfig.service**"  inside "system" directory (/etc/systemd/system) and open it to edit using the following command.
     ```sh
     $ sudo vim /etc/systemd/system/consulconfig.service
     ```
18. Add the below content in the "**consulconfig.service**" file and save
      ```sh
     [Unit]
     Description=Consul server config process
     Requires=network-online.target
     After=network-online.target
     [Service]
     Type=simple
     Restart=on-failure
    ExecStart=/usr/local/bin/consul agent -config-dir /etc/consul.d/server
     KillSignal=SIGTERM
     User=consul
     Group=consul
     [Install]
     WantedBy=multi-user.target
       ```
19. Login as consul user using the below command.
    ```sh
    $ su – consul
    ```
    You will be prompted for password, enter the password.
    
20. Start the consulconfig service using the below command.
    ```sh
    $ sudo systemctl start consulconfig.service
    ```
    You will be prompted for password, enter the password.
    
21. You can check the status of consulconfig service using the below command.
    ```sh
    $ sudo systemctl status consulconfig.service
    ```
22. Start the consul service using the below command.
    ```sh
    $ sudo systemctl start consul.service
    ```
23. You can check the status of consul service using the below command.
    ```sh
    $ sudo systemctl status consul.service
    ```
24. Use the following command to make the consul service run automatically after System Boot.
    ```sh
    $ sudo systemctl enable consul.service
    ```
25. Use the following command to make the consulconfig service run automatically after System Boot.
    ```sh
    $ sudo systemctl enable consulconfig.service
    ```
    
## Kafka
Use the following commands to install Kafka:
> **_NOTE:_**
  Make sure JDK is installed.
1. ```sh
   $ wget http://www-us.apache.org/dist/kafka/2.2.1/kafka_2.12-2.2.1.tgz -P /var/tmp
   ```
2. ```sh
   $ tar -C /usr/local/kafka -xzf xzf kafka_2.12-2.2.1.tgz
   ```
3. Create a user for Kafka using the following command.You will be prompted for a new password, type the password. 
    When prompted for user information such as full name, room number, phone number and more, press Enter to select default values for all the fields.
   ```sh
   $ sudo adduser kafka
   ```
    You will be prompted to validate the information provided. Enter 'Y'.
    
4. Add the user to the **sudo** group using the below command.
   ```sh
   $ sudo usermod -aG sudo kafka
   ```
5. Login using the below command. You will be prompted for password, enter the password.
   ```sh
   $ su - kafka
   ```
6. Create a file called "**zookeeper.service**" inside **system** directory(/lib/systemd/system) and open it to edit using the below command.
   ```sh
   $ sudo vim /lib/systemd/system/zookeeper.service
   ```
7. Add the content below in the "**zookeeper.service**" file.
   ```sh
   # For zookeeper.service
   [Unit]
   Requires=network.target remote-fs.target
   After=network.target remote-fs.target
   [Service]
   Type=simple
   User=kafka
   ExecStart=/usr/local/kafka/bin/zookeeper-server-start.sh /usr/local/kafka/config/zookeeper.properties
   ExecStop=/usr/local/kafka/bin/zookeeper-server-stop.sh
   Restart=on-abnormal
   [Install]
   WantedBy=multi-user.target
   ```
8. Create a file called "**kafka.service**" inside **system** directory(/lib/systemd/system) and open it to edit using the below command.
   ```sh
   $ sudo vim /lib/systemd/system/kafka.service
   ```
9. Add the content below in the "**kafka.service**" file.
   ```sh
   # For kafka.service
   [Unit]
   Requires=zookeeper.service
   After=zookeeper.service
   [Service]
   Type=simple
   User=kafka
   ExecStart=/bin/sh -c '/usr/local/kafka/bin/kafka-server-start.sh /usr/local/kafka/config/server.properties > /usr/local/kafka/kafka.log 2>&1'
   ExecStop=/usr/local/kafka/bin/kafka-server-stop.sh
   Restart=on-abnormal
   [Install]
   WantedBy=multi-user.target
   ```
10. Issue **644** permissions to **kafka.service** and **zookeeper.service** files using the following commands.
    ```sh
    $ sudo chmod 644 /lib/systemd/system/kafka.service
    $ sudo chmod 644 /lib/systemd/system/zookeeper.service
    ```
11. Create the log file - **kafka.log**
    ```sh
    $ sudo touch /usr/local/kafka/kafka.log
    ```
12. Issue **777** permissions to **kafka.log** file
    ```sh
    $ sudo chmod **777** /usr/local/kafka/kafka.log
    ```
13. Issue **755** permissions to **server.properties** file
    ```sh
    $ sudo chmod 755 /usr/local/kafka/server.properties
    ```
14. Start the zookeeper service using the below command.
    ```sh
    $ sudo systemctl status zookeeper.service
    ```
15. Start the kafka service using the below command.
    ```sh
    $ sudo systemctl start kafka.service
    ```
16. You can check if the zookeeper service is running using the below command.
    ```sh
    $ sudo systemctl status zookeeper.service
    ```
17. You can check if the kafka service is running using the below command.
    ```sh
    $ sudo systemctl status kafka.service
    ```
18. Use the following command to make the zookeeper service run automatically after System Boot.
    ```sh
    $ sudo systemctl enable zookeeper.service
    ```
19. Use the following command to make the kafka service run automatically after System Boot.
    ```sh
    $ sudo systemctl enable kafka.service
    ```
## Configuring TLS Certificates in Kafka
To configure TLS certificates, use the following commands:

>_**Note:**_
  *You need to be in the user home directory. Make sure to navigate here before you begin installation.
  *Make sure Keytool and openSSL packages are installed.

1. Clone **Bruce** repository using the following commands.
   ```sh
   $ mkdir /var/tmp/certificate
   $ cd /var/tmp
   $ git clone https://github.hpe.com/Bruce/OpenRAFT.git -b development
   ```
2. To generate certificates, run the Bash Script (OpenRaft/lib-messagebus/platforms/certs/tlsconfig.sh) using the following commands.
   ```sh
   $ cd OpenRAFT/lib-messagebus/platforms/certs/
   $ sudo cp tlsconfig.sh rootCA.crt rootCA.key /var/tmp/certificate
   $ cd /var/tmp/certificate
   $ bash tlsconfig.sh
   ```
   You will see multiple prompts while the Bash Script runs:
     
    | Prompt | Action | Note |
    | ------ | ----------- | ------- |
   | New Keystore password   | Type the password | Remember this password as you need to enter the same for all the subsequent prompts for passwords.|
   | First and Last name   | Enter **localhost** |
   | Organizational unit | Press Enter |
   | Organization  | Press Enter |
   | City or locality  | Press Enter |
   | State or province  | Press Enter |
   | Two-letter country code  | Press Enter |
   | Two-letter country code  | Press Enter |
   | Is correct?  | Enter yes |
   | Trust this certificate? [no]:  | Enter yes | Appears for each certificate being generated. |
     | keystore password, destination keystore password, source keystore password, import keystore password  | Enter the keystore password | Appears for each certificate being generated. |
     
    >**Note:**
    All the generated certificates will be saved inside "**certificate**" folder in this path - "**/var/tmp/certificate**"( It is not mandatory to save certificates only in this path. You can choose a different path to save certificates). Note down this path as you need it later. 
    
3. Stop the Kafka service using the below command. You will be prompted for password, enter Kafka password.
   ```sh
   $ sudo systemctl stop kafka.service
   ```
4. ```sh
   $ cd /usr/local/kafka
   ```
5. Open the file,  "**server.properties**"" to edit using the below command.
   ```sh
   $ sudo vim config/server.properties
   ```
6. Add the below content in the **server.properties** file after the last line (end of file). 
   - Replace the placeholder, "<your_keystore_password>" with your **keystore password** for the fields - **ssl.keystore.password**, **ssl.key.password**, and **ssl.truststore.password**.
   - Add the exactt path of certificates for **ssl.keystore.location** and **ssl.truststore.location** (if you chose a path other than "/var/tmp/certificate/" to generate certificates) - E.g: "**<your_certificates_path>/server.keystore.jks**"
   
   ```sh
   auto.create.topics.enable=true
   listeners=PLAINTEXT://:9092,SSL://localhost:9082
   ssl.endpoint.identification.algorithm=
   auto.create.topics.enable=true
   ssl.keystore.location= /var/tmp/certificate/server.keystore.jks 
   ssl.keystore.password=<your_keystore_password>
   ssl.key.password=<your_keystore_password>
   ssl.truststore.location= /var/tmp/certificate/server.truststore.jks
   ssl.truststore.password=<your_keystore_password>
   ssl.enabled.protocols=TLSv1.2,TLSv1.1,TLSv1
   ssl.keystore.type=JKS
   ssl.truststore.type=JKS
   ```
7. Start the Kafka service:
   ```sh
   sudo systemctl start kafka.service
   ```
8. Check if the Kafka service is running:
   ```sh
   sudo systemctl status kafka.service
   ```
9. Check if TLS certificates are installed in Kafka using the below command:
   ```sh
   $ openssl s_client -debug -connect localhost:9082 -tls1
   ```


