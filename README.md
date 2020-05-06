# nova-helm
Nova Helm Repo

## Installation
Refer to Pre-requisites and the sample nova.yaml. 
```console
$ helm repo add nova-helm https://snapt.github.io/nova-helm
$ helm repo update
$ helm -f <release_name> nova.yaml nova-helm/nova
```

### Pre-requisites

* Make a copy for the values.yaml file.
* Create a node on https://nova.snapt.net
* Obtain the ID and Key from the installation page for your node
* Navigate to https://nova.snapt.net/nodes
* On the Action button for your node, select "Install"
* Enter the ID and Key into the node_id and node_key in the copy of the values.yaml file
* Edit port mappings for the deployment and service as required.

## Contents
3 Kubernetes elements are created
* A Namespace following the following convention: $release_name-nova-ns
* A Deployment with a single replica using the novaadc client container
* A Service with type LoadBalancer


## Sample nova.yaml
Make use of the below as input during the installation step
```console
replicaCount: 1

image:
  repository: novaadc/nova-client:latest
  tag: 0.1.0
  pullPolicy: IfNotPresent

serviceAccount:
  # Specifies whether a service account should be created
  create: false
  # The name of the service account to use.
  # If not set and create is true, a name is generated using the fullname template
  
node_id: <Refer to Pre-requisites>
node_key: <Refer to Pre-requisites>
host: 'poll.nova-adc.com'

deployment_port_map: # Port mappings for the kubernetes deployment
  #port 1080 is used for Nova traffic
  port80:
    containerPort: 80
    protocol: TCP
  port443:
    containerPort: 443
    protocol: TCP

service_port_map: # Port mappings for the kubernetes service
  #port 1080 is used for Nova traffic
  port80:  # Map object
    name: 'port80' # Name of the service port
    port: 80 # Maps to the port value of the service
    targetPort: 80 # Maps to the targetPort value of the service
  port443:
    name: 'port443'
    port: 443
    targetPort: 443
```