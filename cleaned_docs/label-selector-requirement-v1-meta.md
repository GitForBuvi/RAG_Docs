apiVersion: meta/v1
import "k8s.io/apimachinery/pkg/apis/meta/v1"
LabelSelectorRequirement {#LabelSelectorRequirement}
A label selector requirement is a selector that contains values, a key, and an operator that relates the key and values.

FieldDescription

key *string
key is the label key that the selector applies to.

operator *string
operator represents a key's relationship to a set of values. Valid operators are In, NotIn, Exists and DoesNotExist.

valuesstring array
values is an array of string values. If the operator is In or NotIn, the values array must be non-empty. If the operator is Exists or DoesNotExist, the values array must be empty. This array is replaced during a strategic merge patch.