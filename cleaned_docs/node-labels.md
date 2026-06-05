Kubernetes {{< glossary_tooltip text="nodes" term_id="node" >}} come pre-populated
with a standard set of {{< glossary_tooltip text="labels" term_id="label" >}}.
You can also set your own labels on nodes, either through the kubelet configuration or
using the Kubernetes API.
Preset labels
The preset labels that Kubernetes sets on nodes are:

kubernetes.io/arch
kubernetes.io/hostname
kubernetes.io/os
node.kubernetes.io/instance-type
  (if known to the kubelet – Kubernetes may not have this information to set the label)
topology.kubernetes.io/region
  (if known to the kubelet – Kubernetes may not have this information to set the label)
topology.kubernetes.io/zone
  (if known to the kubelet – Kubernetes may not have this information to set the label)

{{}}
The value of these labels is cloud provider specific and is not guaranteed to be reliable.
For example, the value of kubernetes.io/hostname may be the same as the node name in some environments
and a different value in other environments.
{{}}
{{% heading "whatsnext" %}}

See Well-Known Labels, Annotations and Taints for a list of common labels.
Learn how to add a label to a node.