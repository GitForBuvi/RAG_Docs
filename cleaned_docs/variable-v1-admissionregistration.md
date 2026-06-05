apiVersion: admissionregistration.k8s.io/v1
import "k8s.io/api/admissionregistration/v1"
Variable {#Variable}
Variable is the definition of a variable that is used for composition. A variable is defined as a named expression.

FieldDescription

expression *string
expression is the expression that will be evaluated as the value of the variable. The CEL expression has access to the same identifiers as the CEL expressions in Validation.

name *string
name is the name of the variable. The name must be a valid CEL identifier and unique among all variables. The variable can be accessed in other expressions through `variables` For example, if name is "foo", the variable will be available as `variables.foo`