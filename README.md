# pre-requirements for GRAFANA, LOKI, PROMTAIL and INFLUXDB

Docker
Kubernetes
Helm

=================================================================================================
============================================= GRAFANA ===========================================
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
helm search repo grafana
# To change the configuration, change the data in values.yaml file
helm install --values grafana/values.yaml grafana grafana/grafana --version 7.2.1 -n log-aggregation

# To see the Grafana Dashboard
kubectl port-forward  service/grafana 3000:80 -n log-aggregation
# helm show values grafana/grafana --version 7.2.1 > grafana/values.yaml       To get values.yaml file
# ======================
# To add the data source
Url: http://<service_name>.<namespace>.svc.cluster.local
username: "admin"
password: "Smiles@happiestminds"

# 1. InfluxDB
http://influxdb.log-aggregation.svc.cluster.local

Organization = <Org_ID>
Token = <Create API token in InfluxDB and use it>

# 2. Loki
http://loki-promtail-grafana-loki-gateway.log-aggregation.svc.cluster.local
HTTP headers = X-Scope-OrgID

# 3. Prometheus
http://prometheus-server.log-aggregation.svc.cluster.local

=================================================================================================
=================================== LOKI-PROMTAIL ===============================================

helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
helm show values oci://registry-1.docker.io/bitnamicharts/grafana-loki  > loki/values.yaml
# To change the configuration, change the data in values.yaml file
helm install --values loki/values.yaml loki-promtail oci://registry-1.docker.io/bitnamicharts/grafana-loki
# To update values.yaml file and upgrade helm
helm upgrade --reuse-values -f loki/values.yaml loki-promtail oci://registry-1.docker.io/bitnamicharts/grafana-loki
#
kubectl port-forward service/loki-promtail-grafana-loki-promtail 3100:3100
========================
# Promtail configuration for creating labels, adding jobs etc..
kubectl get secret
kubectl create secret generic loki-promtail-grafana-loki-promtail --from=./promtail.yaml
kubectl get pod
kubectl delete loki-promtail-grafana-loki-promtail-vd9wr


====================================================================================================
====================================== INFLUXDB ====================================================

helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
helm show values oci://registry-1.docker.io/bitnamicharts/influxdb  > influxdb2/values.yaml

# Give permission influxdb pv mount point
sudo groupadd log-aggregation
sudo useradd -G log-aggregation influxdb
sudo chown influxdb:log-aggregation
sudo chmod g+w influxdb


# To change the configuration, change the data in values.yaml file
helm install --values influxdb/values.yaml influxdb oci://registry-1.docker.io/bitnamicharts/influxdb -n log-aggregation
helm upgrade --reuse-values -f influxdb/values.yaml influxdb oci://registry-1.docker.io/bitnamicharts/influxdb -n log-aggregation
# To get the password
echo $(kubectl get secret influxdb-auth -o "jsonpath={.data['admin-password']}" --namespace log-aggregation | base64 --decode)

# ADMIN
username: admin
password: "Smiles@123"
# user
username: "ashith.ss"
password: "Smiles@123"
# readUser:
username: ""
password: ""