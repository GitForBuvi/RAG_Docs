apiVersion: meta/v1
import "k8s.io/apimachinery/pkg/apis/meta/v1"
APIResource {#APIResource}
APIResource specifies the name of a resource and whether it is namespaced.

FieldDescription

categoriesstring array
categories is a list of the grouped resources this resource belongs to (e.g. 'all')

groupstring
group is the preferred group of the resource.  Empty implies the group of the containing resource list. For subresources, this may have a different value, for example: Scale".

kind *string
kind is the kind for the resource (e.g. 'Foo' is the kind for a resource 'foo')

name *string
name is the plural name of the resource.

namespaced *boolean
namespaced indicates if a resource is namespaced or not.

shortNamesstring array
shortNames is a list of suggested short names of the resource.

singularName *string
singularName is the singular name of the resource.  This allows clients to handle plural and singular opaquely. The singularName is more correct for reporting status on a single item and both singular and plural are allowed from the kubectl CLI interface.

storageVersionHashstring
The hash value of the storage version, the version this resource is converted to when written to the data store. Value must be treated as opaque by clients. Only equality comparison on the value is valid. This is an alpha feature and may change or be removed in the future. The field is populated by the apiserver only if the StorageVersionHash feature gate is enabled. This field will remain optional even if it graduates.

verbs *string array
verbs is a list of supported kube verbs (this includes get, list, watch, create, update, patch, delete, deletecollection, and proxy)

versionstring
version is the preferred version of the resource.  Empty implies the version of the containing resource list For subresources, this may have a different value, for example: v1 (while inside a v1beta1 version of the core resource's group)".

APIResourceList {#APIResourceList}
APIResourceList is a list of APIResource, it is used to expose the name of the resources supported in a specific group and version, and if the resource is namespaced.

FieldDescription

apiVersionstring
APIVersion defines the versioned schema of this representation of an object. Servers should convert recognized schemas to the latest internal value, and may reject unrecognized values. More info: https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#resources

groupVersion *string
groupVersion is the group and version this APIResourceList is for.

kindstring
Kind is a string value representing the REST resource this object represents. Servers may infer this from the endpoint the client submits requests to. Cannot be updated. In CamelCase. More info: https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#types-kinds

resources *}}">APIResource array
resources contains the name of the resources and if they are namespaced.