apiVersion: v1
import "k8s.io/api/core/v1"
TypedLocalObjectReference {#TypedLocalObjectReference}
TypedLocalObjectReference contains enough information to let you locate the typed referenced object inside the same namespace.

FieldDescription

apiGroupstring
APIGroup is the group for the resource being referenced. If APIGroup is not specified, the specified Kind must be in the core API group. For any other third-party types, APIGroup is required.

kind *string
Kind is the type of resource being referenced

name *string
Name is the name of resource being referenced