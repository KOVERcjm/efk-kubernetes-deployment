# EFK Kubernetes Deployment
A set of yaml for Kubernetes deployment of EFK stack (Elasticsearch, Fluentd and Kibana).

Successfully tested on Azure Kubernetes Services.

# Quickstart

**Step 0**: Please make sure you have Kubernetes installed, and you have the cluster admin permission.

**Step 1**: Deploy ECK (Elastic Cloud on Kubernetes), following the latest [official guide](https://www.elastic.co/guide/en/cloud-on-k8s/current/k8s-deploy-eck.html), or just input the following. 

``` shell 
kubectl apply -f https://download.elastic.co/downloads/eck/1.7.0/crds.yaml -f https://download.elastic.co/downloads/eck/1.7.0/operator.yaml
```

**Step 2**: Create `logging` namespace and deploy Elasticsearch and Kibana. 

``` shell
kubectl create namespace logging
kubectl apply -f elasticsearch-kibana_default.yaml    # with default storage
```

**Step 3**: Deploy Fluentd.

``` shell
kubectl apply -f fluentd.yaml
```

# Customized Usage

## Changing location for Elasticsearch data storage

In **Step 2**, Elasticsearch data is default stored by Kubernetes [default storage class](https://kubernetes.io/docs/tasks/administer-cluster/change-default-storage-class/). If you wish to store elsewhere, please follow the ECK instructions [here](https://www.elastic.co/guide/en/cloud-on-k8s/current/k8s-volume-claim-templates.html). And there is also provided an example for using Azure Storage Account, using the following command instead. 

``` shell
kubectl apply -f elasticsearch-kibana_azurestorage.yaml    # with Azure Storage Account
```

## Changing rules for Fluentd

In **Step 3**, Fluentd is set to collect node-level logs on each node. The logs contained stdout of all pods, see [explanation](https://kubernetes.io/docs/concepts/cluster-administration/logging/). You can also customize the Fluentd config following [official documentation](https://docs.fluentd.org/configuration).
