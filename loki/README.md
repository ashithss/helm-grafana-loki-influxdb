# pre-requirements

Docker
Kubernetes
Helm

=================================================================================================

helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
helm show values oci://registry-1.docker.io/bitnamicharts/grafana-loki  > loki/values.yaml
# To change the configuration, change the data in values.yaml file
helm install --values loki/values.yaml loki-promtail oci://registry-1.docker.io/bitnamicharts/grafana-loki
helm upgrade --reuse-values -f loki/values.yaml loki-promtail oci://registry-1.docker.io/bitnamicharts/grafana-loki
kubectl port-forward service/loki-promtail-grafana-loki-promtail 3100:3100
========================
# Promtail configuration for creating labels, adding jobs etc..
kubectl get secret
kubectl create secret generic loki-promtail-grafana-loki-promtail --from=./promtail.yaml
kubectl get pod
kubectl delete loki-promtail-grafana-loki-promtail-vd9wr
