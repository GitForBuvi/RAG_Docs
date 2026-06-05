apiVersion: authorization.k8s.io/v1
import "k8s.io/api/authorization/v1"
NonResourceRule {#NonResourceRule}
NonResourceRule holds information that describes a rule for the non-resource

FieldDescription

nonResourceURLsstring array
nonResourceURLs is a set of partial urls that a user should have access to.  *s are allowed, but only as the full, final step in the path.  "*" means all.

verbs *string array
verbs is a list of kubernetes non-resource API verbs, like: get, post, put, delete, patch, head, options.  "*" means all.