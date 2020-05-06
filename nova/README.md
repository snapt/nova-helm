# nova-helm
Nova Helm Repo

## Installation

```console
$ helm -f nova.yaml ./nova
```

### Pre-requisites
Make a copy for the values.yaml file.

Create a node on https://nova.snapt.net
Obtain the ID and Key from the installation page for your node
Navigate to https://nova.snapt.net/nodes
On the Action button for your node, select "Install"
Enter the ID and Key into the node_id and node_key in the copy of the values.yaml file
