Defines a template that Kubernetes uses to create
{{< glossary_tooltip text="ResourceClaims" term_id="resourceclaim" >}}.
ResourceClaimTemplates are used in
dynamic resource allocation (DRA)
to provide per-Pod or per-{{< glossary_tooltip text="PodGroup" term_id="podgroup" >}} access to separate, similar resources.

When a ResourceClaimTemplate is referenced in a workload specification,
Kubernetes automatically creates ResourceClaim objects based on the template.
Each ResourceClaim is bound to a specific Pod or PodGroup. When the Pod
terminates or the PodGroup is deleted, Kubernetes deletes the corresponding
ResourceClaim. PodGroup ResourceClaimTemplates require the
DRAWorkloadResourceClaims
feature to be enabled.