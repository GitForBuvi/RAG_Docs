apiVersion: v1
import "k8s.io/api/core/v1"
NodeSelectorTerm {#NodeSelectorTerm}
A null or empty node selector term matches no objects. The requirements of them are ANDed. The TopologySelectorTerm type implements a subset of the NodeSelectorTerm.

FieldDescription

matchExpressions}}">NodeSelectorRequirement array
A list of node selector requirements by node's labels.

matchFields}}">NodeSelectorRequirement array
A list of node selector requirements by node's fields.