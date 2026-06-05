Synopsis
Join a machine as a control plane instance
kubeadm join phase control-plane-join [flags]
Examples
# Joins a machine as a control plane instance
  kubeadm join phase control-plane-join all
Options

-h, --help

help for control-plane-join

Options inherited from parent commands

--rootfs string

The path to the 'real' host root filesystem. This will cause kubeadm to chroot into the provided path.