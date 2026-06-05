A Pod Disruption Budget allows an 
 application owner to create an object for a replicated application, that ensures 
 a certain number or percentage of {{< glossary_tooltip text="Pods" term_id="pod" >}}
 with an assigned label will not be voluntarily evicted at any point in time.

Involuntary disruptions cannot be prevented by PDBs; however they 
do count against the budget.