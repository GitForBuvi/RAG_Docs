apiVersion: meta/v1
import "k8s.io/apimachinery/pkg/apis/meta/v1"
ShardInfo {#ShardInfo}
ShardInfo describes the shard selector that was applied to produce a list response. Its presence on a list response indicates the list is a filtered subset.

FieldDescription

selector *string
selector is the shard selector string from the request, echoed back so clients can verify which shard they received and merge responses from multiple shards.