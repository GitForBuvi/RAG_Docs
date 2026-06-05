Install the CoreDNS addon to a Kubernetes cluster
Synopsis
Install the CoreDNS addon components via the API server. Please note that although the DNS server is deployed, it will not be scheduled until CNI is installed.
kubeadm init phase addon coredns [flags]
Options

--config string

Path to a kubeadm configuration file.

--dry-run

Don't apply any changes; just output what would be done.

--feature-gates string

A set of key=value pairs that describe feature gates for various features. Options are:NodeLocalCRISocket=true|false (default=true)PublicKeysECDSA=true|false (DEPRECATED - default=false)RootlessControlPlane=true|false (ALPHA - default=false)

-h, --help

help for coredns

--image-repository string     Default: "registry.k8s.io"

Choose a container registry to pull control plane images from

--kubeconfig string     Default: "/etc/kubernetes/admin.conf"

The kubeconfig file to use when talking to the cluster. If the flag is not set, a set of standard locations can be searched for an existing kubeconfig file.

--kubernetes-version string     Default: "stable-1"

Choose a specific Kubernetes version for the control plane.

--print-manifest

Print the addon manifests to STDOUT instead of installing them

--service-cidr string     Default: "10.96.0.0/12"

Use alternative range of IP address for service VIPs.

--service-dns-domain string     Default: "cluster.local"

Use alternative domain for services, e.g. "myorg.internal".

Options inherited from parent commands

--rootfs string

The path to the 'real' host root filesystem. This will cause kubeadm to chroot into the provided path.