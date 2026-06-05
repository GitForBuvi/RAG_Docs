Migrate ServiceAccount volumes to use a projected volume
consisting of a ServiceAccountTokenVolumeProjection. Cluster admins can use metric
serviceaccount_stale_tokens_total to monitor workloads that are depending on the extended
tokens. If there are no such workloads, turn off extended tokens by starting kube-apiserver with
flag --service-account-extend-token-expiration=false.
Check Bound Service Account Tokens
for more details.