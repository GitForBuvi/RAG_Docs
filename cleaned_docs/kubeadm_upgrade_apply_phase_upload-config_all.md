Synopsis
Upload all the configurations to ConfigMaps
kubeadm upgrade apply phase upload-config all [flags]
Options

--config string

Path to a kubeadm configuration file.

--dry-run

Do not change any state, just output what actions would be performed.

-h, --help

help for all

--kubeconfig string     Default: "/etc/kubernetes/admin.conf"

The kubeconfig file to use when talking to the cluster. If the flag is not set, a set of standard locations can be searched for an existing kubeconfig file.

Options inherited from parent commands

--rootfs string

The path to the 'real' host root filesystem. This will cause kubeadm to chroot into the provided path.