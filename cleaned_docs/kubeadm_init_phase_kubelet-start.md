Write kubelet settings and (re)start the kubelet
Synopsis
Write a file with KubeletConfiguration and an environment file with node specific kubelet settings, and then (re)start kubelet.
kubeadm init phase kubelet-start [flags]
Examples
# Writes a dynamic environment file with kubelet flags from a InitConfiguration file.
  kubeadm init phase kubelet-start --config config.yaml
Options

--config string

Path to a kubeadm configuration file.

--cri-socket string

Path to the CRI socket to connect. If empty kubeadm will try to auto-detect this value; use this option only if you have more than one CRI installed or if you have non-standard CRI socket.

--dry-run

Don't apply any changes; just output what would be done.

-h, --help

help for kubelet-start

--image-repository string     Default: "registry.k8s.io"

Choose a container registry to pull control plane images from

--node-name string

Specify the node name.

--patches string

Path to a directory that contains files named "target[suffix][+patchtype].extension". For example, "kube-apiserver0+merge.yaml" or just "etcd.json". "target" can be one of "kube-apiserver", "kube-controller-manager", "kube-scheduler", "etcd", "kubeletconfiguration", "corednsdeployment". "patchtype" can be one of "strategic", "merge" or "json" and they match the patch formats supported by kubectl. The default "patchtype" is "strategic". "extension" must be either "json" or "yaml". "suffix" is an optional string that can be used to determine which patches are applied first alpha-numerically.

Options inherited from parent commands

--rootfs string

The path to the 'real' host root filesystem. This will cause kubeadm to chroot into the provided path.