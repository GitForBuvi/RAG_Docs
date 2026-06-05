API-initiated eviction is the process by which you use the Eviction API
to create an Eviction object that triggers graceful pod termination.

You can request eviction either by directly calling the Eviction API 
using a client of the kube-apiserver, like the kubectl drain command. 
When an Eviction object is created, the API server terminates the Pod. 
API-initiated evictions respect your configured PodDisruptionBudgets
and terminationGracePeriodSeconds.
API-initiated eviction is not the same as node-pressure eviction.

See API-initiated eviction for more information.