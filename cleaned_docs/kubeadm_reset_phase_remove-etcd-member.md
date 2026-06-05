Synopsis
Remove a local etcd member for a control plane node.
kubeadm reset phase remove-etcd-member [flags]
Options

--dry-run

Don't apply any changes; just output what would be done.

-h, --help

help for remove-etcd-member

--kubeconfig string     Default: "/etc/kubernetes/admin.conf"

The kubeconfig file to use when talking to the cluster. If the flag is not set, a set of standard locations can be searched for an existing kubeconfig file.

Options inherited from parent commands

--rootfs string

The path to the 'real' host root filesystem. This will cause kubeadm to chroot into the provided path.