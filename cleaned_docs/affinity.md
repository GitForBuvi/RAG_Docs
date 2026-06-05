In Kubernetes, affinity is a set of rules that give hints to the scheduler about where to place pods.

There are two kinds of affinity:
* node affinity
* pod-to-pod affinity
The rules are defined using the Kubernetes {{< glossary_tooltip term_id="label" text="labels">}},
and {{< glossary_tooltip term_id="selector" text="selectors">}} specified in {{< glossary_tooltip term_id="pod" text="pods" >}}, 
and they can be either required or preferred, depending on how strictly you want the scheduler to enforce them.