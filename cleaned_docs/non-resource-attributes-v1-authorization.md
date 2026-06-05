apiVersion: authorization.k8s.io/v1
import "k8s.io/api/authorization/v1"
NonResourceAttributes {#NonResourceAttributes}
NonResourceAttributes includes the authorization attributes available for non-resource requests to the Authorizer interface

FieldDescription

pathstring
path is the URL path of the request

verbstring
verb is the standard HTTP verb