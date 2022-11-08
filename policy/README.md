# Policies



Policy | Description | Additional Info
--- | --- | ---
namespace-label | Creates a namespace with a label if it does not exist. If the namespace exist, but not the labels, the labels are created as well. | Add this metric to the Configmap `observability-metrics-custom-allowlist` in namespace `open-cluster-management-observability`. Create this Configmap if it does not exist following [guidelines](https://access.redhat.com/documentation/en-us/red_hat_advanced_cluster_management_for_kubernetes/2.5/html/observability/observing-environments-intro#adding-custom-metrics).  Reference [KubeState metrics](https://github.com/kubernetes/kube-state-metrics/blob/master/docs/namespace-metrics.md).
obs-metrics-allowlist | Using this policy you can create a allow list in a managed cluster to collect metrics using the metrics collector | This cannot be used in production today. To use this, a few changes has to be done which is not yet supported. And before you use it, you need to change the placement rule. It is hard coded to a fixed cluster name.
search-metrics | Creates service monitors, clusterrole and clusterrolebinding for Prometheus to pick these metrics up. | Had to change the existing service for search-api and search-indexer by adding labels to it. Namely `search: search-api ` and `search: search-indexer ` 