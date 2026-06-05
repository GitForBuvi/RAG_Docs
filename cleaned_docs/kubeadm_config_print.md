Print configuration
Synopsis
This command prints configurations for subcommands provided.
For details, see: https://pkg.go.dev/k8s.io/kubernetes/cmd/kubeadm/app/apis/kubeadm#section-directories
kubeadm config print [flags]
Options

-h, --help

help for print

Options inherited from parent commands

--kubeconfig string     Default: "/etc/kubernetes/admin.conf"

The kubeconfig file to use when talking to the cluster. If the flag is not set, a set of standard locations can be searched for an existing kubeconfig file.

--rootfs string

The path to the 'real' host root filesystem. This will cause kubeadm to chroot into the provided path.