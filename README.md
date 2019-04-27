How to setup Monitoring for Docker Swarm using Prometheus

Architecture

![image](https://user-images.githubusercontent.com/31573070/56849295-54866d80-6910-11e9-92e8-0a4443efaf6b.png)


 
cAdvisor
-	Container Advisor provides container user an understanding of the resource usage and performance characteristics of their running containers.
-	It is a running daemon that collects, aggregates, processes and exports information about running containers.
-	Specifically, for each container it keeps resource isolation parameters, historical resource usage, histograms of complete historical resource usage and network statistics.
-	This data is exported by container and machine-wide.
-	cAdvisor scrapes information about containers inside the host system and send this data to the Prometheus.
