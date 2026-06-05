apiVersion: meta/v1
import "k8s.io/apimachinery/pkg/apis/meta/v1"
GroupVersionForDiscovery {#GroupVersionForDiscovery}
GroupVersion contains the "group/version" and "version" string of a version. It is made a struct to keep extensibility.

FieldDescription

groupVersion *string
groupVersion specifies the API group and version in the form "group/version"

version *string
version specifies the version in the form of "version". This is to save the clients the trouble of splitting the GroupVersion.