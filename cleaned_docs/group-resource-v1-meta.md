apiVersion: meta/v1
import "k8s.io/apimachinery/pkg/apis/meta/v1"
GroupResource {#GroupResource}
GroupResource specifies a Group and a Resource, but does not force a version.  This is useful for identifying concepts during lookup stages without having partially valid types

FieldDescription

group *string

resource *string