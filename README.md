# apache-prometheus-grafana

You can setup a prometheus-grafana monitoring setup which will collect apache and prometheus metrics and show it on grafana in visual graphs.

first you have to setup an apache server in minikube. If you use your apache image in apache.yml file you have to made a few changes in httpd.conf
file to get metrics.

kubectl create -f apache.yml         

### you have to add this lines in httpd.conf in /usr/local/apache2/conf ###

### <Location "/server-status">
    ### SetHandler server-status
    ### Require all granted
### </Location>

### After that You have to setup apache-exporter which will collect metrices from apache and export it to prometheus. ###

kubectl create -f apache-exporter.yml

kubectl create -f apache-metrices-svc.yml

Now you have to setup prometheus.By default prometheus uses prometheus.yml file for configuration and we can create configmap to override
or implement changes in configuration.So we will create a configmap.

kubectl create -f prometheus-configmap.yml
## we will mount this configmap in prometheus-deployment.yml file, so that we can add targets in prometheus.yml ##


Kubectl create -f prometheus-deployment.yml 

### Now setup grafana. If you want to save dashboards rather then importing dashboards again and again, you have to create Persistent Volume and 
Persistent Volume claims.After creating PV and PVC you can save dashboards in it.So, that in case your grafana pod restarts, you can have your 
dashboards saved. ###

kubectl create -f grafana-pv.yml

kubectl create -f grafana-pvc.yml

kubectl create -f grafana.yml

### This will create whole monitoring Setup. ###

Now login to grafana and add prometheus as Datasource.You have to enter the url of prometheus in datasource.After successfully adding prometheus as 
data source you can import dashboards from grafana's official website or create one by yourself.
