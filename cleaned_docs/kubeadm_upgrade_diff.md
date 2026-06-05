Synopsis
Show what differences would be applied to existing static pod manifests. See also: kubeadm upgrade apply --dry-run
kubeadm upgrade diff [version] [flags]
Options

--config string

Path to a kubeadm configuration file.

-c, --context-lines int     Default: 3

How many lines of context in the diff

-h, --help

help for diff

--kubeconfig string     Default: "/etc/kubernetes/admin.conf"

The kubeconfig file to use when talking to the cluster. If the flag is not set, a set of standard locations can be searched for an existing kubeconfig file.

Options inherited from parent commands

--rootfs string

The path to the 'real' host root filesystem. This will cause kubeadm to chroot into the provided path.