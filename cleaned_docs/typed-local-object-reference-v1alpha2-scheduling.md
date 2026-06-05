apiVersion: scheduling.k8s.io/v1alpha2
import "k8s.io/api/scheduling/v1alpha2"
TypedLocalObjectReference {#TypedLocalObjectReference}
TypedLocalObjectReference allows to reference typed object inside the same namespace.

FieldDescription

apiGroupstring
APIGroup is the group for the resource being referenced. If APIGroup is empty, the specified Kind must be in the core API group. For any other third-party types, setting APIGroup is required. It must be a DNS subdomain.

kind *string
Kind is the type of resource being referenced. It must be a path segment name.

name *string
Name is the name of resource being referenced. It must be a path segment name.