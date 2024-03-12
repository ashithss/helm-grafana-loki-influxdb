# pre-requirements

Docker
Kubernetes
Helm

=================================================================================================
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
helm show values oci://registry-1.docker.io/bitnamicharts/influxdb  > influxdb2/values.yaml
# To change the configuration, change the data in values.yaml file
helm install --values influxdb/values.yaml influxdb oci://registry-1.docker.io/bitnamicharts/influxdb -n log-aggregation
helm upgrade --reuse-values -f influxdb/values.yaml influxdb oci://registry-1.docker.io/bitnamicharts/influxdb
# To get the password
echo $(kubectl get secret influxdb-auth -o "jsonpath={.data['admin-password']}" --namespace default | base64 --decode)

# ADMIN
username: AdminUser
password: "Smiles@happiestminds"
# user
username: "nagaraj"
password: "Smiles@123"
# readUser:
username: "poornesh"
password: "Smiles@123"