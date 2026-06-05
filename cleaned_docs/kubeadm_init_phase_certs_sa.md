Generate a private key for signing service account tokens along with its public key
Synopsis
Generate the private key for signing service account tokens along with its public key, and save them into sa.key and sa.pub files.
If both files already exist, kubeadm skips the generation step and existing files will be used.
kubeadm init phase certs sa [flags]
Options

--cert-dir string     Default: "/etc/kubernetes/pki"

The path where to save and store the certificates.

--config string

Path to a kubeadm configuration file.

--dry-run

Don't apply any changes; just output what would be done.

-h, --help

help for sa

--kubernetes-version string     Default: "stable-1"

Choose a specific Kubernetes version for the control plane.

Options inherited from parent commands

--rootfs string

The path to the 'real' host root filesystem. This will cause kubeadm to chroot into the provided path.