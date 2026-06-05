apiVersion: admissionregistration.k8s.io/v1
import "k8s.io/api/admissionregistration/v1"
ParamKind {#ParamKind}
ParamKind is a tuple of Group Kind and Version.

FieldDescription

apiVersionstring
apiVersion is the API group version the resources belong to. In format of "group/version". Required.

kindstring
kind is the API kind the resources belong to. Required.