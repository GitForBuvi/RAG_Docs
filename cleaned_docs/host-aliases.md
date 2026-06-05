A HostAliases is a mapping between the IP address and hostname to be injected into a {{< glossary_tooltip text="Pod" term_id="pod" >}}'s hosts file.

HostAliases is an optional list of hostnames and IP addresses that will be injected into the Pod's hosts file if specified. This is only valid for non-hostNetwork Pods.