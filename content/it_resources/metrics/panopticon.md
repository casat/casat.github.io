---
title: "Panopticon"
weight: 13
date: "2022-08-24"
author: "John Marks"
---
08/30/22

Panopticon is the monitoring server that we use to monitor the logs and metrics of our many servers. The site works on a stack of Linux, Apache, Prometheus, Loki, and Grafana. The server is accessible at panopticon.casat.org. 

In order to collect the metrics, Prometheus utilizes a few 'exporters' which are small apps used to either collect information or forward information on to the Prometheus server. On the panopticon server there is the Blackbox Exporter installed to periodically ping our remote servers and collect connectivity statistics. Node Exporter is the exporter installed our remote servers for collecting server metrics. Promtail is an Exporter installed on the remote severs which forwards copies of the server logs to the panopticon server. Grafana is the application used to display the metrics collected by Prometheus and the exporters on the website. And Loki is an addon application for Grafana that allows the display and browsing of the logfiles forwarded by Promtail.

At this time the best way to install the necessary monitoring agents on a remote server is to use the Ansible playbook monitoring.yml. After running the playbook on a target server you will need to manually update the UFW firewall on the panoption server to accept connections from the target's IP address. The instructions can be found below:

- Run the monitoring.yml playbook
- Add the staging server IP address to the panopticon server firewall 'allowed' list
    - ```sudo ufw allow from $IP proto tcp to any port 3100```
- Remove the old server IP address from the panopticon server firewall 'allowed' list
    - ```sudo ufw status numbered```
    - ```sudo ufw delete #```