# Dirtypod

A kubernetes deployment which will add a public key RSA to every node in a cluster. This is a example of what can be done when PSPs are not employed on a cluster, or no implemented well enough.

# What Is Happening?

The dirtypod is a daemonset which deployed onto every node (see the tolerations of the deployment). It mounts the directory of the node which it runs on. This gives it access to the hosts filesystem, and enables it to do all kinds of nasty stuff. In this case, it will add public key to the ~/.ssh/authorized_hosts directory of the host node.

# Instructions

Replace the "INSERT PUBLIC KEY HERE" in the configmap of dirtypod.yaml with your public key and deploy the dirtypod-rbac.yaml config, then the dirtypod.yaml. The pod will add your public key to the nodes.

# Mitigation 

There are several methods which can be employed to stop this exploit from running:
  1. Ensure that your cluster is accessable only by a bastion. Dirtypod cannot put a public key on this, so it won't be able to access the cluster nodes, despite having SSH access.
  2. Tighten PSPs. The volume: "\*" part of the PSP is the core reason why this pod works. If a pod can mount its hosts volume, it can do this.
  3. Ensure RBAC permissions are restrictive. 

