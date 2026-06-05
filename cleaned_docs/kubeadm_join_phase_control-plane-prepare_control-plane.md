Synopsis
Generate the manifests for the new control plane components
kubeadm join phase control-plane-prepare control-plane [flags]
Options

--apiserver-advertise-address string

If the node should host a new control plane instance, the IP address the API Server will advertise it's listening on. If not set the default network interface will be used.

--apiserver-bind-port int32     Default: 6443

If the node should host a new control plane instance, the port for the API Server to bind to.

--config string

Path to a kubeadm configuration file.

--control-plane

Create a new control plane instance on this node

--dry-run

Don't apply any changes; just output what would be done.

-h, --help

help for control-plane

--patches string

Path to a directory that contains files named "target[suffix][+patchtype].extension". For example, "kube-apiserver0+merge.yaml" or just "etcd.json". "target" can be one of "kube-apiserver", "kube-controller-manager", "kube-scheduler", "etcd", "kubeletconfiguration", "corednsdeployment". "patchtype" can be one of "strategic", "merge" or "json" and they match the patch formats supported by kubectl. The default "patchtype" is "strategic". "extension" must be either "json" or "yaml". "suffix" is an optional string that can be used to determine which patches are applied first alpha-numerically.

Options inherited from parent commands

--rootfs string

The path to the 'real' host root filesystem. This will cause kubeadm to chroot into the provided path.