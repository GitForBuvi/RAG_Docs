apiVersion: authorization.k8s.io/v1
import "k8s.io/api/authorization/v1"
ResourceAttributes {#ResourceAttributes}
ResourceAttributes includes the authorization attributes available for resource requests to the Authorizer interface

FieldDescription

fieldSelector}}">FieldSelectorAttributes
fieldSelector describes the limitation on access based on field.  It can only limit access, not broaden it.

groupstring
group is the API Group of the Resource.  "*" means all.

labelSelector}}">LabelSelectorAttributes
labelSelector describes the limitation on access based on labels.  It can only limit access, not broaden it.

namestring
name is the name of the resource being requested for a "get" or deleted for a "delete". "" (empty) means all.

namespacestring
namespace is the namespace of the action being requested.  Currently, there is no distinction between no namespace and all namespaces "" (empty) is defaulted for LocalSubjectAccessReviews "" (empty) is empty for cluster-scoped resources "" (empty) means "all" for namespace scoped resources from a SubjectAccessReview or SelfSubjectAccessReview

resourcestring
resource is one of the existing resource types.  "*" means all.

subresourcestring
subresource is one of the existing resource types.  "" means none.

verbstring
verb is a kubernetes resource API verb, like: get, list, watch, create, update, delete, proxy.  "*" means all.

versionstring
version is the API Version of the Resource.  "*" means all.