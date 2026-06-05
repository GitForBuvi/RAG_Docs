Package v1 is the v1 version of the API.
Resource Types

WebhookAdmission

WebhookAdmission     {#apiserver-config-k8s-io-v1-WebhookAdmission}
WebhookAdmission provides configuration for the webhook admission controller.

FieldDescription

apiVersionstringapiserver.config.k8s.io/v1
kindstringWebhookAdmission
kubeConfigFile [Required]
string

KubeConfigFile is the path to the kubeconfig file.

staticManifestsDir
string

StaticManifestsDir is the path to a directory containing static webhook
configurations to be loaded at startup. Files with extensions .yaml,
.yml, and .json are read. Only admissionregistration.k8s.io/v1
ValidatingWebhookConfiguration and MutatingWebhookConfiguration
resources are supported.
Using this field requires the ManifestBasedAdmissionControlConfig
feature gate to be enabled.