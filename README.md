Prometheus Monitoring

https://kjanshair.github.io/2018/02/20/prometheus-monitoring/

Monitoring with Prometheus Monitoring applications & application servers is an important part of the today’s DevOps workflow which includes continuous monitoring of your applications and servers for application exceptions, server’s CPU & memory usage or storage spikes. You would also like to get some type of notification if some CPU or memory spike occurs at a given time or a service of your application stops responding so you can perform appropriate actions against those failures or exceptions.

There are a number of monitoring tools out there such Amazon CloudWatch, Nagios, New Relic, Prometheus and others. Some are free & open-source and some are paid. Here, we will take a look at the Prometheus and how it is different.
Prometheus Monitoring

https://kjanshair.github.io/2018/02/20/prometheus-monitoring/

Monitoring with Prometheus Monitoring applications & application servers is an important part of the today’s DevOps workflow which includes continuous monitoring of your applications and servers for application exceptions, server’s CPU & memory usage or storage spikes. You would also like to get some type of notification if some CPU or memory spike occurs at a given time or a service of your application stops responding so you can perform appropriate actions against those failures or exceptions.

There are a number of monitoring tools out there such Amazon CloudWatch, Nagios, New Relic, Prometheus and others. Some are free & open-source and some are paid. Here, we will take a look at the Prometheus and how it is different.

What exactly Prometheus is and how it is different? Prometheus is a popular CNCF project, free & open-source monitoring tool for monitoring dozens of services and is completely written in Golang. Some of its components are written in Ruby and other programming languages but most of the Prometheus components are written in Go. That means, you have a single binary executable, you download and run Prometheus as that simple. Prometheus is also fully Docker compatible. A number of Prometheus components with the Prometheus server itself are available on the Docker Hub that we are going to see here in action.

You will see how to spin-up a minimal Prometheus server with Node Exporter and Grafana components in Docker containers to monitor a stand-alone Linux Ubuntu 16.04 server. Let’s take a look at what are the mandatory components in Prometheus from ground-up.

Prometheus Components Prometheus Server Prometheus has a main central component called Prometheus Server. As a monitoring service, Prometheus server Monitor particular thing, that thing could be anything i.e. it could be a an entire Linux server, a stand-alone Apache server, a single process, it could be a database service or some other system unit that you want to monitor. In Prometheus terms, we call the main monitoring service as the Prometheus Server and the stuff that Prometheus monitors are called Targets. Hence Prometheus server monitors Targets. As said before, Targets could be anything i.e. it could be a single server or a targets for probing of endpoints over HTTP, HTTPS, DNS, TCP and ICMP (Black-Box Exporter) or it could be a simple HTTP endpoint that an application exposes through which the Prometheus server get the application health status from.

Each unit of a target such as current CPU status, memory usage (In case of a Linux server Prometheus target) or any other specific unit that you would like to monitor is called a metric. So Prometheus server collects metrics from targets (over HTTP), stores them locally or remotely and displays them back in the Prometheus server.

Prometheus server scrapes targets at a given interval that you define to collect metrics from specific targets and store them in a time-series database. You define the targets and the time-interval for scraping metrics in the prometheus.yml configuration file.

prometheus-app-api

You get the metrics details by querying from the Prometheus’s time-series database where the Prometheus stores metrics and you use a query language called PromQL in the Prometheus server to query metrics about the targets. In other words, you ask the Prometheus server via PromQL to show us the status of a particular target at a given time.

Prometheus provides client-libraries in a number of languages that you can use to provide health-status of your application. But Prometheus is not only about application monitoring, you can use something called Exporters to monitor third-party systems (Such as a Linux Server, MySQL daemon etc). An Exporter is a piece of software that gets existing metrics from a third-party system and export them to the metric format that the Prometheus server can understand.

prometheus-app-exporters

A sample metric from a Prometheus server could be the current usage of free memory or file-system free via a Node Exporter in the Prometheus server.

It is important to know that metrics that you get from a third-party system are different from the Prometheus metrics. Because Prometheus uses a standard data-model with a key-value based metrics that might not match with the third-party system this is why you use exporters to convert them. To keep things simple, I won’t go into the details of each syntax of a Prometheus metrics and how they are different.

Visualization Layer with Grafana You use Grafana the visualization layer as the 3rd component to visualize metrics stored in the Prometheus time-series database. Instead of writing PromQL queries directly into the Prometheus server, you use Grafana UI boards to query metrics from Prometheus server and visualize them in the Grafana Dashboard as a whole as we will see it in action shortly.

Alert Management with Prometheus Alert Manager Prometheus also has a Alert Management component called AlertManager for firing alerts via Email or Slack or other notification clients. You define the Alert Rules in a file called alert.rules through which the Prometheus server reads the alert configurations and fire alerts at appropriate times via the Alert Manager component. For example, if the Prometheus server finds the value of a metric greater than the threshold that you defined in the alert.rules file for more than 30 seconds, it will trigger the Alert Manager to fire an alert about the threshold and the metric. We will see how AlertManager works with Prometheus and how do we setup in the Prometheus stack in some later post.

The above 3 components are the basis of the entire Prometheus Monitoring system. You need the central Prometheus server, a target and a visualization layer. Lets see how to setup a minimal Prometheus stack with the above 3 components to monitor a simple Ubuntu 16.04 server using docker-compose.

