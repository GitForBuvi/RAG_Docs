Renew all available certificates
Synopsis
Renew all known certificates necessary to run the control plane. Renewals are run unconditionally, regardless of expiration date. Renewals can also be run individually for more control.
kubeadm certs renew all [flags]
Options

--cert-dir string     Default: "/etc/kubernetes/pki"

The path where to save the certificates

--config string

Path to a kubeadm configuration file.

-h, --help

help for all

--kubeconfig string     Default: "/etc/kubernetes/admin.conf"

The kubeconfig file to use when talking to the cluster. If the flag is not set, a set of standard locations can be searched for an existing kubeconfig file.

Options inherited from parent commands

--rootfs string

The path to the 'real' host root filesystem. This will cause kubeadm to chroot into the provided path.