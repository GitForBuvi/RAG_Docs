Configure the kube-controller-manager to publish a
{{< glossary_tooltip text="ConfigMap" term_id="configmap" >}} named kube-root-ca.crt
to every namespace. This ConfigMap contains a CA bundle used for verifying connections
to the kube-apiserver. See
Bound Service Account Tokens
for more details.