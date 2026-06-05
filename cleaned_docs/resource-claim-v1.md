apiVersion: resource.k8s.io/v1
import "k8s.io/api/resource/v1"
ResourceClaim {#ResourceClaim}
ResourceClaim describes a request for access to resources in the cluster, for use by workloads. For example, if a workload needs an accelerator device with specific properties, this is how that request is expressed. The status stanza tracks whether this claim has been satisfied and what specific resources have been allocated.

FieldDescription

apiVersionstring
APIVersion defines the versioned schema of this representation of an object. Servers should convert recognized schemas to the latest internal value, and may reject unrecognized values. More info: https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#resources

kindstring
Kind is a string value representing the REST resource this object represents. Servers may infer this from the endpoint the client submits requests to. Cannot be updated. In CamelCase. More info: https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#types-kinds

metadata}}">ObjectMeta
Standard object metadata

spec *}}">ResourceClaimSpec
Spec describes what is being requested and how to configure it. The spec is immutable.

status}}">ResourceClaimStatus
Status describes whether the claim is ready to use and what has been allocated.

ResourceClaimSpec {#ResourceClaimSpec}
ResourceClaimSpec defines what is being requested in a ResourceClaim and how to configure it.

FieldDescription

devices}}">DeviceClaim
Devices defines how to request devices.

ResourceClaimStatus {#ResourceClaimStatus}
ResourceClaimStatus tracks whether the resource has been allocated and what the result of that was.

FieldDescription

allocation}}">AllocationResult
Allocation is set once the claim has been allocated successfully.

devices}}">AllocatedDeviceStatus array
Devices contains the status of each device allocated for this claim, as reported by the driver. This can include driver-specific information. Entries are owned by their respective drivers.

reservedFor}}">ResourceClaimConsumerReference arraypatch strategy: merge on key uid
ReservedFor indicates which entities are currently allowed to use the claim. A Pod which references a ResourceClaim which is not reserved for that Pod will not be started. A claim that is in use or might be in use because it has been reserved must not get deallocated.  In a cluster with multiple scheduler instances, two pods might get scheduled concurrently by different schedulers. When they reference the same ResourceClaim which already has reached its maximum number of consumers, only one pod can be scheduled.  Both schedulers try to add their pod to the claim.status.reservedFor field, but only the update that reaches the API server first gets stored. The other one fails with an error and the scheduler which issued it knows that it must put the pod back into the queue, waiting for the ResourceClaim to become usable again.  There can be at most 256 such reservations. This may get increased in the future, but not reduced.

ResourceClaimList {#ResourceClaimList}
ResourceClaimList is a collection of claims.

FieldDescription

apiVersionstring
APIVersion defines the versioned schema of this representation of an object. Servers should convert recognized schemas to the latest internal value, and may reject unrecognized values. More info: https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#resources

items *}}">ResourceClaim array
Items is the list of resource claims.

kindstring
Kind is a string value representing the REST resource this object represents. Servers may infer this from the endpoint the client submits requests to. Cannot be updated. In CamelCase. More info: https://git.k8s.io/community/contributors/devel/sig-architecture/api-conventions.md#types-kinds

metadata}}">ListMeta
Standard list metadata

AllocatedDeviceStatus {#AllocatedDeviceStatus}
AllocatedDeviceStatus contains the status of an allocated device, if the driver chooses to report it. This may include driver-specific information.
The combination of Driver, Pool, Device, and ShareID must match the corresponding key in Status.Allocation.Devices.

FieldDescription

conditions}}">Condition array
Conditions contains the latest observation of the device's state. If the device has been configured according to the class and claim config references, the `Ready` condition should be True.  Must not contain more than 8 entries.

data
Data contains arbitrary driver-specific data.  The length of the raw data must be smaller or equal to 10 Ki.

device *string
Device references one device instance via its name in the driver's resource pool. It must be a DNS label.

driver *string
Driver specifies the name of the DRA driver whose kubelet plugin should be invoked to process the allocation once the claim is needed on a node.  Must be a DNS subdomain and should end with a DNS domain owned by the vendor of the driver. It should use only lower case characters.

networkData}}">NetworkDeviceData
NetworkData contains network-related information specific to the device.

pool *string
This name together with the driver name and the device name field identify which device was allocated (`\/\/\`).  Must not be longer than 253 characters and may contain one or more DNS sub-domains separated by slashes.

shareIDstring
ShareID uniquely identifies an individual allocation share of the device.

AllocationResult {#AllocationResult}
AllocationResult contains attributes of an allocated resource.

FieldDescription

allocationTimestamp}}">Time
AllocationTimestamp stores the time when the resources were allocated. This field is not guaranteed to be set, in which case that time is unknown.  This is a beta field and requires enabling the DRADeviceBindingConditions and DRAResourceClaimDeviceStatus feature gate.

devices}}">DeviceAllocationResult
Devices is the result of allocating devices.

nodeSelector}}">NodeSelector
NodeSelector defines where the allocated resources are available. If unset, they are available everywhere.

CapacityRequirements {#CapacityRequirements}
CapacityRequirements defines the capacity requirements for a specific device request.

FieldDescription

requestsobject
Requests represent individual device resource requests for distinct resources, all of which must be provided by the device.  This value is used as an additional filtering condition against the available capacity on the device. This is semantically equivalent to a CEL selector with `device.capacity[\].\.compareTo(quantity(\)) >= 0`. For example, device.capacity['test-driver.cdi.k8s.io'].counters.compareTo(quantity('2')) >= 0.  When a requestPolicy is defined, the requested amount is adjusted upward to the nearest valid value based on the policy. If the requested amount cannot be adjusted to a valid value—because it exceeds what the requestPolicy allows— the device is considered ineligible for allocation.  For any capacity that is not explicitly requested: - If no requestPolicy is set, the default consumed capacity is equal to the full device capacity   (i.e., the whole device is claimed). - If a requestPolicy is set, the default consumed capacity is determined according to that policy.  If the device allows multiple allocation, the aggregated amount across all requests must not exceed the capacity value. The consumed capacity, which may be adjusted based on the requestPolicy if defined, is recorded in the resource claim’s status.devices[*].consumedCapacity field.

DeviceAllocationConfiguration {#DeviceAllocationConfiguration}
DeviceAllocationConfiguration gets embedded in an AllocationResult.

FieldDescription

opaque}}">OpaqueDeviceConfiguration
Opaque provides driver-specific configuration parameters.

requestsstring array
Requests lists the names of requests where the configuration applies. If empty, its applies to all requests.  References to subrequests must include the name of the main request and may include the subrequest using the format \[/\]. If just the main request is given, the configuration applies to all subrequests.

source *string
Source records whether the configuration comes from a class and thus is not something that a normal user would have been able to set or from a claim.Possible enum values: - `"FromClaim"` - `"FromClass"`

DeviceAllocationResult {#DeviceAllocationResult}
DeviceAllocationResult is the result of allocating devices.

FieldDescription

config}}">DeviceAllocationConfiguration array
This field is a combination of all the claim and class configuration parameters. Drivers can distinguish between those based on a flag.  This includes configuration parameters for drivers which have no allocated devices in the result because it is up to the drivers which configuration parameters they support. They can silently ignore unknown configuration parameters.

results}}">DeviceRequestAllocationResult array
Results lists all allocated devices.

DeviceClaim {#DeviceClaim}
DeviceClaim defines how to request devices with a ResourceClaim.

FieldDescription

config}}">DeviceClaimConfiguration array
This field holds configuration for multiple potential drivers which could satisfy requests in this claim. It is ignored while allocating the claim.

constraints}}">DeviceConstraint array
These constraints must be satisfied by the set of devices that get allocated for the claim.

requests}}">DeviceRequest array
Requests represent individual requests for distinct devices which must all be satisfied. If empty, nothing needs to be allocated.

DeviceClaimConfiguration {#DeviceClaimConfiguration}
DeviceClaimConfiguration is used for configuration parameters in DeviceClaim.

FieldDescription

opaque}}">OpaqueDeviceConfiguration
Opaque provides driver-specific configuration parameters.

requestsstring array
Requests lists the names of requests where the configuration applies. If empty, it applies to all requests.  References to subrequests must include the name of the main request and may include the subrequest using the format \[/\]. If just the main request is given, the configuration applies to all subrequests.

DeviceConstraint {#DeviceConstraint}
DeviceConstraint must have exactly one field set besides Requests.

FieldDescription

distinctAttributestring
DistinctAttribute requires that all devices in question have this attribute and that its type and value are unique across those devices.  When the DRAListTypeAttributes feature gate is enabled, comparison uses set semantics (i.e., element order and duplicates are ignored): list-valued attributes must be pairwise disjoint across devices. Scalar values are treated as singleton sets for backward compatibility.  This acts as the inverse of MatchAttribute.  This constraint is used to avoid allocating multiple requests to the same device by ensuring attribute-level differentiation.  This is useful for scenarios where resource requests must be fulfilled by separate physical devices. For example, a container requests two network interfaces that must be allocated from two different physical NICs.

matchAttributestring
MatchAttribute requires that all devices in question have this attribute and that its type and value are the same across those devices.  For example, if you specified "dra.example.com/numa" (a hypothetical example!), then only devices in the same NUMA node will be chosen. A device which does not have that attribute will not be chosen. All devices should use a value of the same type for this attribute because that is part of its specification, but if one device doesn't, then it also will not be chosen.  When the DRAListTypeAttributes feature gate is enabled, comparison uses set semantics(i.e., element order and duplicates are ignored): list-valued attributes match when the intersection across all devices is non-empty. Scalar values are treated as single-element lists for backward compatibility.  Must include the domain qualifier.

requestsstring array
Requests is a list of the one or more requests in this claim which must co-satisfy this constraint. If a request is fulfilled by multiple devices, then all of the devices must satisfy the constraint. If this is not specified, this constraint applies to all requests in this claim.  References to subrequests must include the name of the main request and may include the subrequest using the format \[/\]. If just the main request is given, the constraint applies to all subrequests.

DeviceRequest {#DeviceRequest}
DeviceRequest is a request for devices required for a claim. This is typically a request for a single resource like a device, but can also ask for several identical devices. With FirstAvailable it is also possible to provide a prioritized list of requests.

FieldDescription

exactly}}">ExactDeviceRequest
Exactly specifies the details for a single request that must be met exactly for the request to be satisfied.  One of Exactly or FirstAvailable must be set.

firstAvailable}}">DeviceSubRequest array
FirstAvailable contains subrequests, of which exactly one will be selected by the scheduler. It tries to satisfy them in the order in which they are listed here. So if there are two entries in the list, the scheduler will only check the second one if it determines that the first one can not be used.  DRA does not yet implement scoring, so the scheduler will select the first set of devices that satisfies all the requests in the claim. And if the requirements can be satisfied on more than one node, other scheduling features will determine which node is chosen. This means that the set of devices allocated to a claim might not be the optimal set available to the cluster. Scoring will be implemented later.

name *string
Name can be used to reference this request in a pod.spec.containers[].resources.claims entry and in a constraint of the claim.  References using the name in the DeviceRequest will uniquely identify a request when the Exactly field is set. When the FirstAvailable field is set, a reference to the name of the DeviceRequest will match whatever subrequest is chosen by the scheduler.  Must be a DNS label.

DeviceRequestAllocationResult {#DeviceRequestAllocationResult}
DeviceRequestAllocationResult contains the allocation result for one request.

FieldDescription

adminAccessboolean
AdminAccess indicates that this device was allocated for administrative access. See the corresponding request field for a definition of mode.  Admin access is disabled if this field is unset or set to false, otherwise it is enabled.

bindingConditionsstring array
BindingConditions contains a copy of the BindingConditions from the corresponding ResourceSlice at the time of allocation.  This is a beta field and requires enabling the DRADeviceBindingConditions and DRAResourceClaimDeviceStatus feature gates.

bindingFailureConditionsstring array
BindingFailureConditions contains a copy of the BindingFailureConditions from the corresponding ResourceSlice at the time of allocation.  This is a beta field and requires enabling the DRADeviceBindingConditions and DRAResourceClaimDeviceStatus feature gates.

consumedCapacityobject
ConsumedCapacity tracks the amount of capacity consumed per device as part of the claim request. The consumed amount may differ from the requested amount: it is rounded up to the nearest valid value based on the device’s requestPolicy if applicable (i.e., may not be less than the requested amount).  The total consumed capacity for each device must not exceed the DeviceCapacity's Value.  This field is populated only for devices that allow multiple allocations. All capacity entries are included, even if the consumed amount is zero.

device *string
Device references one device instance via its name in the driver's resource pool. It must be a DNS label.

driver *string
Driver specifies the name of the DRA driver whose kubelet plugin should be invoked to process the allocation once the claim is needed on a node.  Must be a DNS subdomain and should end with a DNS domain owned by the vendor of the driver. It should use only lower case characters.

pool *string
This name together with the driver name and the device name field identify which device was allocated (`\/\/\`).  Must not be longer than 253 characters and may contain one or more DNS sub-domains separated by slashes.

request *string
Request is the name of the request in the claim which caused this device to be allocated. If it references a subrequest in the firstAvailable list on a DeviceRequest, this field must include both the name of the main request and the subrequest using the format \/\.  Multiple devices may have been allocated per request.

shareIDstring
ShareID uniquely identifies an individual allocation share of the device, used when the device supports multiple simultaneous allocations. It serves as an additional map key to differentiate concurrent shares of the same device.

tolerations}}">DeviceToleration array
A copy of all tolerations specified in the request at the time when the device got allocated.  The maximum number of tolerations is 16.  This is a beta field and requires enabling the DRADeviceTaints feature gate.

DeviceSubRequest {#DeviceSubRequest}
DeviceSubRequest describes a request for device provided in the claim.spec.devices.requests[].firstAvailable array. Each is typically a request for a single resource like a device, but can also ask for several identical devices.
DeviceSubRequest is similar to ExactDeviceRequest, but doesn't expose the AdminAccess field as that one is only supported when requesting a specific device.

FieldDescription

allocationModestring
AllocationMode and its related fields define how devices are allocated to satisfy this subrequest. Supported values are:  - ExactCount: This request is for a specific number of devices.   This is the default. The exact number is provided in the   count field.  - All: This subrequest is for all of the matching devices in a pool.   Allocation will fail if some devices are already allocated,   unless adminAccess is requested.  If AllocationMode is not specified, the default mode is ExactCount. If the mode is ExactCount and count is not specified, the default count is one. Any other subrequests must specify this field.  More modes may get added in the future. Clients must refuse to handle requests with unknown modes.Possible enum values: - `"All"` - `"ExactCount"`

capacity}}">CapacityRequirements
Capacity define resource requirements against each capacity.  If this field is unset and the device supports multiple allocations, the default value will be applied to each capacity according to requestPolicy. For the capacity that has no requestPolicy, default is the full capacity value.  Applies to each device allocation. If Count > 1, the request fails if there aren't enough devices that meet the requirements. If AllocationMode is set to All, the request fails if there are devices that otherwise match the request, and have this capacity, with a value >= the requested amount, but which cannot be allocated to this request.

countinteger
Count is used only when the count mode is "ExactCount". Must be greater than zero. If AllocationMode is ExactCount and this field is not specified, the default is one.

deviceClassName *string
DeviceClassName references a specific DeviceClass, which can define additional configuration and selectors to be inherited by this subrequest.  A class is required. Which classes are available depends on the cluster.  Administrators may use this to restrict which devices may get requested by only installing classes with selectors for permitted devices. If users are free to request anything without restrictions, then administrators can create an empty DeviceClass for users to reference.

name *string
Name can be used to reference this subrequest in the list of constraints or the list of configurations for the claim. References must use the format \/\.  Must be a DNS label.

selectors}}">DeviceSelector array
Selectors define criteria which must be satisfied by a specific device in order for that device to be considered for this subrequest. All selectors must be satisfied for a device to be considered.

tolerations}}">DeviceToleration array
If specified, the request's tolerations.  Tolerations for NoSchedule are required to allocate a device which has a taint with that effect. The same applies to NoExecute.  In addition, should any of the allocated devices get tainted with NoExecute after allocation and that effect is not tolerated, then all pods consuming the ResourceClaim get deleted to evict them. The scheduler will not let new pods reserve the claim while it has these tainted devices. Once all pods are evicted, the claim will get deallocated.  The maximum number of tolerations is 16.  This is a beta field and requires enabling the DRADeviceTaints feature gate.

DeviceToleration {#DeviceToleration}
The ResourceClaim this DeviceToleration is attached to tolerates any taint that matches the triple <key,value,effect> using the matching operator <operator>.

FieldDescription

effectstring
Effect indicates the taint effect to match. Empty means match all taint effects. When specified, allowed values are NoSchedule and NoExecute.Possible enum values: - `"NoExecute"` Evict any already-running pods that do not tolerate the device taint. - `"NoSchedule"` Do not allow new pods to schedule which use a tainted device unless they tolerate the taint, but allow all pods submitted to Kubelet without going through the scheduler to start, and allow all already-running pods to continue running. - `"None"` No effect, the taint is purely informational.

keystring
Key is the taint key that the toleration applies to. Empty means match all taint keys. If the key is empty, operator must be Exists; this combination means to match all values and all keys. Must be a label name.

operatorstring
Operator represents a key's relationship to the value. Valid operators are Exists and Equal. Defaults to Equal. Exists is equivalent to wildcard for value, so that a ResourceClaim can tolerate all taints of a particular category.Possible enum values: - `"Equal"` - `"Exists"`

tolerationSecondsinteger
TolerationSeconds represents the period of time the toleration (which must be of effect NoExecute, otherwise this field is ignored) tolerates the taint. By default, it is not set, which means tolerate the taint forever (do not evict). Zero and negative values will be treated as 0 (evict immediately) by the system. If larger than zero, the time when the pod needs to be evicted is calculated as \ + \.

valuestring
Value is the taint value the toleration matches to. If the operator is Exists, the value must be empty, otherwise just a regular string. Must be a label value.

ExactDeviceRequest {#ExactDeviceRequest}
ExactDeviceRequest is a request for one or more identical devices.

FieldDescription

adminAccessboolean
AdminAccess indicates that this is a claim for administrative access to the device(s). Claims with AdminAccess are expected to be used for monitoring or other management services for a device.  They ignore all ordinary claims to the device with respect to access modes and any resource allocations.  Admin access is disabled if this field is unset or set to false, otherwise it is enabled.

allocationModestring
AllocationMode and its related fields define how devices are allocated to satisfy this request. Supported values are:  - ExactCount: This request is for a specific number of devices.   This is the default. The exact number is provided in the   count field.  - All: This request is for all of the matching devices in a pool.   At least one device must exist on the node for the allocation to succeed.   Allocation will fail if some devices are already allocated,   unless adminAccess is requested.  If AllocationMode is not specified, the default mode is ExactCount. If the mode is ExactCount and count is not specified, the default count is one. Any other requests must specify this field.  More modes may get added in the future. Clients must refuse to handle requests with unknown modes.Possible enum values: - `"All"` - `"ExactCount"`

capacity}}">CapacityRequirements
Capacity define resource requirements against each capacity.  If this field is unset and the device supports multiple allocations, the default value will be applied to each capacity according to requestPolicy. For the capacity that has no requestPolicy, default is the full capacity value.  Applies to each device allocation. If Count > 1, the request fails if there aren't enough devices that meet the requirements. If AllocationMode is set to All, the request fails if there are devices that otherwise match the request, and have this capacity, with a value >= the requested amount, but which cannot be allocated to this request.

countinteger
Count is used only when the count mode is "ExactCount". Must be greater than zero. If AllocationMode is ExactCount and this field is not specified, the default is one.

deviceClassName *string
DeviceClassName references a specific DeviceClass, which can define additional configuration and selectors to be inherited by this request.  A DeviceClassName is required.  Administrators may use this to restrict which devices may get requested by only installing classes with selectors for permitted devices. If users are free to request anything without restrictions, then administrators can create an empty DeviceClass for users to reference.

selectors}}">DeviceSelector array
Selectors define criteria which must be satisfied by a specific device in order for that device to be considered for this request. All selectors must be satisfied for a device to be considered.

tolerations}}">DeviceToleration array
If specified, the request's tolerations.  Tolerations for NoSchedule are required to allocate a device which has a taint with that effect. The same applies to NoExecute.  In addition, should any of the allocated devices get tainted with NoExecute after allocation and that effect is not tolerated, then all pods consuming the ResourceClaim get deleted to evict them. The scheduler will not let new pods reserve the claim while it has these tainted devices. Once all pods are evicted, the claim will get deallocated.  The maximum number of tolerations is 16.  This is a beta field and requires enabling the DRADeviceTaints feature gate.

NetworkDeviceData {#NetworkDeviceData}
NetworkDeviceData provides network-related details for the allocated device. This information may be filled by drivers or other components to configure or identify the device within a network context.

FieldDescription

hardwareAddressstring
HardwareAddress represents the hardware address (e.g. MAC Address) of the device's network interface.  Must not be longer than 128 bytes.

interfaceNamestring
InterfaceName specifies the name of the network interface associated with the allocated device. This might be the name of a physical or virtual network interface being configured in the pod.  Must not be longer than 256 bytes.

ipsstring array
IPs lists the network addresses assigned to the device's network interface. This can include both IPv4 and IPv6 addresses. The IPs are in the CIDR notation, which includes both the address and the associated subnet mask. e.g.: "192.0.2.5/24" for IPv4 and "2001:db8::5/64" for IPv6.

ResourceClaimConsumerReference {#ResourceClaimConsumerReference}
ResourceClaimConsumerReference contains enough information to let you locate the consumer of a ResourceClaim. The user must be a resource in the same namespace as the ResourceClaim.

FieldDescription

apiGroupstring
APIGroup is the group for the resource being referenced. It is empty for the core API. This matches the group in the APIVersion that is used when creating the resources.

name *string
Name is the name of resource being referenced.

resource *string
Resource is the type of resource being referenced, for example "pods".

uid *string
UID identifies exactly one incarnation of the resource.

Operations {#Operations}

post Create
HTTP Request
POST /apis/resource.k8s.io/v1/namespaces/{namespace}/resourceclaims
Path Parameters

NameTypeDescription

namespace
string
object name and auth scope, such as for teams and projects

Query Parameters

NameTypeDescription

pretty
string
If 'true', then the output is pretty printed. Defaults to 'false' unless the user-agent indicates a browser or command-line HTTP tool (curl and wget).

dryRun
string
When present, indicates that modifications should not be persisted. An invalid or unrecognized dryRun directive will result in an error response and no further processing of the request. Valid values are: - All: all dry run stages will be processed

fieldManager
string
fieldManager is a name associated with the actor or entity that is making these changes. The value must be less than or 128 characters long, and only contain printable characters, as defined by https://golang.org/pkg/unicode/#IsPrint.

fieldValidation
string
fieldValidation instructs the server on how to handle objects in the request (POST/PUT/PATCH) containing unknown or duplicate fields. Valid values are: - Ignore: This will ignore any unknown fields that are silently dropped from the object, and will ignore all but the last duplicate field that the decoder encounters. This is the default behavior prior to v1.23. - Warn: This will send a warning via the standard warning response header for each unknown field that is dropped from the object, and for each duplicate field that is encountered. The request will still succeed if there are no other errors, and will only persist the last of any duplicate fields. This is the default in v1.23+ - Strict: This will fail the request with a BadRequest error if any unknown fields would be dropped from the object, or if any duplicate fields are present. The error returned from the server will contain all unknown and duplicate fields encountered.

Body Parameters

NameTypeDescription

body
}}">ResourceClaim

Response

StatusDescriptionResponse

200
OK
}}">ResourceClaim

201
Created
}}">ResourceClaim

202
Accepted
}}">ResourceClaim

patch Patch
HTTP Request
PATCH /apis/resource.k8s.io/v1/namespaces/{namespace}/resourceclaims/{name}
Path Parameters

NameTypeDescription

name
string
name of the ResourceClaim

namespace
string
object name and auth scope, such as for teams and projects

Query Parameters

NameTypeDescription

pretty
string
If 'true', then the output is pretty printed. Defaults to 'false' unless the user-agent indicates a browser or command-line HTTP tool (curl and wget).

dryRun
string
When present, indicates that modifications should not be persisted. An invalid or unrecognized dryRun directive will result in an error response and no further processing of the request. Valid values are: - All: all dry run stages will be processed

fieldManager
string
fieldManager is a name associated with the actor or entity that is making these changes. The value must be less than or 128 characters long, and only contain printable characters, as defined by https://golang.org/pkg/unicode/#IsPrint. This field is required for apply requests (application/apply-patch) but optional for non-apply patch types (JsonPatch, MergePatch, StrategicMergePatch).

fieldValidation
string
fieldValidation instructs the server on how to handle objects in the request (POST/PUT/PATCH) containing unknown or duplicate fields. Valid values are: - Ignore: This will ignore any unknown fields that are silently dropped from the object, and will ignore all but the last duplicate field that the decoder encounters. This is the default behavior prior to v1.23. - Warn: This will send a warning via the standard warning response header for each unknown field that is dropped from the object, and for each duplicate field that is encountered. The request will still succeed if there are no other errors, and will only persist the last of any duplicate fields. This is the default in v1.23+ - Strict: This will fail the request with a BadRequest error if any unknown fields would be dropped from the object, or if any duplicate fields are present. The error returned from the server will contain all unknown and duplicate fields encountered.

force
boolean
Force is going to "force" Apply requests. It means user will re-acquire conflicting fields owned by other people. Force flag must be unset for non-apply patch requests.

Body Parameters

NameTypeDescription

body
}}">Patch

Response

StatusDescriptionResponse

200
OK
}}">ResourceClaim

201
Created
}}">ResourceClaim

put Replace
HTTP Request
PUT /apis/resource.k8s.io/v1/namespaces/{namespace}/resourceclaims/{name}
Path Parameters

NameTypeDescription

name
string
name of the ResourceClaim

namespace
string
object name and auth scope, such as for teams and projects

Query Parameters

NameTypeDescription

pretty
string
If 'true', then the output is pretty printed. Defaults to 'false' unless the user-agent indicates a browser or command-line HTTP tool (curl and wget).

dryRun
string
When present, indicates that modifications should not be persisted. An invalid or unrecognized dryRun directive will result in an error response and no further processing of the request. Valid values are: - All: all dry run stages will be processed

fieldManager
string
fieldManager is a name associated with the actor or entity that is making these changes. The value must be less than or 128 characters long, and only contain printable characters, as defined by https://golang.org/pkg/unicode/#IsPrint.

fieldValidation
string
fieldValidation instructs the server on how to handle objects in the request (POST/PUT/PATCH) containing unknown or duplicate fields. Valid values are: - Ignore: This will ignore any unknown fields that are silently dropped from the object, and will ignore all but the last duplicate field that the decoder encounters. This is the default behavior prior to v1.23. - Warn: This will send a warning via the standard warning response header for each unknown field that is dropped from the object, and for each duplicate field that is encountered. The request will still succeed if there are no other errors, and will only persist the last of any duplicate fields. This is the default in v1.23+ - Strict: This will fail the request with a BadRequest error if any unknown fields would be dropped from the object, or if any duplicate fields are present. The error returned from the server will contain all unknown and duplicate fields encountered.

Body Parameters

NameTypeDescription

body
}}">ResourceClaim

Response

StatusDescriptionResponse

200
OK
}}">ResourceClaim

201
Created
}}">ResourceClaim

delete Delete
HTTP Request
DELETE /apis/resource.k8s.io/v1/namespaces/{namespace}/resourceclaims/{name}
Path Parameters

NameTypeDescription

name
string
name of the ResourceClaim

namespace
string
object name and auth scope, such as for teams and projects

Query Parameters

NameTypeDescription

pretty
string
If 'true', then the output is pretty printed. Defaults to 'false' unless the user-agent indicates a browser or command-line HTTP tool (curl and wget).

dryRun
string
When present, indicates that modifications should not be persisted. An invalid or unrecognized dryRun directive will result in an error response and no further processing of the request. Valid values are: - All: all dry run stages will be processed

gracePeriodSeconds
integer
The duration in seconds before the object should be deleted. Value must be non-negative integer. The value zero indicates delete immediately. If this value is nil, the default grace period for the specified type will be used. Defaults to a per object value if not specified. zero means delete immediately.

ignoreStoreReadErrorWithClusterBreakingPotential
boolean
if set to true, it will trigger an unsafe deletion of the resource in case the normal deletion flow fails with a corrupt object error. A resource is considered corrupt if it can not be retrieved from the underlying storage successfully because of a) its data can not be transformed e.g. decryption failure, or b) it fails to decode into an object. NOTE: unsafe deletion ignores finalizer constraints, skips precondition checks, and removes the object from the storage. WARNING: This may potentially break the cluster if the workload associated with the resource being unsafe-deleted relies on normal deletion flow. Use only if you REALLY know what you are doing. The default value is false, and the user must opt in to enable it

orphanDependents
boolean
Deprecated: please use the PropagationPolicy, this field will be deprecated in 1.7. Should the dependent objects be orphaned. If true/false, the "orphan" finalizer will be added to/removed from the object's finalizers list. Either this field or PropagationPolicy may be set, but not both.

propagationPolicy
string
Whether and how garbage collection will be performed. Either this field or OrphanDependents may be set, but not both. The default policy is decided by the existing finalizer set in the metadata.finalizers and the resource-specific default policy. Acceptable values are: 'Orphan' - orphan the dependents; 'Background' - allow the garbage collector to delete the dependents in the background; 'Foreground' - a cascading policy that deletes all dependents in the foreground.

Body Parameters

NameTypeDescription

body
}}">DeleteOptions

Response

StatusDescriptionResponse

200
OK
}}">ResourceClaim

202
Accepted
}}">ResourceClaim

delete Delete Collection
HTTP Request
DELETE /apis/resource.k8s.io/v1/namespaces/{namespace}/resourceclaims
Path Parameters

NameTypeDescription

namespace
string
object name and auth scope, such as for teams and projects

Query Parameters

NameTypeDescription

pretty
string
If 'true', then the output is pretty printed. Defaults to 'false' unless the user-agent indicates a browser or command-line HTTP tool (curl and wget).

continue
string
The continue option should be set when retrieving more results from the server. Since this value is server defined, clients may only use the continue value from a previous query result with identical query parameters (except for the value of continue) and the server may reject a continue value it does not recognize. If the specified continue value is no longer valid whether due to expiration (generally five to fifteen minutes) or a configuration change on the server, the server will respond with a 410 ResourceExpired error together with a continue token. If the client needs a consistent list, it must restart their list without the continue field. Otherwise, the client may send another list request with the token received with the 410 error, the server will respond with a list starting from the next key, but from the latest snapshot, which is inconsistent from the previous list results - objects that are created, modified, or deleted after the first list request will be included in the response, as long as their keys are after the "next key".  This field is not supported when watch is true. Clients may start a watch from the last resourceVersion value returned by the server and not miss any modifications.

dryRun
string
When present, indicates that modifications should not be persisted. An invalid or unrecognized dryRun directive will result in an error response and no further processing of the request. Valid values are: - All: all dry run stages will be processed

fieldSelector
string
A selector to restrict the list of returned objects by their fields. Defaults to everything.

gracePeriodSeconds
integer
The duration in seconds before the object should be deleted. Value must be non-negative integer. The value zero indicates delete immediately. If this value is nil, the default grace period for the specified type will be used. Defaults to a per object value if not specified. zero means delete immediately.

ignoreStoreReadErrorWithClusterBreakingPotential
boolean
if set to true, it will trigger an unsafe deletion of the resource in case the normal deletion flow fails with a corrupt object error. A resource is considered corrupt if it can not be retrieved from the underlying storage successfully because of a) its data can not be transformed e.g. decryption failure, or b) it fails to decode into an object. NOTE: unsafe deletion ignores finalizer constraints, skips precondition checks, and removes the object from the storage. WARNING: This may potentially break the cluster if the workload associated with the resource being unsafe-deleted relies on normal deletion flow. Use only if you REALLY know what you are doing. The default value is false, and the user must opt in to enable it

labelSelector
string
A selector to restrict the list of returned objects by their labels. Defaults to everything.

limit
integer
limit is a maximum number of responses to return for a list call. If more items exist, the server will set the `continue` field on the list metadata to a value that can be used with the same initial query to retrieve the next set of results. Setting a limit may return fewer than the requested amount of items (up to zero items) in the event all requested objects are filtered out and clients should only use the presence of the continue field to determine whether more results are available. Servers may choose not to support the limit argument and will return all of the available results. If limit is specified and the continue field is empty, clients may assume that no more results are available. This field is not supported if watch is true.  The server guarantees that the objects returned when using continue will be identical to issuing a single list call without a limit - that is, no objects created, modified, or deleted after the first request is issued will be included in any subsequent continued requests. This is sometimes referred to as a consistent snapshot, and ensures that a client that is using limit to receive smaller chunks of a very large result can ensure they see all possible objects. If objects are updated during a chunked list the version of the object that was present at the time the first list result was calculated is returned.

orphanDependents
boolean
Deprecated: please use the PropagationPolicy, this field will be deprecated in 1.7. Should the dependent objects be orphaned. If true/false, the "orphan" finalizer will be added to/removed from the object's finalizers list. Either this field or PropagationPolicy may be set, but not both.

propagationPolicy
string
Whether and how garbage collection will be performed. Either this field or OrphanDependents may be set, but not both. The default policy is decided by the existing finalizer set in the metadata.finalizers and the resource-specific default policy. Acceptable values are: 'Orphan' - orphan the dependents; 'Background' - allow the garbage collector to delete the dependents in the background; 'Foreground' - a cascading policy that deletes all dependents in the foreground.

resourceVersion
string
resourceVersion sets a constraint on what resource versions a request may be served from. See https://kubernetes.io/docs/reference/using-api/api-concepts/#resource-versions for details.  Defaults to unset

resourceVersionMatch
string
resourceVersionMatch determines how resourceVersion is applied to list calls. It is highly recommended that resourceVersionMatch be set for list calls where resourceVersion is set See https://kubernetes.io/docs/reference/using-api/api-concepts/#resource-versions for details.  Defaults to unset

sendInitialEvents
boolean
`sendInitialEvents=true` may be set together with `watch=true`. In that case, the watch stream will begin with synthetic events to produce the current state of objects in the collection. Once all such events have been sent, a synthetic "Bookmark" event  will be sent. The bookmark will report the ResourceVersion (RV) corresponding to the set of objects, and be marked with `"k8s.io/initial-events-end": "true"` annotation. Afterwards, the watch stream will proceed as usual, sending watch events corresponding to changes (subsequent to the RV) to objects watched.  When `sendInitialEvents` option is set, we require `resourceVersionMatch` option to also be set. The semantic of the watch request is as following: - `resourceVersionMatch` = NotOlderThan   is interpreted as "data at least as new as the provided `resourceVersion`"   and the bookmark event is send when the state is synced   to a `resourceVersion` at least as fresh as the one provided by the ListOptions.   If `resourceVersion` is unset, this is interpreted as "consistent read" and the   bookmark event is send when the state is synced at least to the moment   when request started being processed. - `resourceVersionMatch` set to any other value or unset   Invalid error is returned.  Defaults to true if `resourceVersion=""` or `resourceVersion="0"` (for backward compatibility reasons) and to false otherwise.

shardSelector
string
shardSelector restricts the list of returned objects using a CEL-based shard selector expression. The format uses the shardRange() function combined with || (logical OR) to specify one or more hash ranges:    shardRange(object.metadata.uid, '0x0', '0x8000000000000000')   shardRange(object.metadata.uid, '0x0', '0x8000000000000000') || shardRange(object.metadata.uid, '0x8000000000000000', '0x10000000000000000')  Field paths use CEL-style object-rooted syntax (e.g. "object.metadata.uid"), NOT the fieldSelector format ("metadata.uid"). Currently supported paths:   - object.metadata.uid   - object.metadata.namespace  hexStart and hexEnd are single-quoted CEL string literals with a '0x' prefix, defining the inclusive lower and exclusive upper bounds over the 64-bit FNV-1a hash space. The full range is [0x0, 0x10000000000000000), where the exclusive upper bound equals 2^64.  Examples:   2-shard split:     shard 0: shardRange(object.metadata.uid, '0x0000000000000000', '0x8000000000000000')     shard 1: shardRange(object.metadata.uid, '0x8000000000000000', '0x10000000000000000')   4-shard split:     shard 0: shardRange(object.metadata.uid, '0x0000000000000000', '0x4000000000000000')     shard 1: shardRange(object.metadata.uid, '0x4000000000000000', '0x8000000000000000')     shard 2: shardRange(object.metadata.uid, '0x8000000000000000', '0xc000000000000000')     shard 3: shardRange(object.metadata.uid, '0xc000000000000000', '0x10000000000000000')  This is an alpha field and requires enabling the ShardedListAndWatch feature gate.

timeoutSeconds
integer
Timeout for the list/watch call. This limits the duration of the call, regardless of any activity or inactivity.

Body Parameters

NameTypeDescription

body
}}">DeleteOptions

Response

StatusDescriptionResponse

200
OK
}}">Status

get Read
HTTP Request
GET /apis/resource.k8s.io/v1/namespaces/{namespace}/resourceclaims/{name}
Path Parameters

NameTypeDescription

name
string
name of the ResourceClaim

namespace
string
object name and auth scope, such as for teams and projects

Query Parameters

NameTypeDescription

pretty
string
If 'true', then the output is pretty printed. Defaults to 'false' unless the user-agent indicates a browser or command-line HTTP tool (curl and wget).

Response

StatusDescriptionResponse

200
OK
}}">ResourceClaim

get List
HTTP Request
GET /apis/resource.k8s.io/v1/namespaces/{namespace}/resourceclaims
Path Parameters

NameTypeDescription

namespace
string
object name and auth scope, such as for teams and projects

Query Parameters

NameTypeDescription

pretty
string
If 'true', then the output is pretty printed. Defaults to 'false' unless the user-agent indicates a browser or command-line HTTP tool (curl and wget).

allowWatchBookmarks
boolean
allowWatchBookmarks requests watch events with type "BOOKMARK". Servers that do not implement bookmarks may ignore this flag and bookmarks are sent at the server's discretion. Clients should not assume bookmarks are returned at any specific interval, nor may they assume the server will send any BOOKMARK event during a session. If this is not a watch, this field is ignored.

continue
string
The continue option should be set when retrieving more results from the server. Since this value is server defined, clients may only use the continue value from a previous query result with identical query parameters (except for the value of continue) and the server may reject a continue value it does not recognize. If the specified continue value is no longer valid whether due to expiration (generally five to fifteen minutes) or a configuration change on the server, the server will respond with a 410 ResourceExpired error together with a continue token. If the client needs a consistent list, it must restart their list without the continue field. Otherwise, the client may send another list request with the token received with the 410 error, the server will respond with a list starting from the next key, but from the latest snapshot, which is inconsistent from the previous list results - objects that are created, modified, or deleted after the first list request will be included in the response, as long as their keys are after the "next key".  This field is not supported when watch is true. Clients may start a watch from the last resourceVersion value returned by the server and not miss any modifications.

fieldSelector
string
A selector to restrict the list of returned objects by their fields. Defaults to everything.

labelSelector
string
A selector to restrict the list of returned objects by their labels. Defaults to everything.

limit
integer
limit is a maximum number of responses to return for a list call. If more items exist, the server will set the `continue` field on the list metadata to a value that can be used with the same initial query to retrieve the next set of results. Setting a limit may return fewer than the requested amount of items (up to zero items) in the event all requested objects are filtered out and clients should only use the presence of the continue field to determine whether more results are available. Servers may choose not to support the limit argument and will return all of the available results. If limit is specified and the continue field is empty, clients may assume that no more results are available. This field is not supported if watch is true.  The server guarantees that the objects returned when using continue will be identical to issuing a single list call without a limit - that is, no objects created, modified, or deleted after the first request is issued will be included in any subsequent continued requests. This is sometimes referred to as a consistent snapshot, and ensures that a client that is using limit to receive smaller chunks of a very large result can ensure they see all possible objects. If objects are updated during a chunked list the version of the object that was present at the time the first list result was calculated is returned.

resourceVersion
string
resourceVersion sets a constraint on what resource versions a request may be served from. See https://kubernetes.io/docs/reference/using-api/api-concepts/#resource-versions for details.  Defaults to unset

resourceVersionMatch
string
resourceVersionMatch determines how resourceVersion is applied to list calls. It is highly recommended that resourceVersionMatch be set for list calls where resourceVersion is set See https://kubernetes.io/docs/reference/using-api/api-concepts/#resource-versions for details.  Defaults to unset

sendInitialEvents
boolean
`sendInitialEvents=true` may be set together with `watch=true`. In that case, the watch stream will begin with synthetic events to produce the current state of objects in the collection. Once all such events have been sent, a synthetic "Bookmark" event  will be sent. The bookmark will report the ResourceVersion (RV) corresponding to the set of objects, and be marked with `"k8s.io/initial-events-end": "true"` annotation. Afterwards, the watch stream will proceed as usual, sending watch events corresponding to changes (subsequent to the RV) to objects watched.  When `sendInitialEvents` option is set, we require `resourceVersionMatch` option to also be set. The semantic of the watch request is as following: - `resourceVersionMatch` = NotOlderThan   is interpreted as "data at least as new as the provided `resourceVersion`"   and the bookmark event is send when the state is synced   to a `resourceVersion` at least as fresh as the one provided by the ListOptions.   If `resourceVersion` is unset, this is interpreted as "consistent read" and the   bookmark event is send when the state is synced at least to the moment   when request started being processed. - `resourceVersionMatch` set to any other value or unset   Invalid error is returned.  Defaults to true if `resourceVersion=""` or `resourceVersion="0"` (for backward compatibility reasons) and to false otherwise.

shardSelector
string
shardSelector restricts the list of returned objects using a CEL-based shard selector expression. The format uses the shardRange() function combined with || (logical OR) to specify one or more hash ranges:    shardRange(object.metadata.uid, '0x0', '0x8000000000000000')   shardRange(object.metadata.uid, '0x0', '0x8000000000000000') || shardRange(object.metadata.uid, '0x8000000000000000', '0x10000000000000000')  Field paths use CEL-style object-rooted syntax (e.g. "object.metadata.uid"), NOT the fieldSelector format ("metadata.uid"). Currently supported paths:   - object.metadata.uid   - object.metadata.namespace  hexStart and hexEnd are single-quoted CEL string literals with a '0x' prefix, defining the inclusive lower and exclusive upper bounds over the 64-bit FNV-1a hash space. The full range is [0x0, 0x10000000000000000), where the exclusive upper bound equals 2^64.  Examples:   2-shard split:     shard 0: shardRange(object.metadata.uid, '0x0000000000000000', '0x8000000000000000')     shard 1: shardRange(object.metadata.uid, '0x8000000000000000', '0x10000000000000000')   4-shard split:     shard 0: shardRange(object.metadata.uid, '0x0000000000000000', '0x4000000000000000')     shard 1: shardRange(object.metadata.uid, '0x4000000000000000', '0x8000000000000000')     shard 2: shardRange(object.metadata.uid, '0x8000000000000000', '0xc000000000000000')     shard 3: shardRange(object.metadata.uid, '0xc000000000000000', '0x10000000000000000')  This is an alpha field and requires enabling the ShardedListAndWatch feature gate.

timeoutSeconds
integer
Timeout for the list/watch call. This limits the duration of the call, regardless of any activity or inactivity.

watch
boolean
Watch for changes to the described resources and return them as a stream of add, update, and remove notifications. Specify resourceVersion.

Response

StatusDescriptionResponse

200
OK
}}">ResourceClaimList

get List All Namespaces
HTTP Request
GET /apis/resource.k8s.io/v1/resourceclaims
Query Parameters

NameTypeDescription

allowWatchBookmarks
boolean
allowWatchBookmarks requests watch events with type "BOOKMARK". Servers that do not implement bookmarks may ignore this flag and bookmarks are sent at the server's discretion. Clients should not assume bookmarks are returned at any specific interval, nor may they assume the server will send any BOOKMARK event during a session. If this is not a watch, this field is ignored.

continue
string
The continue option should be set when retrieving more results from the server. Since this value is server defined, clients may only use the continue value from a previous query result with identical query parameters (except for the value of continue) and the server may reject a continue value it does not recognize. If the specified continue value is no longer valid whether due to expiration (generally five to fifteen minutes) or a configuration change on the server, the server will respond with a 410 ResourceExpired error together with a continue token. If the client needs a consistent list, it must restart their list without the continue field. Otherwise, the client may send another list request with the token received with the 410 error, the server will respond with a list starting from the next key, but from the latest snapshot, which is inconsistent from the previous list results - objects that are created, modified, or deleted after the first list request will be included in the response, as long as their keys are after the "next key".  This field is not supported when watch is true. Clients may start a watch from the last resourceVersion value returned by the server and not miss any modifications.

fieldSelector
string
A selector to restrict the list of returned objects by their fields. Defaults to everything.

labelSelector
string
A selector to restrict the list of returned objects by their labels. Defaults to everything.

limit
integer
limit is a maximum number of responses to return for a list call. If more items exist, the server will set the `continue` field on the list metadata to a value that can be used with the same initial query to retrieve the next set of results. Setting a limit may return fewer than the requested amount of items (up to zero items) in the event all requested objects are filtered out and clients should only use the presence of the continue field to determine whether more results are available. Servers may choose not to support the limit argument and will return all of the available results. If limit is specified and the continue field is empty, clients may assume that no more results are available. This field is not supported if watch is true.  The server guarantees that the objects returned when using continue will be identical to issuing a single list call without a limit - that is, no objects created, modified, or deleted after the first request is issued will be included in any subsequent continued requests. This is sometimes referred to as a consistent snapshot, and ensures that a client that is using limit to receive smaller chunks of a very large result can ensure they see all possible objects. If objects are updated during a chunked list the version of the object that was present at the time the first list result was calculated is returned.

pretty
string
If 'true', then the output is pretty printed. Defaults to 'false' unless the user-agent indicates a browser or command-line HTTP tool (curl and wget).

resourceVersion
string
resourceVersion sets a constraint on what resource versions a request may be served from. See https://kubernetes.io/docs/reference/using-api/api-concepts/#resource-versions for details.  Defaults to unset

resourceVersionMatch
string
resourceVersionMatch determines how resourceVersion is applied to list calls. It is highly recommended that resourceVersionMatch be set for list calls where resourceVersion is set See https://kubernetes.io/docs/reference/using-api/api-concepts/#resource-versions for details.  Defaults to unset

sendInitialEvents
boolean
`sendInitialEvents=true` may be set together with `watch=true`. In that case, the watch stream will begin with synthetic events to produce the current state of objects in the collection. Once all such events have been sent, a synthetic "Bookmark" event  will be sent. The bookmark will report the ResourceVersion (RV) corresponding to the set of objects, and be marked with `"k8s.io/initial-events-end": "true"` annotation. Afterwards, the watch stream will proceed as usual, sending watch events corresponding to changes (subsequent to the RV) to objects watched.  When `sendInitialEvents` option is set, we require `resourceVersionMatch` option to also be set. The semantic of the watch request is as following: - `resourceVersionMatch` = NotOlderThan   is interpreted as "data at least as new as the provided `resourceVersion`"   and the bookmark event is send when the state is synced   to a `resourceVersion` at least as fresh as the one provided by the ListOptions.   If `resourceVersion` is unset, this is interpreted as "consistent read" and the   bookmark event is send when the state is synced at least to the moment   when request started being processed. - `resourceVersionMatch` set to any other value or unset   Invalid error is returned.  Defaults to true if `resourceVersion=""` or `resourceVersion="0"` (for backward compatibility reasons) and to false otherwise.

shardSelector
string
shardSelector restricts the list of returned objects using a CEL-based shard selector expression. The format uses the shardRange() function combined with || (logical OR) to specify one or more hash ranges:    shardRange(object.metadata.uid, '0x0', '0x8000000000000000')   shardRange(object.metadata.uid, '0x0', '0x8000000000000000') || shardRange(object.metadata.uid, '0x8000000000000000', '0x10000000000000000')  Field paths use CEL-style object-rooted syntax (e.g. "object.metadata.uid"), NOT the fieldSelector format ("metadata.uid"). Currently supported paths:   - object.metadata.uid   - object.metadata.namespace  hexStart and hexEnd are single-quoted CEL string literals with a '0x' prefix, defining the inclusive lower and exclusive upper bounds over the 64-bit FNV-1a hash space. The full range is [0x0, 0x10000000000000000), where the exclusive upper bound equals 2^64.  Examples:   2-shard split:     shard 0: shardRange(object.metadata.uid, '0x0000000000000000', '0x8000000000000000')     shard 1: shardRange(object.metadata.uid, '0x8000000000000000', '0x10000000000000000')   4-shard split:     shard 0: shardRange(object.metadata.uid, '0x0000000000000000', '0x4000000000000000')     shard 1: shardRange(object.metadata.uid, '0x4000000000000000', '0x8000000000000000')     shard 2: shardRange(object.metadata.uid, '0x8000000000000000', '0xc000000000000000')     shard 3: shardRange(object.metadata.uid, '0xc000000000000000', '0x10000000000000000')  This is an alpha field and requires enabling the ShardedListAndWatch feature gate.

timeoutSeconds
integer
Timeout for the list/watch call. This limits the duration of the call, regardless of any activity or inactivity.

watch
boolean
Watch for changes to the described resources and return them as a stream of add, update, and remove notifications. Specify resourceVersion.

Response

StatusDescriptionResponse

200
OK
}}">ResourceClaimList

get Watch
HTTP Request
GET /apis/resource.k8s.io/v1/watch/namespaces/{namespace}/resourceclaims/{name}
Path Parameters

NameTypeDescription

name
string
name of the ResourceClaim

namespace
string
object name and auth scope, such as for teams and projects

Query Parameters

NameTypeDescription

allowWatchBookmarks
boolean
allowWatchBookmarks requests watch events with type "BOOKMARK". Servers that do not implement bookmarks may ignore this flag and bookmarks are sent at the server's discretion. Clients should not assume bookmarks are returned at any specific interval, nor may they assume the server will send any BOOKMARK event during a session. If this is not a watch, this field is ignored.

continue
string
The continue option should be set when retrieving more results from the server. Since this value is server defined, clients may only use the continue value from a previous query result with identical query parameters (except for the value of continue) and the server may reject a continue value it does not recognize. If the specified continue value is no longer valid whether due to expiration (generally five to fifteen minutes) or a configuration change on the server, the server will respond with a 410 ResourceExpired error together with a continue token. If the client needs a consistent list, it must restart their list without the continue field. Otherwise, the client may send another list request with the token received with the 410 error, the server will respond with a list starting from the next key, but from the latest snapshot, which is inconsistent from the previous list results - objects that are created, modified, or deleted after the first list request will be included in the response, as long as their keys are after the "next key".  This field is not supported when watch is true. Clients may start a watch from the last resourceVersion value returned by the server and not miss any modifications.

fieldSelector
string
A selector to restrict the list of returned objects by their fields. Defaults to everything.

labelSelector
string
A selector to restrict the list of returned objects by their labels. Defaults to everything.

limit
integer
limit is a maximum number of responses to return for a list call. If more items exist, the server will set the `continue` field on the list metadata to a value that can be used with the same initial query to retrieve the next set of results. Setting a limit may return fewer than the requested amount of items (up to zero items) in the event all requested objects are filtered out and clients should only use the presence of the continue field to determine whether more results are available. Servers may choose not to support the limit argument and will return all of the available results. If limit is specified and the continue field is empty, clients may assume that no more results are available. This field is not supported if watch is true.  The server guarantees that the objects returned when using continue will be identical to issuing a single list call without a limit - that is, no objects created, modified, or deleted after the first request is issued will be included in any subsequent continued requests. This is sometimes referred to as a consistent snapshot, and ensures that a client that is using limit to receive smaller chunks of a very large result can ensure they see all possible objects. If objects are updated during a chunked list the version of the object that was present at the time the first list result was calculated is returned.

pretty
string
If 'true', then the output is pretty printed. Defaults to 'false' unless the user-agent indicates a browser or command-line HTTP tool (curl and wget).

resourceVersion
string
resourceVersion sets a constraint on what resource versions a request may be served from. See https://kubernetes.io/docs/reference/using-api/api-concepts/#resource-versions for details.  Defaults to unset

resourceVersionMatch
string
resourceVersionMatch determines how resourceVersion is applied to list calls. It is highly recommended that resourceVersionMatch be set for list calls where resourceVersion is set See https://kubernetes.io/docs/reference/using-api/api-concepts/#resource-versions for details.  Defaults to unset

sendInitialEvents
boolean
`sendInitialEvents=true` may be set together with `watch=true`. In that case, the watch stream will begin with synthetic events to produce the current state of objects in the collection. Once all such events have been sent, a synthetic "Bookmark" event  will be sent. The bookmark will report the ResourceVersion (RV) corresponding to the set of objects, and be marked with `"k8s.io/initial-events-end": "true"` annotation. Afterwards, the watch stream will proceed as usual, sending watch events corresponding to changes (subsequent to the RV) to objects watched.  When `sendInitialEvents` option is set, we require `resourceVersionMatch` option to also be set. The semantic of the watch request is as following: - `resourceVersionMatch` = NotOlderThan   is interpreted as "data at least as new as the provided `resourceVersion`"   and the bookmark event is send when the state is synced   to a `resourceVersion` at least as fresh as the one provided by the ListOptions.   If `resourceVersion` is unset, this is interpreted as "consistent read" and the   bookmark event is send when the state is synced at least to the moment   when request started being processed. - `resourceVersionMatch` set to any other value or unset   Invalid error is returned.  Defaults to true if `resourceVersion=""` or `resourceVersion="0"` (for backward compatibility reasons) and to false otherwise.

shardSelector
string
shardSelector restricts the list of returned objects using a CEL-based shard selector expression. The format uses the shardRange() function combined with || (logical OR) to specify one or more hash ranges:    shardRange(object.metadata.uid, '0x0', '0x8000000000000000')   shardRange(object.metadata.uid, '0x0', '0x8000000000000000') || shardRange(object.metadata.uid, '0x8000000000000000', '0x10000000000000000')  Field paths use CEL-style object-rooted syntax (e.g. "object.metadata.uid"), NOT the fieldSelector format ("metadata.uid"). Currently supported paths:   - object.metadata.uid   - object.metadata.namespace  hexStart and hexEnd are single-quoted CEL string literals with a '0x' prefix, defining the inclusive lower and exclusive upper bounds over the 64-bit FNV-1a hash space. The full range is [0x0, 0x10000000000000000), where the exclusive upper bound equals 2^64.  Examples:   2-shard split:     shard 0: shardRange(object.metadata.uid, '0x0000000000000000', '0x8000000000000000')     shard 1: shardRange(object.metadata.uid, '0x8000000000000000', '0x10000000000000000')   4-shard split:     shard 0: shardRange(object.metadata.uid, '0x0000000000000000', '0x4000000000000000')     shard 1: shardRange(object.metadata.uid, '0x4000000000000000', '0x8000000000000000')     shard 2: shardRange(object.metadata.uid, '0x8000000000000000', '0xc000000000000000')     shard 3: shardRange(object.metadata.uid, '0xc000000000000000', '0x10000000000000000')  This is an alpha field and requires enabling the ShardedListAndWatch feature gate.

timeoutSeconds
integer
Timeout for the list/watch call. This limits the duration of the call, regardless of any activity or inactivity.

watch
boolean
Watch for changes to the described resources and return them as a stream of add, update, and remove notifications. Specify resourceVersion.

Response

StatusDescriptionResponse

200
OK
}}">WatchEvent

get Watch List
HTTP Request
GET /apis/resource.k8s.io/v1/watch/namespaces/{namespace}/resourceclaims
Path Parameters

NameTypeDescription

namespace
string
object name and auth scope, such as for teams and projects

Query Parameters

NameTypeDescription

allowWatchBookmarks
boolean
allowWatchBookmarks requests watch events with type "BOOKMARK". Servers that do not implement bookmarks may ignore this flag and bookmarks are sent at the server's discretion. Clients should not assume bookmarks are returned at any specific interval, nor may they assume the server will send any BOOKMARK event during a session. If this is not a watch, this field is ignored.

continue
string
The continue option should be set when retrieving more results from the server. Since this value is server defined, clients may only use the continue value from a previous query result with identical query parameters (except for the value of continue) and the server may reject a continue value it does not recognize. If the specified continue value is no longer valid whether due to expiration (generally five to fifteen minutes) or a configuration change on the server, the server will respond with a 410 ResourceExpired error together with a continue token. If the client needs a consistent list, it must restart their list without the continue field. Otherwise, the client may send another list request with the token received with the 410 error, the server will respond with a list starting from the next key, but from the latest snapshot, which is inconsistent from the previous list results - objects that are created, modified, or deleted after the first list request will be included in the response, as long as their keys are after the "next key".  This field is not supported when watch is true. Clients may start a watch from the last resourceVersion value returned by the server and not miss any modifications.

fieldSelector
string
A selector to restrict the list of returned objects by their fields. Defaults to everything.

labelSelector
string
A selector to restrict the list of returned objects by their labels. Defaults to everything.

limit
integer
limit is a maximum number of responses to return for a list call. If more items exist, the server will set the `continue` field on the list metadata to a value that can be used with the same initial query to retrieve the next set of results. Setting a limit may return fewer than the requested amount of items (up to zero items) in the event all requested objects are filtered out and clients should only use the presence of the continue field to determine whether more results are available. Servers may choose not to support the limit argument and will return all of the available results. If limit is specified and the continue field is empty, clients may assume that no more results are available. This field is not supported if watch is true.  The server guarantees that the objects returned when using continue will be identical to issuing a single list call without a limit - that is, no objects created, modified, or deleted after the first request is issued will be included in any subsequent continued requests. This is sometimes referred to as a consistent snapshot, and ensures that a client that is using limit to receive smaller chunks of a very large result can ensure they see all possible objects. If objects are updated during a chunked list the version of the object that was present at the time the first list result was calculated is returned.

pretty
string
If 'true', then the output is pretty printed. Defaults to 'false' unless the user-agent indicates a browser or command-line HTTP tool (curl and wget).

resourceVersion
string
resourceVersion sets a constraint on what resource versions a request may be served from. See https://kubernetes.io/docs/reference/using-api/api-concepts/#resource-versions for details.  Defaults to unset

resourceVersionMatch
string
resourceVersionMatch determines how resourceVersion is applied to list calls. It is highly recommended that resourceVersionMatch be set for list calls where resourceVersion is set See https://kubernetes.io/docs/reference/using-api/api-concepts/#resource-versions for details.  Defaults to unset

sendInitialEvents
boolean
`sendInitialEvents=true` may be set together with `watch=true`. In that case, the watch stream will begin with synthetic events to produce the current state of objects in the collection. Once all such events have been sent, a synthetic "Bookmark" event  will be sent. The bookmark will report the ResourceVersion (RV) corresponding to the set of objects, and be marked with `"k8s.io/initial-events-end": "true"` annotation. Afterwards, the watch stream will proceed as usual, sending watch events corresponding to changes (subsequent to the RV) to objects watched.  When `sendInitialEvents` option is set, we require `resourceVersionMatch` option to also be set. The semantic of the watch request is as following: - `resourceVersionMatch` = NotOlderThan   is interpreted as "data at least as new as the provided `resourceVersion`"   and the bookmark event is send when the state is synced   to a `resourceVersion` at least as fresh as the one provided by the ListOptions.   If `resourceVersion` is unset, this is interpreted as "consistent read" and the   bookmark event is send when the state is synced at least to the moment   when request started being processed. - `resourceVersionMatch` set to any other value or unset   Invalid error is returned.  Defaults to true if `resourceVersion=""` or `resourceVersion="0"` (for backward compatibility reasons) and to false otherwise.

shardSelector
string
shardSelector restricts the list of returned objects using a CEL-based shard selector expression. The format uses the shardRange() function combined with || (logical OR) to specify one or more hash ranges:    shardRange(object.metadata.uid, '0x0', '0x8000000000000000')   shardRange(object.metadata.uid, '0x0', '0x8000000000000000') || shardRange(object.metadata.uid, '0x8000000000000000', '0x10000000000000000')  Field paths use CEL-style object-rooted syntax (e.g. "object.metadata.uid"), NOT the fieldSelector format ("metadata.uid"). Currently supported paths:   - object.metadata.uid   - object.metadata.namespace  hexStart and hexEnd are single-quoted CEL string literals with a '0x' prefix, defining the inclusive lower and exclusive upper bounds over the 64-bit FNV-1a hash space. The full range is [0x0, 0x10000000000000000), where the exclusive upper bound equals 2^64.  Examples:   2-shard split:     shard 0: shardRange(object.metadata.uid, '0x0000000000000000', '0x8000000000000000')     shard 1: shardRange(object.metadata.uid, '0x8000000000000000', '0x10000000000000000')   4-shard split:     shard 0: shardRange(object.metadata.uid, '0x0000000000000000', '0x4000000000000000')     shard 1: shardRange(object.metadata.uid, '0x4000000000000000', '0x8000000000000000')     shard 2: shardRange(object.metadata.uid, '0x8000000000000000', '0xc000000000000000')     shard 3: shardRange(object.metadata.uid, '0xc000000000000000', '0x10000000000000000')  This is an alpha field and requires enabling the ShardedListAndWatch feature gate.

timeoutSeconds
integer
Timeout for the list/watch call. This limits the duration of the call, regardless of any activity or inactivity.

watch
boolean
Watch for changes to the described resources and return them as a stream of add, update, and remove notifications. Specify resourceVersion.

Response

StatusDescriptionResponse

200
OK
}}">WatchEvent

get Watch List All Namespaces
HTTP Request
GET /apis/resource.k8s.io/v1/watch/resourceclaims
Query Parameters

NameTypeDescription

allowWatchBookmarks
boolean
allowWatchBookmarks requests watch events with type "BOOKMARK". Servers that do not implement bookmarks may ignore this flag and bookmarks are sent at the server's discretion. Clients should not assume bookmarks are returned at any specific interval, nor may they assume the server will send any BOOKMARK event during a session. If this is not a watch, this field is ignored.

continue
string
The continue option should be set when retrieving more results from the server. Since this value is server defined, clients may only use the continue value from a previous query result with identical query parameters (except for the value of continue) and the server may reject a continue value it does not recognize. If the specified continue value is no longer valid whether due to expiration (generally five to fifteen minutes) or a configuration change on the server, the server will respond with a 410 ResourceExpired error together with a continue token. If the client needs a consistent list, it must restart their list without the continue field. Otherwise, the client may send another list request with the token received with the 410 error, the server will respond with a list starting from the next key, but from the latest snapshot, which is inconsistent from the previous list results - objects that are created, modified, or deleted after the first list request will be included in the response, as long as their keys are after the "next key".  This field is not supported when watch is true. Clients may start a watch from the last resourceVersion value returned by the server and not miss any modifications.

fieldSelector
string
A selector to restrict the list of returned objects by their fields. Defaults to everything.

labelSelector
string
A selector to restrict the list of returned objects by their labels. Defaults to everything.

limit
integer
limit is a maximum number of responses to return for a list call. If more items exist, the server will set the `continue` field on the list metadata to a value that can be used with the same initial query to retrieve the next set of results. Setting a limit may return fewer than the requested amount of items (up to zero items) in the event all requested objects are filtered out and clients should only use the presence of the continue field to determine whether more results are available. Servers may choose not to support the limit argument and will return all of the available results. If limit is specified and the continue field is empty, clients may assume that no more results are available. This field is not supported if watch is true.  The server guarantees that the objects returned when using continue will be identical to issuing a single list call without a limit - that is, no objects created, modified, or deleted after the first request is issued will be included in any subsequent continued requests. This is sometimes referred to as a consistent snapshot, and ensures that a client that is using limit to receive smaller chunks of a very large result can ensure they see all possible objects. If objects are updated during a chunked list the version of the object that was present at the time the first list result was calculated is returned.

pretty
string
If 'true', then the output is pretty printed. Defaults to 'false' unless the user-agent indicates a browser or command-line HTTP tool (curl and wget).

resourceVersion
string
resourceVersion sets a constraint on what resource versions a request may be served from. See https://kubernetes.io/docs/reference/using-api/api-concepts/#resource-versions for details.  Defaults to unset

resourceVersionMatch
string
resourceVersionMatch determines how resourceVersion is applied to list calls. It is highly recommended that resourceVersionMatch be set for list calls where resourceVersion is set See https://kubernetes.io/docs/reference/using-api/api-concepts/#resource-versions for details.  Defaults to unset

sendInitialEvents
boolean
`sendInitialEvents=true` may be set together with `watch=true`. In that case, the watch stream will begin with synthetic events to produce the current state of objects in the collection. Once all such events have been sent, a synthetic "Bookmark" event  will be sent. The bookmark will report the ResourceVersion (RV) corresponding to the set of objects, and be marked with `"k8s.io/initial-events-end": "true"` annotation. Afterwards, the watch stream will proceed as usual, sending watch events corresponding to changes (subsequent to the RV) to objects watched.  When `sendInitialEvents` option is set, we require `resourceVersionMatch` option to also be set. The semantic of the watch request is as following: - `resourceVersionMatch` = NotOlderThan   is interpreted as "data at least as new as the provided `resourceVersion`"   and the bookmark event is send when the state is synced   to a `resourceVersion` at least as fresh as the one provided by the ListOptions.   If `resourceVersion` is unset, this is interpreted as "consistent read" and the   bookmark event is send when the state is synced at least to the moment   when request started being processed. - `resourceVersionMatch` set to any other value or unset   Invalid error is returned.  Defaults to true if `resourceVersion=""` or `resourceVersion="0"` (for backward compatibility reasons) and to false otherwise.

shardSelector
string
shardSelector restricts the list of returned objects using a CEL-based shard selector expression. The format uses the shardRange() function combined with || (logical OR) to specify one or more hash ranges:    shardRange(object.metadata.uid, '0x0', '0x8000000000000000')   shardRange(object.metadata.uid, '0x0', '0x8000000000000000') || shardRange(object.metadata.uid, '0x8000000000000000', '0x10000000000000000')  Field paths use CEL-style object-rooted syntax (e.g. "object.metadata.uid"), NOT the fieldSelector format ("metadata.uid"). Currently supported paths:   - object.metadata.uid   - object.metadata.namespace  hexStart and hexEnd are single-quoted CEL string literals with a '0x' prefix, defining the inclusive lower and exclusive upper bounds over the 64-bit FNV-1a hash space. The full range is [0x0, 0x10000000000000000), where the exclusive upper bound equals 2^64.  Examples:   2-shard split:     shard 0: shardRange(object.metadata.uid, '0x0000000000000000', '0x8000000000000000')     shard 1: shardRange(object.metadata.uid, '0x8000000000000000', '0x10000000000000000')   4-shard split:     shard 0: shardRange(object.metadata.uid, '0x0000000000000000', '0x4000000000000000')     shard 1: shardRange(object.metadata.uid, '0x4000000000000000', '0x8000000000000000')     shard 2: shardRange(object.metadata.uid, '0x8000000000000000', '0xc000000000000000')     shard 3: shardRange(object.metadata.uid, '0xc000000000000000', '0x10000000000000000')  This is an alpha field and requires enabling the ShardedListAndWatch feature gate.

timeoutSeconds
integer
Timeout for the list/watch call. This limits the duration of the call, regardless of any activity or inactivity.

watch
boolean
Watch for changes to the described resources and return them as a stream of add, update, and remove notifications. Specify resourceVersion.

Response

StatusDescriptionResponse

200
OK
}}">WatchEvent

patch Patch Status
HTTP Request
PATCH /apis/resource.k8s.io/v1/namespaces/{namespace}/resourceclaims/{name}/status
Path Parameters

NameTypeDescription

name
string
name of the ResourceClaim

namespace
string
object name and auth scope, such as for teams and projects

Query Parameters

NameTypeDescription

pretty
string
If 'true', then the output is pretty printed. Defaults to 'false' unless the user-agent indicates a browser or command-line HTTP tool (curl and wget).

dryRun
string
When present, indicates that modifications should not be persisted. An invalid or unrecognized dryRun directive will result in an error response and no further processing of the request. Valid values are: - All: all dry run stages will be processed

fieldManager
string
fieldManager is a name associated with the actor or entity that is making these changes. The value must be less than or 128 characters long, and only contain printable characters, as defined by https://golang.org/pkg/unicode/#IsPrint. This field is required for apply requests (application/apply-patch) but optional for non-apply patch types (JsonPatch, MergePatch, StrategicMergePatch).

fieldValidation
string
fieldValidation instructs the server on how to handle objects in the request (POST/PUT/PATCH) containing unknown or duplicate fields. Valid values are: - Ignore: This will ignore any unknown fields that are silently dropped from the object, and will ignore all but the last duplicate field that the decoder encounters. This is the default behavior prior to v1.23. - Warn: This will send a warning via the standard warning response header for each unknown field that is dropped from the object, and for each duplicate field that is encountered. The request will still succeed if there are no other errors, and will only persist the last of any duplicate fields. This is the default in v1.23+ - Strict: This will fail the request with a BadRequest error if any unknown fields would be dropped from the object, or if any duplicate fields are present. The error returned from the server will contain all unknown and duplicate fields encountered.

force
boolean
Force is going to "force" Apply requests. It means user will re-acquire conflicting fields owned by other people. Force flag must be unset for non-apply patch requests.

Body Parameters

NameTypeDescription

body
}}">Patch

Response

StatusDescriptionResponse

200
OK
}}">ResourceClaim

201
Created
}}">ResourceClaim

get Read Status
HTTP Request
GET /apis/resource.k8s.io/v1/namespaces/{namespace}/resourceclaims/{name}/status
Path Parameters

NameTypeDescription

name
string
name of the ResourceClaim

namespace
string
object name and auth scope, such as for teams and projects

Query Parameters

NameTypeDescription

pretty
string
If 'true', then the output is pretty printed. Defaults to 'false' unless the user-agent indicates a browser or command-line HTTP tool (curl and wget).

Response

StatusDescriptionResponse

200
OK
}}">ResourceClaim

put Replace Status
HTTP Request
PUT /apis/resource.k8s.io/v1/namespaces/{namespace}/resourceclaims/{name}/status
Path Parameters

NameTypeDescription

name
string
name of the ResourceClaim

namespace
string
object name and auth scope, such as for teams and projects

Query Parameters

NameTypeDescription

pretty
string
If 'true', then the output is pretty printed. Defaults to 'false' unless the user-agent indicates a browser or command-line HTTP tool (curl and wget).

dryRun
string
When present, indicates that modifications should not be persisted. An invalid or unrecognized dryRun directive will result in an error response and no further processing of the request. Valid values are: - All: all dry run stages will be processed

fieldManager
string
fieldManager is a name associated with the actor or entity that is making these changes. The value must be less than or 128 characters long, and only contain printable characters, as defined by https://golang.org/pkg/unicode/#IsPrint.

fieldValidation
string
fieldValidation instructs the server on how to handle objects in the request (POST/PUT/PATCH) containing unknown or duplicate fields. Valid values are: - Ignore: This will ignore any unknown fields that are silently dropped from the object, and will ignore all but the last duplicate field that the decoder encounters. This is the default behavior prior to v1.23. - Warn: This will send a warning via the standard warning response header for each unknown field that is dropped from the object, and for each duplicate field that is encountered. The request will still succeed if there are no other errors, and will only persist the last of any duplicate fields. This is the default in v1.23+ - Strict: This will fail the request with a BadRequest error if any unknown fields would be dropped from the object, or if any duplicate fields are present. The error returned from the server will contain all unknown and duplicate fields encountered.

Body Parameters

NameTypeDescription

body
}}">ResourceClaim

Response

StatusDescriptionResponse

200
OK
}}">ResourceClaim

201
Created
}}">ResourceClaim