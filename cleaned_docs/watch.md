A verb that is used to track changes to an object in Kubernetes as a stream.
It is used for the efficient detection of changes.

A verb that is used to track changes to an object in Kubernetes as a stream. Watches allow
efficient detection of changes; for example, a
{{< glossary_tooltip term_id="controller" text="controller">}} that needs to know whenever a
ConfigMap has changed can use a watch rather than polling.
See Efficient Detection of Changes in API Concepts for more information.