The API server is a component of the Kubernetes
{{< glossary_tooltip text="control plane" term_id="control-plane" >}} that exposes the Kubernetes API.
The API server is the front end for the Kubernetes control plane.

The main implementation of a Kubernetes API server is kube-apiserver.
kube-apiserver is designed to scale horizontally—that is, it scales by deploying more instances.
You can run several instances of kube-apiserver and balance traffic between those instances.