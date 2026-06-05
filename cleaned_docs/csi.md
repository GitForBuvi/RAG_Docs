The Container Storage Interface (CSI) defines a standard interface to expose storage systems to containers.

CSI allows vendors to create custom storage plugins for Kubernetes without adding them to the Kubernetes repository (out-of-tree plugins). To use a CSI driver from a storage provider, you must first deploy it to your cluster. You will then be able to create a {{< glossary_tooltip text="Storage Class" term_id="storage-class" >}} that uses that CSI driver.

CSI in the Kubernetes documentation
List of available CSI drivers