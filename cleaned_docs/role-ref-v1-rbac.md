apiVersion: rbac.authorization.k8s.io/v1
import "k8s.io/api/rbac/v1"
RoleRef {#RoleRef}
RoleRef contains information that points to the role being used

FieldDescription

apiGroupstring
APIGroup is the group for the resource being referenced

kind *string
Kind is the type of resource being referenced

name *string
Name is the name of resource being referenced