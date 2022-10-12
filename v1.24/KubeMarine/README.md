# How to Reproduce the Results

## Setup the Cluster

Download and install [KubeMarine](https://github.com/Netcracker/KubeMarine#kubemarine-cli-installation) on your environment.

Create an inventory file named `cluster.yaml` for your cluster based on given examples: https://github.com/Netcracker/KubeMarine/tree/main/examples/cluster.yaml

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

Deploy the cluster according to the KubeMarine documentation: https://github.com/Netcracker/KubeMarine#running-cluster-installation.

## Run Conformance Test

Upload [Sonobuoy](https://github.com/vmware-tanzu/sonobuoy) to one of the control-plane nodes and run it according to the documentation: https://github.com/cncf/k8s-conformance/blob/master/instructions.md#running


