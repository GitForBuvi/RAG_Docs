Disruptions are events that lead to one or more
{{< glossary_tooltip term_id="pod" text="Pods" >}} going out of service.
A disruption has consequences for workload management {{< glossary_tooltip text="resources" term_id="api-resource" >}},
such as {{< glossary_tooltip term_id="deployment" >}}, that rely on the affected
Pods.

If you, as cluster operator, destroy a Pod that belongs to an application,
Kubernetes terms that a voluntary disruption. If a Pod goes offline
because of a Node failure, or an outage affecting a wider failure zone,
Kubernetes terms that an involuntary disruption.
See Disruptions for more information.