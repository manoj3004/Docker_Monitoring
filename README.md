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

2.	You have installed these two pre-requisites
I.	Docker
II.	Docker swarm initialize on master node.
III.	Create docker network
# docker network create --driver=overlay monitor
Step-1: Install cAdvisor, append the following lines to the cAdviser.yml

![image](https://user-images.githubusercontent.com/31573070/56849337-b8109b00-6910-11e9-8953-86f0997504e0.png)

![image](https://user-images.githubusercontent.com/31573070/56849338-bc3cb880-6910-11e9-9b80-33c76196d4c0.png)

