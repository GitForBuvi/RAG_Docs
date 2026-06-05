apiVersion: admissionregistration.k8s.io/v1
import "k8s.io/api/admissionregistration/v1"
ServiceReference {#ServiceReference}
ServiceReference holds a reference to Service.legacy.k8s.io

FieldDescription

name *string
name is the name of the service. Required

namespace *string
namespace is the namespace of the service. Required

pathstring
path is an optional URL path which will be sent in any request to this service.

portinteger
port is the port on the service that hosts the webhook. Default to 443 for backward compatibility. `port` should be a valid port number (1-65535, inclusive).