## Simple ELK stack bootstrapped to monitor Docker Datacenter

**Summary:** Easily bootstrap an ELK stack and configure Docker UCP to stream logs
to it. You can bootstrap ELK on UCP itself but it would be recommended to run it
outside the cluster. Enabling logs from UCP would stream system containers logs
to ELK. If you'd like to stream all cluster container logs, please follow Step#3.

**Step 1:**  On the host that you'd like to install ELK on run the following
command:

```
git clone https://github.com/nicolaka/elk-stack.git
cd elk-stack
docker-compose up -d
```

In the sample compose file we use the following ports:

```
kibana: tcp/5601(Host Mounted)
logstash: tcp/5600 (Host Mounted)
elasticsearch: tcp/9200, 9300 (Not Host Mounted)
```

**Step 2:** Set up UCP to stream its logs to ELK:

In UCP dashboard, click on Settings  > Logs , then plug in the IP address of the host that is running ELK (see below)


**Step 3(Optional):** If you'd like to stream Docker engine logs to ELK stack you
need to add the following to your ` /etc/docker/daemon.json` file on each of the
Docker engines in your cluster followed by an engine restart.

```
{
 "log-driver": "syslog",
  "log-opts": {
    "syslog-address": "tcp://<ELK_IP_ADDRESS>:5600"
}
}

``` 

**Step 4:** Proceed to access Kibana at `http://<ELK_IP_ADDRESS>:5601`. Enjoy!
 
 
 
