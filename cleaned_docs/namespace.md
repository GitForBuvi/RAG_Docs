An abstraction used by Kubernetes to support isolation of groups of {{< glossary_tooltip text="API resources" term_id="api-resource" >}}
within a single {{< glossary_tooltip text="cluster" term_id="cluster" >}}.

Namespaces are used to organize objects in a cluster and provide a way to divide cluster resources. Names of resources need to be unique within a namespace, but not across namespaces. Namespace-based scoping is applicable only for namespaced resources (for example: Pods, Deployments, Services) and not for cluster-wide resources (for example: StorageClasses, Nodes, PersistentVolumes).