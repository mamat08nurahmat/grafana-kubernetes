# grafana-kubernetes

create the monitoring namespace:
$ kubectl create -f namespace.yaml

create the service account and cluster role with the permission of getting cluster info for prometheus.
$ kubectl create -f prometheus-rbac.yaml

Note: If you were failed to create cluster role, create a cluster role binding to yourself first. (Email is case-sensitive)
$ kubectl create clusterrolebinding cluster-admin-binding --clusterrole=cluster-admin --user=${YOUR_EMAIL_ADDRESS}

create the config for prometheus to collect data from Kubernetes
$ kubectl create -f prometheus-config.yaml

create the deployment of prometheus
$ kubectl create -f prometheus-deploy.yaml

create the prometheus service
kubectl -f apply prometheus-svc.yaml



create the grafana deployment
$ kubectl create -f grafana.yaml

expose grafana deployment with kubectl expose
$ kubectl expose deployment grafana --type=LoadBalancer --namespace=monitoring

Get the IP via 
$ kubectl get svc -n monitoring -o wide

Now you can access Grafana via http://{IP}:3000/

create node-exporter daemon set to export node information
$ kubectl create -f node-exporter.yaml

create kube-state-metrics deployment to collect metrics about the cluster
$ kubectl create -f state-metrics-deploy.yaml

create the service account and cluster role with the permission of getting cluster info for state-metrics.
$ kubectl create -f state-metrics-rbac.yaml


Now, configure your cluster setting on Grafana.

    Enable the app at http://{IP}:3000/plugins/grafana-kubernetes-app/edit
    Add a data source of prometheus type: http://{IP}:3000/datasources/new
 https://cdn-images-1.medium.com/max/800/1*GpshLEQrXPPIvo0RrkpxUA.png
 
 
 Add a cluster at http://{IP}:3000/plugins/grafana-kubernetes-app/page/cluster-config with username/password and CA certificate.
 https://cdn-images-1.medium.com/max/800/1*KQ5M9-cQKBVlSq04l2WcJw.png
 https://cdn-images-1.medium.com/max/800/1*prKsnv2LBzpK4EKP97gjEQ.png
