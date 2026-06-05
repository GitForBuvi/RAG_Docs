apiVersion: authentication.k8s.io/v1
import "k8s.io/api/authentication/v1"
UserInfo {#UserInfo}
UserInfo holds the information about the user needed to implement the user.Info interface.

FieldDescription

extraobject
extra is any additional information provided by the authenticator.

groupsstring array
groups is the names of groups this user is a part of.

uidstring
uid is a unique value that identifies this user across time. If this user is deleted and another user by the same name is added, they will have different UIDs.

usernamestring
username is the name that uniquely identifies this user among all active users.