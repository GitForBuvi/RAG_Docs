apiVersion: authorization.k8s.io/v1
import "k8s.io/api/authorization/v1"
SubjectAccessReview {#SubjectAccessReview}
SubjectAccessReview checks whether or not a user or group can perform an action.

FieldDescription

apiVersionstring
APIVersion defines the versioned schema of this representation of an object. Servers should convert recognized schemas to the latest internal value, and may reject unrecognized values. More info: https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#resources

kindstring
Kind is a string value representing the REST resource this object represents. Servers may infer this from the endpoint the client submits requests to. Cannot be updated. In CamelCase. More info: https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#types-kinds

metadata}}">ObjectMeta
metadata is the standard list metadata. More info: https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#metadata

spec *}}">SubjectAccessReviewSpec
spec holds information about the request being evaluated

status}}">SubjectAccessReviewStatus
status is filled in by the server and indicates whether the request is allowed or not

SubjectAccessReviewSpec {#SubjectAccessReviewSpec}
SubjectAccessReviewSpec is a description of the access request.  Exactly one of resourceAttributes and nonResourceAttributes must be set

FieldDescription

extraobject
extra corresponds to the user.Info.GetExtra() method from the authenticator.  Since that is input to the authorizer it needs a reflection here.

groupsstring array
groups is the groups you're testing for.

nonResourceAttributes}}">NonResourceAttributes
nonResourceAttributes describes information for a non-resource access request

resourceAttributes}}">ResourceAttributes
resourceAttributes describes information for a resource access request

uidstring
uid information about the requesting user.

userstring
user is the user you're testing for. If you specify "User" but not "Groups", then is it interpreted as "What if User were not a member of any groups

SubjectAccessReviewStatus {#SubjectAccessReviewStatus}
SubjectAccessReviewStatus

FieldDescription

allowed *boolean
allowed is required. True if the action would be allowed, false otherwise.

deniedboolean
denied is optional. True if the action would be denied, otherwise false. If both allowed is false and denied is false, then the authorizer has no opinion on whether to authorize the action. Denied may not be true if Allowed is true.

evaluationErrorstring
evaluationError is an indication that some error occurred during the authorization check. It is entirely possible to get an error and be able to continue determine authorization status in spite of it. For instance, RBAC can be missing a role, but enough roles are still present and bound to reason about the request.

reasonstring
reason is optional.  It indicates why a request was allowed or denied.