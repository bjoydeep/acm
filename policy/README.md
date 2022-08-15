# Policies



Policy | Description | Additional Info
--- | --- | ---
namespace-label | Creates a namespace with a label if it does not exist. If the namespace exist, but not the labels, the labels are created as well. | Add this metric to the Configmap `observability-metrics-custom-allowlist` in namespace `open-cluster-management-observability`. Create this Configmap if it does not exist following [guidelines](https://access.redhat.com/documentation/en-us/red_hat_advanced_cluster_management_for_kubernetes/2.5/html/observability/observing-environments-intro#adding-custom-metrics).  Reference [KubeState metrics](https://github.com/kubernetes/kube-state-metrics/blob/master/docs/namespace-metrics.md).