Synopsis
Enable kubelet client certificate rotation
kubeadm init phase kubelet-finalize enable-client-cert-rotation [flags]
Options

--cert-dir string     Default: "/etc/kubernetes/pki"

The path where to save and store the certificates.

--config string

Path to a kubeadm configuration file.

--dry-run

Don't apply any changes; just output what would be done.

-h, --help

help for enable-client-cert-rotation

Options inherited from parent commands

--rootfs string

The path to the 'real' host root filesystem. This will cause kubeadm to chroot into the provided path.