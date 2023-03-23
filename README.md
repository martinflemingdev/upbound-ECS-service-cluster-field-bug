# upbound-ECS-service-cluster-field-bug

spec.forProvider.cluster field does not work as expected, documentation says it takes an ARN, but only the cluster name works.

Therefore clusterSelector and clusterRef fields that resolve to ARN are also broken and do not work.

Workaround at this time is to hardcode the cluster name into the spec.forProvider.cluster field

ECS Service component manifest with comments:

apiVersion: ecs.aws.upbound.io/v1beta1
kind: Service
metadata:
  labels:
    testing.upbound.io/example-name: sample-app-service
  name: sample-app-service
spec:
  forProvider:
    # according to Upbound docs, cluster should be the ARN, but throws "InvalidParameterException: Unsupported resource type: cluster:"
    # works when given the name of the cluster, not ARN
    cluster: sample-app-cluster 
    #
    # Selector and Ref also don't work because they resolve to ARN
    #
    # clusterSelector:
    #   matchLabels:
    #     testing.upbound.io/example-name: sample-app-cluster
    #
    # clusterRef:
    #   name: sample-app-cluster
    #
