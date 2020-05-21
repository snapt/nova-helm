# nova-helm
Nova Helm Repo

## Installation
Refer to Pre-requisites and the sample nova.yaml. 
```console
$ helm repo add nova-helm https://snapt.github.io/nova-helm
$ helm repo update
$ helm install <release_name> -f nova.yaml nova-helm/nova
```

### Pre-requisites

 * Setup a Nova Node on https://nova.snapt.net
   * Create a node on Nova ADC OR
   * View the install instructions on an existing node (https://nova.snapt.net/nodes) by selecting the dropdown and selecting "Install"
 * On the right pane you will find a nova.yaml file generated specifically for your node
 * Edit port mappings for the deployment and service as required in the nova.yaml file.

## Contents

3 Kubernetes elements are created
* A Namespace following the following convention: $release_name-nova-ns
* A Deployment with a single replica using the novaadc client container
* A Service with type LoadBalancer


## Sample nova.yaml

Make use of the below as input during the installation and pre-requisites step

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
