When LocalStorageCapacityIsolation 
is enabled for 
local ephemeral storage, 
the backing filesystem for emptyDir volumes supports project quotas,
and UserNamespacesSupport is enabled, 
project quotas are used to monitor emptyDir volume storage consumption rather than using filesystem walk, ensuring better performance and accuracy.