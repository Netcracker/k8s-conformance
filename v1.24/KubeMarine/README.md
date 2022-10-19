# How to Reproduce the Results

## Setup the Cluster

1. Download and install [KubeMarine](https://github.com/Netcracker/KubeMarine#kubemarine-cli-installation) on your environment.

2. Prepare VMs or bare-metal machines according to [Recommended Hardware Requirements](https://github.com/Netcracker/KubeMarine/blob/main/documentation/Installation.md#recommended-hardware-requirements) and selected [Deployment Scheme](https://github.com/Netcracker/KubeMarine/blob/main/documentation/Installation.md#deployment-schemes). To run `Sonobuoy` tests we recommend Full-HA configuration.

Make sure the nodes meet [Cluster Nodes Prerequisites](https://github.com/Netcracker/KubeMarine/blob/main/documentation/Installation.md#prerequisites-for-cluster-nodes).

3. Create an inventory file named `cluster.yaml` for your cluster based on given [examples](https://github.com/Netcracker/KubeMarine/tree/main/examples/cluster.yaml) and [documentation](https://github.com/Netcracker/KubeMarine/blob/main/documentation/Installation.md#configuration).

In the `cluster.yaml` enable PodSecurityStandards and add an exemption for the `sonobuoy` namespace:
```
...
rbac:
...
  admission: pss
  pss:
    exemptions:
      namespaces: ["kube-system","sonobuoy"]
...

```

Example of `cluster.yaml` which can be used:

```yaml
node_defaults:
  keyfile: "/home/username/.ssh/id_rsa"
  username: "centos"

nodes:
  - name: "k8s-lb"
    address: "10.101.0.1"
    internal_address: "192.168.0.1"
    roles: ["balancer"]
  - name: "k8s-control-plane-1"
    address: "10.101.0.2"
    internal_address: "192.168.0.2"
    roles: ["control-plane"]
  - name: "k8s-control-plane-2"
    address: "10.101.0.3"
    internal_address: "192.168.0.3"
    roles: ["control-plane"]
  - name: "k8s-control-plane-3"
    address: "10.101.0.4"
    internal_address: "192.168.0.4"
    roles: ["control-plane"]
  - name: "k8s-worker-1"
    address: "10.101.0.5"
    internal_address: "192.168.0.5"
    roles: ["worker"]
  - name: "k8s-worker-2"
    address: "10.101.0.6"
    internal_address: "192.168.0.6"
    roles: ["worker"]
  - name: "k8s-worker-3"
    address: "10.101.0.7"
    internal_address: "192.168.0.7"
    roles: ["worker"]

cluster_name: "k8s.example.com"

rbac:
  admission: pss
  pss:
    exemptions:
      namespaces: ["kube-system","sonobuoy"]
```

4. Move `cluster.yaml` to the directory, where Kubemarine installed and run the kubernetes installation:

```
kubemarine install
```


## Run Conformance Test

Upload [Sonobuoy](https://github.com/vmware-tanzu/sonobuoy) to one of the control-plane nodes and run it according to the documentation: https://github.com/cncf/k8s-conformance/blob/master/instructions.md#running


