How to setup Monitoring for Docker Swarm using Prometheus

Architecture

![image](https://user-images.githubusercontent.com/31573070/56849295-54866d80-6910-11e9-92e8-0a4443efaf6b.png)


 
cAdvisor
-	Container Advisor provides container user an understanding of the resource usage and performance characteristics of their running containers.
-	It is a running daemon that collects, aggregates, processes and exports information about running containers.
-	Specifically, for each container it keeps resource isolation parameters, historical resource usage, histograms of complete historical resource usage and network statistics.
-	This data is exported by container and machine-wide.
-	cAdvisor scrapes information about containers inside the host system and send this data to the Prometheus.

Prometheus
-	Prometheus is a multi-dimensional data model.
-	Prometheus is a monitoring solution to collect metrics from several targets.
-	Time series collection happens by means of a pull model over http.
-	Prometheus is to query docker services on predefined metrics, create graphs, query database, to check health status of services. 
NodeExporter
-	The Node Exporter exposes the Prometheus metrics of the host machine in which it is running and shows the machine’s file system, networking devices, processor, memory usages and others features as well.
-	Node exporter can be run as a docker container while reporting stats for the host system.
Grafana
-	Grafana which is a graphical interface with a dashboard that supports Prometheus as a back-end to query for the data to generate the graph.
-	Prometheus has a built-in graph features that can be access through its web interface but Grafana offers a much more powerful feature.
-	Grafana the face of Prometheus.
Now setup monitoring tolls.
Pre-request:
1.	Clone the YAML files form Github.
git clone https://github.com/manoj3004/Docker_Monitoring.git

2.	You have installed these two pre-requisites
I.	Docker
II.	Docker swarm initialize on master node.
III.	Create docker network
# docker network create --driver=overlay monitor
Step-1: Install cAdvisor, append the following lines to the cAdviser.yml

![image](https://user-images.githubusercontent.com/31573070/56849337-b8109b00-6910-11e9-8953-86f0997504e0.png)

![image](https://user-images.githubusercontent.com/31573070/56849338-bc3cb880-6910-11e9-9b80-33c76196d4c0.png)

1.	Now execute the cAdvicer.yml file.
# docker stack deploy -c cAdviser.yml test
# docker stack ps -f desired-state=running test
2.	After executed, now access to the cAdvisor dashboard.
1.	Container related info. http://3.83.126.162:8080/containers/


![image](https://user-images.githubusercontent.com/31573070/56849360-04f47180-6911-11e9-8ce0-1c67ee29e2bb.png)

2.	Docker related info. http://3.83.126.162:8080/docker/

![image](https://user-images.githubusercontent.com/31573070/56849363-0aea5280-6911-11e9-9704-a823c64a0de4.png)
3.	We can also find the all metrics.
http://3.83.126.162:8080/metrics
Step-3: Now configure Prometheus, prometheus.yml

![image](https://user-images.githubusercontent.com/31573070/56849367-10479d00-6911-11e9-9709-db6eeed12e84.png)


a.	Now execute configure prometheus.yml file.
# docker config create prometheus.yml prometheus.yml
Step-4: Create a very simple monitoring-stack.yml to install Prometheus and Grafana.
Pre-request:
a.	Create volume for Prometheus.
# mkdir -p /data/prometheus/data && chwon -R 1000.1000 /data
b.	Create volume for Grafana.
# mkdir -p /data/grafana/data && chown -R 1000.1000 /data

![image](https://user-images.githubusercontent.com/31573070/56849373-189fd800-6911-11e9-9759-e03a517271b8.png)

a.	Then execute monitoring-stack.yml
# docker stack deploy -c monitoring-stack.yml monitoring
# docker stack ps -f desired-state=running monitoring
b.	Now access the Prometheus dashboard.
http://3.83.126.162:9090



![image](https://user-images.githubusercontent.com/31573070/56849375-1c335f00-6911-11e9-
895a-c3b4fc42d29b.png)

c.	Now click the target option from the status menu, you will find that node exporter state is UP and healthy. 

![image](https://user-images.githubusercontent.com/31573070/56849379-281f2100-6911-11e9-9199-3c941f4169ea.png)

Step-5: Point your browser to 3.83.126.162:3000 to access grafana. 
a.	Login to grafana with the user 'admin' and password as 'admin’. You will be taken to grafana dashboard.


![image](https://user-images.githubusercontent.com/31573070/56849381-2d7c6b80-6911-11e9-897b-c65523db4d0d.png)

b.	Once you have successfully logged in to the Grafana dashboard, click setting icon followed by 'data sources' and finally 'Add data source' as shown in the image below.

![image](https://user-images.githubusercontent.com/31573070/56849382-32411f80-6911-11e9-9f0d-be625fcef920.png)

In the add data source page under config tab, provide a name of the data source, type as prometheus. In Http settings, provide URL as the prometheus server IP and access as direct and click 'Add'.


![image](https://user-images.githubusercontent.com/31573070/56849383-38370080-6911-11e9-9017-dca31036f03b.png)

Once data source is added, click the dashboard tab, + icon and import the data source that you have created just now. 
![image](https://user-images.githubusercontent.com/31573070/56849386-3cfbb480-6911-11e9-9bcb-d786b2f9b61f.png)

Next, you can import the dashboard from the Imixs-Cloud project:

![image](https://user-images.githubusercontent.com/31573070/56849389-4258ff00-6911-11e9-96a0-2aa9cf733158.png)

That's it, you can now see how your docker-swarm is working:

![image](https://user-images.githubusercontent.com/31573070/56849391-471db300-6911-11e9-917f-8fae7e13b9db.png)

![image](https://user-images.githubusercontent.com/31573070/56849393-4edd5780-6911-11e9-9653-de3f400675d3.png)



