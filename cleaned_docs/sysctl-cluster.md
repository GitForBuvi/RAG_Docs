{{< feature-state for_k8s_version="v1.21" state="stable" >}}
This document describes how to configure and use kernel parameters within a
Kubernetes cluster using the {{< glossary_tooltip term_id="sysctl" >}}
interface.
{{< note >}}
Starting from Kubernetes version 1.23, the kubelet supports the use of either / or .
as separators for sysctl names.
Starting from Kubernetes version 1.25, setting Sysctls for a Pod supports setting sysctls with slashes.
For example, you can represent the same sysctl name as kernel.shm_rmid_forced using a
period as the separator, or as kernel/shm_rmid_forced using a slash as a separator.
For more sysctl parameter conversion method details, please refer to
the page sysctl.d(5) from
the Linux man-pages project.
{{< /note >}}
{{% heading "prerequisites" %}}
{{< note >}}
sysctl is a Linux-specific command-line tool used to configure various kernel parameters
and it is not available on non-Linux operating systems.
{{< /note >}}
{{< include "task-tutorial-prereqs.md" >}}
For some steps, you also need to be able to reconfigure the command line
options for the kubelets running on your cluster.

Listing all Sysctl Parameters
In Linux, the sysctl interface allows an administrator to modify kernel
parameters at runtime. Parameters are available via the /proc/sys/ virtual
process file system. The parameters cover various subsystems such as:

kernel (common prefix: kernel.)
networking (common prefix: net.)
virtual memory (common prefix: vm.)
MDADM (common prefix: dev.)
More subsystems are described in Kernel docs.

To get a list of all parameters, you can run
shell
sudo sysctl -a
Safe and Unsafe Sysctls
Kubernetes classes sysctls as either safe or unsafe. In addition to proper
namespacing, a safe sysctl must be properly isolated between pods on the
same node. This means that setting a safe sysctl for one pod

must not have any influence on any other pod on the node
must not allow to harm the node's health
must not allow to gain CPU or memory resources outside of the resource limits
  of a pod.

By far, most of the namespaced sysctls are not necessarily considered safe.
The following sysctls are supported in the safe set:

kernel.shm_rmid_forced;
net.ipv4.ip_local_port_range;
net.ipv4.tcp_syncookies;
net.ipv4.ping_group_range (since Kubernetes 1.18);
net.ipv4.ip_unprivileged_port_start (since Kubernetes 1.22);
net.ipv4.ip_local_reserved_ports (since Kubernetes 1.27, needs kernel 3.16+);
net.ipv4.tcp_keepalive_time (since Kubernetes 1.29, needs kernel 4.5+);
net.ipv4.tcp_fin_timeout (since Kubernetes 1.29, needs kernel 4.6+);
net.ipv4.tcp_keepalive_intvl (since Kubernetes 1.29, needs kernel 4.5+);
net.ipv4.tcp_keepalive_probes (since Kubernetes 1.29, needs kernel 4.5+).
net.ipv4.tcp_rmem (since Kubernetes 1.32, needs kernel 4.15+).
net.ipv4.tcp_wmem (since Kubernetes 1.32, needs kernel 4.15+).

{{< note >}}
There are some exceptions to the set of safe sysctls:

The net.* sysctls are not allowed with host networking enabled.
The net.ipv4.tcp_syncookies sysctl is not namespaced on Linux kernel version 4.5 or lower.
{{< /note >}}

This list will be extended in future Kubernetes versions when the kubelet
supports better isolation mechanisms.
Enabling Unsafe Sysctls
All safe sysctls are enabled by default.
All unsafe sysctls are disabled by default and must be allowed manually by the
cluster admin on a per-node basis. Pods with disabled unsafe sysctls will be
scheduled, but will fail to launch.
With the warning above in mind, the cluster admin can allow certain unsafe
sysctls for very special situations such as high-performance or real-time
application tuning. Unsafe sysctls are enabled on a node-by-node basis with a
flag of the kubelet; for example:
shell
kubelet --allowed-unsafe-sysctls \
  'kernel.msg*,net.core.somaxconn' ...
For {{< glossary_tooltip term_id="minikube" >}}, this can be done via the extra-config flag:
shell
minikube start --extra-config="kubelet.allowed-unsafe-sysctls=kernel.msg*,net.core.somaxconn"...
Only namespaced sysctls can be enabled this way.
Setting Sysctls for a Pod
A number of sysctls are namespaced in today's Linux kernels. This means that
they can be set independently for each pod on a node. Only namespaced sysctls
are configurable via the pod securityContext within Kubernetes.
The following sysctls are known to be namespaced. This list could change
in future versions of the Linux kernel.

kernel.shm*,
kernel.msg*,
kernel.sem,
fs.mqueue.*,
Those net.* that can be set in container networking namespace. However,
  there are exceptions (e.g., net.netfilter.nf_conntrack_max and
  net.netfilter.nf_conntrack_expect_max can be set in container networking
  namespace but are unnamespaced before Linux 5.12.2).

Sysctls with no namespace are called node-level sysctls. If you need to set
them, you must manually configure them on each node's operating system, or by
using a DaemonSet with privileged containers.
Use the pod securityContext to configure namespaced sysctls. The securityContext
applies to all containers in the same pod.
This example uses the pod securityContext to set a safe sysctl
kernel.shm_rmid_forced and two unsafe sysctls net.core.somaxconn and
kernel.msgmax. There is no distinction between safe and unsafe sysctls in
the specification.
{{< warning >}}
Only modify sysctl parameters after you understand their effects, to avoid
destabilizing your operating system.
{{< /warning >}}
yaml
apiVersion: v1
kind: Pod
metadata:
  name: sysctl-example
spec:
  securityContext:
    sysctls:
    - name: kernel.shm_rmid_forced
      value: "0"
    - name: net.core.somaxconn
      value: "1024"
    - name: kernel.msgmax
      value: "65536"
  ...

{{< warning >}}
Due to their nature of being unsafe, the use of unsafe sysctls
is at-your-own-risk and can lead to severe problems like wrong behavior of
containers, resource shortage or complete breakage of a node.
{{< /warning >}}
It is good practice to consider nodes with special sysctl settings as
tainted within a cluster, and only schedule pods onto them which need those
sysctl settings. It is suggested to use the Kubernetes taints and toleration
feature to implement this.
A pod with the unsafe sysctls will fail to launch on any node which has not
enabled those two unsafe sysctls explicitly. As with node-level sysctls it
is recommended to use
taints and toleration feature or
taints on nodes
to schedule those pods onto the right nodes.