# pre-requirements

Docker
Kubernetes
Helm

=================================================================================================

helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
helm search repo grafana
helm show values grafana/grafana --version 7.2.1 > grafana/values.yaml
# To change the configuration, change the data in values.yaml file
helm install --values grafana/values.yaml grafana grafana/grafana --version 7.2.1


# To see the Grafana Dashboard
kubectl port-forward  service/grafana 3000:80

# ================================================================================================

# To add the data source
http::<service_name>.<namespace>.cluster.local
    username: "admin"
    password: "Smiles@happiestminds"

# 1. InfluxDB
http://influxdb2.default.svc.cluster.local

Organization = <Org_ID>
Token = <Create API token in InfluxDB and use it>

# 2. Loki
http://loki-promtail-grafana-loki-gateway.default.svc.cluster.local
HTTP headers = X-Scope-OrgID
