Synopsis
Generate the self-signed CA to provision identities for etcd, and save them into etcd/ca.crt and etcd/ca.key files.
If both files already exist, kubeadm skips the generation step and existing files will be used.
kubeadm init phase certs etcd-ca [flags]
Options

--cert-dir string     Default: "/etc/kubernetes/pki"

The path where to save and store the certificates.

--config string

Path to a kubeadm configuration file.

--dry-run

Don't apply any changes; just output what would be done.

-h, --help

help for etcd-ca

--kubernetes-version string     Default: "stable-1"

Choose a specific Kubernetes version for the control plane.

Options inherited from parent commands

--rootfs string

The path to the 'real' host root filesystem. This will cause kubeadm to chroot into the provided path.