# nova-helm
Nova Helm Repo

## Installation
Refer to Pre-requisites and the sample nova.yaml. We will be making use of the AutoJoin node functionality of Nova. This enables the native scaling power of Kubernetes using replicasets.

```console
$ helm repo add nova-helm https://snapt.github.io/nova-helm
$ helm repo update
$ helm <release_name> -f nova.yaml nova-helm/nova
```

### Pre-requisites

* Make a copy for the nova.yaml file from the sample below or download from from [https://nova.snapt.net/nodes](https://nova.snapt.net/adcs/auto-join/keys)
* If you copied the sample nova.yaml file, fetch your ADC AutoJoin Key from [AutoJoin](https://nova.snapt.net/adcs/auto-join/keys) and insert it as the 'nova_auto_conf' variable
* Edit port mappings for the deployment and service as required. Only enable the ports you intend to use for your backend systems.

> WARNING: Enabling ports you don't plan on using immediately may cause issues on the load balancer service. Ports can be enabled later, so it's recommended to only enable active ports.


## Contents

3 Kubernetes elements are created
* A Namespace following the following convention: $release_name-ns
* A Deployment with a single replica using the novaadc client container
  * From this a pod will be generated which is the Nova Node itself.
  * To scale your Nova Nodes up or down, simply increase or decrease the replicas
* A Service with type LoadBalancer


## Scaling Nova Nodes

In order to take full advantage of the AutoJoin functionality of Nova Nodes, scaling up and down is as simple as setting the replicas.
See below for an example of setting the nodes to 2 using native kubernetes scaling

```console
kubectl scale deployments/nova-dpl --replicas=2 -n nova-ns
```

## Sample nova.yaml

Make use of the below as input during the installation and pre-requisites step

```console
replicaCount: 1

image:
  repository: novaadc/nova-client-aj:1.0.1
  tag: 0.1.0
  pullPolicy: IfNotPresent

serviceAccount:
  create: false

nova_auto_conf: 'Insert your AutoJoin Key here'
nova_auto_conf_host: 'nova.snapt.net'
host: 'poll.nova-adc.com'

deployment_port_map:
  port80:
    containerPort: 80
    protocol: TCP


service_port_map:
  port80:
    name: 'port80'
    port: 80
    targetPort: 80
```