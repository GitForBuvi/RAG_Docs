apiVersion: v1
import "k8s.io/api/core/v1"
LocalObjectReference {#LocalObjectReference}
LocalObjectReference contains enough information to let you locate the referenced object inside the same namespace.

FieldDescription

namestring
Name of the referent. This field is effectively required, but due to backwards compatibility is allowed to be empty. Instances of this type with an empty value here are almost certainly wrong. More info: https://kubernetes.io/docs/concepts/overview/working-with-objects/names/#names