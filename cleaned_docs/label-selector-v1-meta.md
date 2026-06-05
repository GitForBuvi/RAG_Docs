apiVersion: meta/v1
import "k8s.io/apimachinery/pkg/apis/meta/v1"
LabelSelector {#LabelSelector}
A label selector is a label query over a set of resources. The result of matchLabels and matchExpressions are ANDed. An empty label selector matches all objects. A null label selector matches no objects.

FieldDescription

matchExpressions}}">LabelSelectorRequirement array
matchExpressions is a list of label selector requirements. The requirements are ANDed.

matchLabelsobject
matchLabels is a map of {key,value} pairs. A single {key,value} in the matchLabels map is equivalent to an element of matchExpressions, whose key field is "key", the operator is "In", and the values array contains only "value". The requirements are ANDed.