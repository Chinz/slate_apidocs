<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <meta content="IE=edge,chrome=1" http-equiv="X-UA-Compatible">
    <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1">
    <title>API Reference</title>

    <style>
      .highlight table td { padding: 5px; }
.highlight table pre { margin: 0; }
.highlight .gh {
  color: #999999;
}
.highlight .sr {
  color: #f6aa11;
}
.highlight .go {
  color: #888888;
}
.highlight .gp {
  color: #555555;
}
.highlight .gs {
}
.highlight .gu {
  color: #aaaaaa;
}
.highlight .nb {
  color: #f6aa11;
}
.highlight .cm {
  color: #75715e;
}
.highlight .cp {
  color: #75715e;
}
.highlight .c1 {
  color: #75715e;
}
.highlight .cs {
  color: #75715e;
}
.highlight .c, .highlight .cd {
  color: #75715e;
}
.highlight .err {
  color: #960050;
}
.highlight .gr {
  color: #960050;
}
.highlight .gt {
  color: #960050;
}
.highlight .gd {
  color: #49483e;
}
.highlight .gi {
  color: #49483e;
}
.highlight .ge {
  color: #49483e;
}
.highlight .kc {
  color: #66d9ef;
}
.highlight .kd {
  color: #66d9ef;
}
.highlight .kr {
  color: #66d9ef;
}
.highlight .no {
  color: #66d9ef;
}
.highlight .kt {
  color: #66d9ef;
}
.highlight .mf {
  color: #ae81ff;
}
.highlight .mh {
  color: #ae81ff;
}
.highlight .il {
  color: #ae81ff;
}
.highlight .mi {
  color: #ae81ff;
}
.highlight .mo {
  color: #ae81ff;
}
.highlight .m, .highlight .mb, .highlight .mx {
  color: #ae81ff;
}
.highlight .sc {
  color: #ae81ff;
}
.highlight .se {
  color: #ae81ff;
}
.highlight .ss {
  color: #ae81ff;
}
.highlight .sd {
  color: #e6db74;
}
.highlight .s2 {
  color: #e6db74;
}
.highlight .sb {
  color: #e6db74;
}
.highlight .sh {
  color: #e6db74;
}
.highlight .si {
  color: #e6db74;
}
.highlight .sx {
  color: #e6db74;
}
.highlight .s1 {
  color: #e6db74;
}
.highlight .s {
  color: #e6db74;
}
.highlight .na {
  color: #a6e22e;
}
.highlight .nc {
  color: #a6e22e;
}
.highlight .nd {
  color: #a6e22e;
}
.highlight .ne {
  color: #a6e22e;
}
.highlight .nf {
  color: #a6e22e;
}
.highlight .vc {
  color: #ffffff;
}
.highlight .nn {
  color: #ffffff;
}
.highlight .nl {
  color: #ffffff;
}
.highlight .ni {
  color: #ffffff;
}
.highlight .bp {
  color: #ffffff;
}
.highlight .vg {
  color: #ffffff;
}
.highlight .vi {
  color: #ffffff;
}
.highlight .nv {
  color: #ffffff;
}
.highlight .w {
  color: #ffffff;
}
.highlight {
  color: #ffffff;
}
.highlight .n, .highlight .py, .highlight .nx {
  color: #ffffff;
}
.highlight .ow {
  color: #f92672;
}
.highlight .nt {
  color: #f92672;
}
.highlight .k, .highlight .kv {
  color: #f92672;
}
.highlight .kn {
  color: #f92672;
}
.highlight .kp {
  color: #f92672;
}
.highlight .o {
  color: #f92672;
}
    </style>
    <link href="stylesheets/screen.css" rel="stylesheet" media="screen" />
    <link href="stylesheets/print.css" rel="stylesheet" media="print" />
      <script src="javascripts/all.js"></script>
  </head>

  <body class="index" data-languages="[&quot;shell&quot;,&quot;ruby&quot;,&quot;python&quot;,&quot;javascript&quot;]">
    <a href="#" id="nav-button">
      <span>
        NAV
        <img src="images/navbar.png" alt="Navbar" />
      </span>
    </a>

<h1 id="bruce">Bruce</h1>

<h3 id="whatisbruce">What is Bruce?</h3>

<p>Bruce puts forward Redfish aggregation function that allows northbound clients to interact with the system and manage southbound infrastructure by using Redfish compliant APIs. By offering a plug-in framework, Bruce allows for managing equipment with different level of Redfish compliance and even equipment that do not know how to speak Redfish.</p>

<h3 id="howdoiinstallbruce">How do I install Bruce?</h3>

<p>Let's get started.</p>

<h3 id="prerequisites">Prerequisites</h3>

<p>Here is a list of software/packages you need:</p>

<ul>
<li>Ubuntu 18.04 or above</li>

<li>JDK 1.8.60</li>

<li>GIT</li>

<li>CURL</li>

<li>GO Lang </li>

<li>RedisDB</li>

<li>RedisJson</li>

<li>RedisSearch</li>

<li>CONSUL (service registry)</li>

<li>KAFKA (message bus)</li>

<li>TLS certificates in Kafka</li>
</ul>

<blockquote>
  <p><strong><em>NOTE:</em></strong>
      * Make sure there is enough disk space to install these software/packages.
      * If your system is on a private network, consider setting proxy for the  downloads to work.</p>
</blockquote>

<p>See <strong>Setting up dependencies section</strong> for instructions on setting up each sofware/package. </p>

<h3 id="deployingbruce">Deploying Bruce</h3>

<p>Use the following commands to install Bruce.</p>

<blockquote>
  <p><em><strong>Note:</strong></em>
    *You need to be in the user home directory. Make sure to navigate here before you begin installation.</p>
</blockquote>

<ol>
<li><p><code>sh
$ git clone https://github.hpe.com/Bruce/OpenRAFT.git -b development
</code></p></li>

<li><p><code>sh
$ cd OpenRAFT/install
</code></p></li>

<li><pre><code class="sh language-sh">./deploy_bruce.sh &lt;your_tls_certificate_directory_full_path&gt; 
</code></pre>

<p>Replace the placeholder, "<your_tls_certificate_directory_full_path>" with the path where you saved the certificates.(E.g: "<strong>/var/tmp/certificate</strong>").</p>

<p>Here is a list of tasks getting done while Bruce is getting installed:</p></li>
</ol>

<ul>
<li>Killing Bruce processes that are already running .</li>

<li>Setting up GO enviornment paths and directories.</li>

<li>Cloning the BRUCE repository from <strong>github.hpe.com</strong> to location <strong>${HOME}/BRUCE</strong></li>

<li>Generating certificates and private keys for secure communication between a plugin and Bruce services.</li>

<li>Copying certificates,keys and configuration files of Bruce services to <strong>${HOME}/BRUCE/bin</strong> directory.</li>

<li>Editing configuration files: Adding the location of generated keys and certificates.</li>

<li>Building BRUCE binaraies.</li>

<li>Checking if CONSUL, KAFKA &amp; RedisDB are already in running state before starting these services.</li>

<li>Populating RedisDB with default vaules.</li>

<li>Starting Bruce services: Bruce API service is configured on <strong>9091</strong> port and Redfish-Plugin is configured on <strong>9099</strong> port.</li>

<li>Sending CURL command to create an admin session with BRUCE.</li>
</ul>

<blockquote>
  <p><strong><em>NOTE:</em></strong>
      * Deployment log file and Bruce services log files are created and saved in this path: <strong>${HOME}/BRUCE_log</strong>.</p>
</blockquote>

<h3 id="settingupdependencies">Setting up dependencies</h3>

<p>## GIT
 Use the following command to install GIT:
 <code>sh
 $ sudo apt install git
</code>
 ## CURL
  Use the following command to install CURL:
 <code>sh
 $ sudo apt install curl
</code>
 ## GO Lang
 Use the following commands to install GO Lang:
  <code>sh
 $ wget https://dl.google.com/go/go1.12.5.linux-amd64.tar.gz -P /var/tmp
</code>
  <code>sh
 $ sudo tar -C /usr/local -xzf /var/tmp/go1.12.5.linux-amd64.tar.gz
</code>
 <code>sh
 $ export PATH=$PATH:/usr/local/go/bin
</code>
 ## RedisDB
 Use the following commands to install RedisDB:</p>

<ol>
<li><pre><code class="sh language-sh"> $ sudo apt-get install build-essential
</code></pre>

<p>You will be prompted to choose whether to continue or not. Enter 'Y'.</p></li>

<li><p><code>sh
 $ wget -c http://download.redis.io/redis-stable.tar.gz -P /var/tmp
</code></p>

<p><ol>
<li><code>sh
$ sudo tar –c /var/tmp –xzf /var/tmp/redis-stable.tar.gz
</code></li></p>

<p><li><code>sh
$ cd /var/tmp/redis-stable
</code></li></p>

<p><li><code>sh
$ sudo make install
</code></li></p>

<p><li><code>sh
$ sudo utils/install_server.sh 
</code></li></ol>

<p></p></p>

<p><ul>
<li>You will be prompted to select the Redis port for this Redis server instance. Press Enter to select the default port( <strong>6379</strong>).</li></p>

<p><li>Press Enter for the prompts that follow to select default values for Redis config file name, Redis log file name, the data directory for this instance, and the Redis executable path.  </li></p>

<p><li>Press Enter to continue and you have the first instance of Redis server up and running. </li></ul></p>

<p><ol>
<li>You can check if the Redis server is running with the command below.
<code>sh
$  sudo /etc/init.d/redis_6379 status 
</code></li></ol></p>

<p></li></p>

<p><li><p>Install the second instance of Redis server on port <strong>6380</strong> by running the below command.
<code>sh
  $ sudo utils/install_server.sh 
</code></p></p>

<p><ul>
<li>You will be prompted to select the Redis port for this Redis server instance. Enter <strong>6380</strong>.</p>

<p><ul>
<li>Press Enter for the prompts that follow to select default values for Redis config file name, Redis log file name, the data directory for this instance, and the Redis executable path.  </li></p>

<p><li>Press Enter to continue and you have the second instance of Redis server up and running. </li></p>

<p><li>To check if this server instance is running, use the command mentioned in step 7 by replacing "/etc/init.d/redis<em>6379" with "/etc/init.d/redis</em>6380 "</li></ul>
</li></ul></p>

<p></li>
</ol></p>

<blockquote>
  <p><strong><em>NOTE:</em></strong>
  The default port on which Redis server listens is <strong>6379</strong>. This port is used for persistent or on-disk storage.
  The Redis port <strong>6380</strong> is used for in-memory storage.</p>
</blockquote>

<h2 id="redisjson">RedisJson</h2>

<p>Use the following commands to install RedisJson:</p>

<ol>
<li><pre><code class="sh language-sh"> $ cd /var/tmp
</code></pre></li>

<li><pre><code class="sh language-sh"> $ git clone https://github.com/RedisJSON/RedisJSON.git
</code></pre>

<ol>
<li>```sh
$ cd RedisJSON</li></ol>

<pre><code>4. ```sh
$ sudo make
</code></pre>

<p>You can find the compiled RedisJson module library file (rejson.so) inside the folder "src" (src/rejson.so).</p>

<ol>
<li>Stop the first instance of Redis server by running the command below. </li></ol>

<pre><code class="sh language-sh"> $ /etc/init.d/redis_6379 stop
</code></pre>

<ol>
<li>Open the file "<strong>6379.conf</strong>" to edit using the following command.</li></ol>

<pre><code class="sh language-sh"> $ sudo vim /etc/redis/6379.conf
</code></pre>

<ol>
<li>Add below line at the end of the opened file and save.
```sh
loadmodule /usr/lib/rejson.so</li></ol>

<pre><code>8. Start the first instance of Redis server by running the below command.
 ```sh
 $ sudo /etc/init.d/redis_6379 start 
</code></pre>

<ol>
<li>Stop the second instance of Redis server using this command:</li></ol>

<pre><code class="sh language-sh">$ /etc/init.d/redis_6380 stop
</code></pre>

<ol>
<li>Open the file "<strong>6380.conf</strong>" to edit  using the following command.</li></ol>

<pre><code class="sh language-sh">$ sudo vim /etc/redis/6380.conf
</code></pre>

<ol>
<li>Add below line at the end of the opened file and save.</li></ol>

<pre><code class="sh language-sh"> loadmodule /usr/lib/rejson.so
</code></pre>

<ol>
<li>Start the second instance of Redis server by running the below command.</li></ol>

<pre><code class="sh language-sh">  $ sudo  /etc/init.d/redis_6380 start
</code></pre>

<ol>
<li>Check if the second instance of Redis server is running :</li></ol>

<pre><code class="sh language-sh"> $ sudo /etc/init.d/redis_6380 status 
</code></pre></li>
</ol>

<h2 id="redissearch">RedisSearch</h2>

<p>Use the following commands to install RedisSearch:</p>

<ol>
<li><pre><code class="sh language-sh">$ sudo apt-get install cmake
</code></pre>

<p>You will be prompted to choose whether to continue or not. Enter Y.</p>

<ol>
<li><code>sh
cd /var/tmp
</code></li></ol></li>

<li><p><code>sh
$ git clone https://github.com/RediSearch/RediSearch.git
</code></p></li>

<li><p><code>sh
$ cd RediSearch
</code></p></li>

<li><pre><code class="sh language-sh">$ sudo make
</code></pre>

<p>You can find the compiled RedisJson module library file (redisearch.so) inside the folder "src" (src/redisearch.so).</p></li>

<li><p><code>sh
$ sudo cp src/redisearch.so /usr/lib/redisearch.so
</code></p></li>

<li><p>Stop the first instance of Redis server using this command:
<code>sh
$ /etc/init.d/redis_6379 stop
</code></p></li>

<li><p>Open the file "<strong>6379.conf</strong>" to edit  using the following command.
<code>sh
$ sudo vim /etc/redis/6379.conf
</code></p></li>

<li><p>Add below line at the end of the opened file and save.
<code>sh
 loadmodule /usr/lib/redisearch.so
</code></p></li>

<li><p>Start the first instance of Redis server by running the below command.
<code>sh
 $ sudo  /etc/init.d/redis_6379 start 
</code></p></li>

<li><p>Check if the first instance of Redis server is running :
<code>sh
 $ sudo /etc/init.d/redis_6379 status 
</code></p></li>

<li><p>Stop the second instance of Redis server using this command:
<code>sh
$ /etc/init.d/redis_6380 stop
</code></p></li>

<li><p>Open the file "<strong>6380.conf</strong>" to edit  using the following command.
<code>sh
$ sudo vim /etc/redis/6380.conf
</code></p></li>

<li><p>Add below line at the end of the opened file and save.
<code>sh
 loadmodule /usr/lib/redisearch.so
</code></p></li>

<li><p>Start the second instance of Redis server by running the below command.
<code>sh
 $ sudo  /etc/init.d/redis_6380 start 
</code></p></li>

<li><p>Check if the second instance of Redis server is running :
<code>sh
 $ sudo /etc/init.d/redis_6380 status 
</code></p></li>
</ol>

<h2 id="consul">CONSUL</h2>

<p>Use the following commands to install CONSUL:</p>

<ol>
<li><p><code>sh
$ sudo apt-get update
</code></p></li>

<li><p><code>sh
$ sudo apt-get install unzip
</code></p></li>

<li><p><code>sh
$ wget https://releases.hashicorp.com/consul/1.5.3/consul_1.5.3_linux_amd64.zip -P /var/tmp
</code></p></li>

<li><p><code>sh
$ sudo unzip /var/tmp/consul_1.5.3_linux_amd64.zip -d /usr/local/bin
</code></p></li>

<li><p>Create a user for Consul using the following command.You will be prompted for a new password; type the password. 
When prompted for user information such as full name, room number, phone number and more, press Enter to select default values for all the fields.</p>

<pre><code class="sh language-sh">$ sudo adduser consul
</code></pre>

<p>You will be prompted to validate the information provided. Enter 'Y'.</p></li>

<li><p><code>sh
$ consul --version
</code></p></li>

<li><p><code>sh
$ consul -autocomplete-install
</code></p></li>

<li><p><code>sh
$ complete -C /usr/local/bin/consul consul
</code></p></li>

<li><p><code>sh
$ sudo mkdir /opt/consul
</code></p></li>

<li><pre><code class="sh language-sh">$ sudo chown consul:consul /opt/consul
</code></pre></li>

<li><p>Add the user to the <strong>sudo</strong> group using the below command.
<code>sh
$ sudo usermod -aG sudo consul
</code></p></li>

<li><p><code>sh
$ sudo mkdir -p /etc/consul.d/server
</code></p></li>

<li><p>Create a file called "<strong>config.json</strong>" inside "server" directory  (/etc/consul.d/server)  and open it to edit using the following commands.
   <code>sh
    $ cd  /etc/consul.d/server
    $ sudo vim  config.json file 
</code></p></li>

<li><p>Add the below content in the <strong>config.json</strong> file and save. Replace the placeholder, "your<em>server</em>ip" with your <strong>actual server ip</strong> for the fields "<strong>bind<em>addr</strong>" and "<strong>client</em>addr</strong>".
<code>sh
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
</code></p></li>

<li><p>Create a file called "<strong>consul.service</strong>" inside "system" directory (/etc/systemd/system) and open it to edit using the following command.
 <code>sh
 $ sudo vim /etc/systemd/system/consul.service
</code></p></li>

<li><p>Add the below content in the "<strong>consul.service</strong>" file and save
<code>sh
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
</code></p></li>

<li><p>Create a file called "<strong>consulconfig.service</strong>"  inside "system" directory (/etc/systemd/system) and open it to edit using the following command.
 <code>sh
 $ sudo vim /etc/systemd/system/consulconfig.service
</code></p></li>

<li><p>Add the below content in the "<strong>consulconfig.service</strong>" file and save
  <code>sh
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
</code></p></li>

<li><p>Login as consul user using the below command.</p>

<pre><code class="sh language-sh">$ su – consul
</code></pre>

<p>You will be prompted for password, enter the password.</p></li>

<li><p>Start the consulconfig service using the below command.</p>

<pre><code class="sh language-sh">$ sudo systemctl start consulconfig.service
</code></pre>

<p>You will be prompted for password, enter the password.</p></li>

<li><p>You can check the status of consulconfig service using the below command.
<code>sh
$ sudo systemctl status consulconfig.service
</code></p></li>

<li><p>Start the consul service using the below command.
<code>sh
$ sudo systemctl start consul.service
</code></p></li>

<li><p>You can check the status of consul service using the below command.
<code>sh
$ sudo systemctl status consul.service
</code></p></li>

<li><p>Use the following command to make the consul service run automatically after System Boot.
<code>sh
$ sudo systemctl enable consul.service
</code></p></li>

<li><p>Use the following command to make the consulconfig service run automatically after System Boot.
<code>sh
$ sudo systemctl enable consulconfig.service
</code></p></li>
</ol>

<h2 id="kafka">Kafka</h2>

<p>Use the following commands to install Kafka:</p>

<blockquote>
  <p><strong><em>NOTE:</em></strong>
    Make sure JDK is installed.</p>
  
  <ol>
  <li><code>sh
  $ wget http://www-us.apache.org/dist/kafka/2.2.1/kafka_2.12-2.2.1.tgz -P /var/tmp
  </code></li>
  
  <li><code>sh
  $ tar -C /usr/local/kafka -xzf xzf kafka_2.12-2.2.1.tgz
  </code></li>
  
  <li>Create a user for Kafka using the following command.You will be prompted for a new password, type the password. 
  When prompted for user information such as full name, room number, phone number and more, press Enter to select default values for all the fields.
  <code>sh
  $ sudo adduser kafka
  </code>
  You will be prompted to validate the information provided. Enter 'Y'.</li>
  </ol>
</blockquote>

<ol>
<li>Add the user to the <strong>sudo</strong> group using the below command.
<code>sh
$ sudo usermod -aG sudo kafka
</code></li>

<li>Login using the below command. You will be prompted for password, enter the password.
<code>sh
$ su - kafka
</code></li>

<li>Create a file called "<strong>zookeeper.service</strong>" inside <strong>system</strong> directory(/lib/systemd/system) and open it to edit using the below command.
<code>sh
$ sudo vim /lib/systemd/system/zookeeper.service
</code></li>

<li>Add the content below in the "<strong>zookeeper.service</strong>" file.
<code>sh
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
</code></li>

<li>Create a file called "<strong>kafka.service</strong>" inside <strong>system</strong> directory(/lib/systemd/system) and open it to edit using the below command.
<code>sh
$ sudo vim /lib/systemd/system/kafka.service
</code></li>

<li>Add the content below in the "<strong>kafka.service</strong>" file.
<code>sh
# For kafka.service
[Unit]
Requires=zookeeper.service
After=zookeeper.service
[Service]
Type=simple
User=kafka
ExecStart=/bin/sh -c '/usr/local/kafka/bin/kafka-server-start.sh /usr/local/kafka/config/server.properties &gt; /usr/local/kafka/kafka.log 2&gt;&amp;1'
ExecStop=/usr/local/kafka/bin/kafka-server-stop.sh
Restart=on-abnormal
[Install]
WantedBy=multi-user.target
</code></li>

<li>Issue <strong>644</strong> permissions to <strong>kafka.service</strong> and <strong>zookeeper.service</strong> files using the following commands.
<code>sh
$ sudo chmod 644 /lib/systemd/system/kafka.service
$ sudo chmod 644 /lib/systemd/system/zookeeper.service
</code></li>

<li>Create the log file - <strong>kafka.log</strong>
<code>sh
$ sudo touch /usr/local/kafka/kafka.log
</code></li>

<li>Issue <strong>777</strong> permissions to <strong>kafka.log</strong> file
<code>sh
$ sudo chmod **777** /usr/local/kafka/kafka.log
</code></li>

<li>Issue <strong>755</strong> permissions to <strong>server.properties</strong> file
<code>sh
$ sudo chmod 755 /usr/local/kafka/server.properties
</code></li>

<li>Start the zookeeper service using the below command.
<code>sh
$ sudo systemctl status zookeeper.service
</code></li>

<li>Start the kafka service using the below command.
<code>sh
$ sudo systemctl start kafka.service
</code></li>

<li>You can check if the zookeeper service is running using the below command.
<code>sh
$ sudo systemctl status zookeeper.service
</code></li>

<li>You can check if the kafka service is running using the below command.
<code>sh
$ sudo systemctl status kafka.service
</code></li>

<li>Use the following command to make the zookeeper service run automatically after System Boot.
<code>sh
$ sudo systemctl enable zookeeper.service
</code></li>

<li>Use the following command to make the kafka service run automatically after System Boot.
<code>sh
$ sudo systemctl enable kafka.service
</code></li>
</ol>

<h2 id="configuringtlscertificatesinkafka">Configuring TLS Certificates in Kafka</h2>

<p>To configure TLS certificates, use the following commands:</p>

<blockquote>
  <p><em><strong>Note:</strong></em>
    *You need to be in the user home directory. Make sure to navigate here before you begin installation.
    *Make sure Keytool and openSSL packages are installed.</p>
</blockquote>

<ol>
<li><p>Clone <strong>Bruce</strong> repository using the following commands.
<code>sh
$ mkdir /var/tmp/certificate
$ cd /var/tmp
$ git clone https://github.hpe.com/Bruce/OpenRAFT.git -b development
</code></p></li>

<li><p>To generate certificates, run the Bash Script (OpenRaft/lib-messagebus/platforms/certs/tlsconfig.sh) using the following commands.</p>

<pre><code class="sh language-sh">$ cd OpenRAFT/lib-messagebus/platforms/certs/
$ sudo cp tlsconfig.sh rootCA.crt rootCA.key /var/tmp/certificate
$ cd /var/tmp/certificate
$ bash tlsconfig.sh
</code></pre>

<p>You will see multiple prompts while the Bash Script runs:</p>

<p>| Prompt | Action | Note |
| ------ | ----------- | ------- |
| New Keystore password   | Type the password | Remember this password as you need to enter the same for all the subsequent prompts for passwords.|
| First and Last name   | Enter <strong>localhost</strong> |
| Organizational unit | Press Enter |
| Organization  | Press Enter |
| City or locality  | Press Enter |
| State or province  | Press Enter |
| Two-letter country code  | Press Enter |
| Two-letter country code  | Press Enter |
| Is correct?  | Enter yes |
| Trust this certificate? [no]:  | Enter yes | Appears for each certificate being generated. |
 | keystore password, destination keystore password, source keystore password, import keystore password  | Enter the keystore password | Appears for each certificate being generated. |</p>

<blockquote>
  <p><strong>Note:</strong>
  All the generated certificates will be saved inside "<strong>certificate</strong>" folder in this path - "<strong>/var/tmp/certificate</strong>"( It is not mandatory to save certificates only in this path. You can choose a different path to save certificates). Note down this path as you need it later. </p>
</blockquote></li>

<li><p>Stop the Kafka service using the below command. You will be prompted for password, enter Kafka password.
<code>sh
$ sudo systemctl stop kafka.service
</code></p></li>

<li><p><code>sh
$ cd /usr/local/kafka
</code></p></li>

<li><p>Open the file,  "<strong>server.properties</strong>"" to edit using the below command.
<code>sh
$ sudo vim config/server.properties
</code></p></li>

<li><p>Add the below content in the <strong>server.properties</strong> file after the last line (end of file). </p>

<ul>
<li>Replace the placeholder, "<your_keystore_password>" with your <strong>keystore password</strong> for the fields - <strong>ssl.keystore.password</strong>, <strong>ssl.key.password</strong>, and <strong>ssl.truststore.password</strong>.</li>

<li>Add the exactt path of certificates for <strong>ssl.keystore.location</strong> and <strong>ssl.truststore.location</strong> (if you chose a path other than "/var/tmp/certificate/" to generate certificates) - E.g: "<strong><your_certificates_path>/server.keystore.jks</strong>"</li></ul>

<pre><code class="sh language-sh">auto.create.topics.enable=true
listeners=PLAINTEXT://:9092,SSL://localhost:9082
ssl.endpoint.identification.algorithm=
auto.create.topics.enable=true
ssl.keystore.location= /var/tmp/certificate/server.keystore.jks 
ssl.keystore.password=&lt;your_keystore_password&gt;
ssl.key.password=&lt;your_keystore_password&gt;
ssl.truststore.location= /var/tmp/certificate/server.truststore.jks
ssl.truststore.password=&lt;your_keystore_password&gt;
ssl.enabled.protocols=TLSv1.2,TLSv1.1,TLSv1
ssl.keystore.type=JKS
ssl.truststore.type=JKS
</code></pre></li>

<li><p>Start the Kafka service:
<code>sh
sudo systemctl start kafka.service
</code></p></li>

<li><p>Check if the Kafka service is running:
<code>sh
sudo systemctl status kafka.service
</code></p></li>

<li><p>Check if TLS certificates are installed in Kafka using the below command:
<code>sh
$ openssl s_client -debug -connect localhost:9082 -tls1
</code></p></li>
</ol>
 </body>
</html>