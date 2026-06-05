A Kubernetes feature that lets you request and share resources among Pods.
These resources are often attached
{{< glossary_tooltip text="devices" term_id="device" >}} like hardware
accelerators.

With DRA, device drivers and cluster admins define device classes that are
available to claim in workloads. Kubernetes allocates matching devices to
specific claims and places the corresponding Pods on nodes that can access the
allocated devices.