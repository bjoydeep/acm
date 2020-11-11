# Explore using ACM Observability

Using Grafana Explorer with PromQL, Search UI and KUI, ACM allows us to explore and interrogate the data collected from across the fleet of managed clusters. This allows you to inspect the state of he fleet, test your hypothesis as well as problem diagnosis. Follow this document to do a dip your feet into this fascinating and empowering world.

## Using Search
Hit the `search icon` on the top right hand corner in ACM UI

### Look for any resources that was created in last hour
```
Hint: Click on the shortcut in the UI called "Create Last Hour"
```
```
Can you figure out how to find out the namespaces that were created in the last hour - hint add a filter: kind:namespace
```
### Search for a "resource" with a name that contains "a string" across any managed cluster
```
Hint: Type in kind:pod prometheus-
```
### search for all things related to a POD/application/CR
```
Hint: Type in kind:pod prometheus-
```
### Search for all clusters connected to the ACM Hub
```
Hint: Type in kind:cluster
```
### Search for all policies that have a violation across any cluster
```
Hint: Type in kind:policy ccompliant:NonCompliant
```
### Search for all applications deployed on any cluster
```
Hint: Type in kind:application
```
### Look for all unhealthy PODs in any cluster created in the last day
```
Hint: Click on the shortcut in the UI called "Unhealthy Pods" and then type in created:hour
```
### Save a search for easy access to certain resources.
```
Hint: You should be able to follow directions in the UI to save it- and then come back to it at a later time.
```
### Look at all things created/related to an Operator/Custom Resource
```
Hint: Type in kind:multiclusterobservability
```
```
multiclusterobservability is an instance of the MultiClusterObservability that you may have created when you enabled Observability in RHACM. You can extend this to check what was created in the last hour around this CR as well or extend to other custom resources as well.
```

### Look at logs of any POD on a managed cluster
```
Select Pods by any one of the techniques shown above, drill down to one Pod and see the Logs
```
### Restart a POD on any managed cluster
```
Select Pods by any one of the techniques shown above, select Pod and use the expand on the right hand side to delete the pod
```
### Modify a resource in a managed cluster.
```
Figure this one out yourself. It is easy
```

## Using Grafana Explore
Open the ACM UI and navigate to `Observe Environment -> Overview -> Grafana -> Explore (icon on the left hand panel)`. Remember, building dashboards for each and everything can be noisy. You can bookmark your favorite queries using [Grafana Explore](https://grafana.com/docs/grafana/latest/explore/) .

### Check the number of OpenShift clusters reporting data
```
Type into the Explorer Bar: count(count by (cluster) (cluster_version))
```

### Check the number of container restarts for a certain container in a certain namespace in a certain cluster
```
Type into the Explorer Bar: kube_pod_container_status_restarts_total{cluster="local-cluster",namespace="open-cluster-management",pod=~".*redis.*"}
```
### Check for all POD restarts by cluster, namespace, pod and container
```
Type into the Explorer Bar: 
sum by (cluster,namespace, container, pod) (kube_pod_container_status_restarts_total) >0
```

### Check number of kubernetes resources in one cluster
```
Type into the Explorer Bar: 
sum(cluster:usage:resources:sum{cluster="local-cluster"}) by (resource)
```
```
Note: cluster:usage:resources:sum is a out of the boxrecording rule created in OpenShift and it only gathers data for certain types of resources
```
### Check all alerts that are in firing or pending state
```
Type into the Explorer Bar: ALERTS
```

### Check if HA Proxy is having errors in connecting to the backend services
```
Type into the Explorer Bar: rate(haproxy_backend_connection_errors_total[5m])
```

### Check CPU and Memory Consumption of a POD
```
Type into the Explorer Bar: 
sum(node_namespace_pod_container:container_cpu_usage_seconds_total:sum_rate{namespace="$namespace", pod="$pod", container!="POD", cluster="$cluster"}) by (container)
```
```
or
```
```
sum(container_memory_working_set_bytes{cluster="$cluster", namespace="$namespace", pod="$pod", container!="POD", container!=""}) by (container)
```
```
Substitute the $pod etc . For example: sum(node_namespace_pod_container:container_cpu_usage_seconds_total:sum_rate{namespace="open-cluster-management", container!="POD", cluster="local-cluster",pod=~".*redis.*"}) by (container)
```
### Check the rate of object creation in API Server across all clusters excluding event objects
```
Type into the Explorer Bar:
sum(rate(etcd_object_counts{resource!="events"}[5m]) ) by (cluster,resource) >0
```
### Check if there is a correlation between object count in API Server and CPU, Memory of a POD or alerts
```
Can you figure this out yourself now. Hint: This may help [Grafana Explore](https://grafana.com/docs/grafana/latest/explore/) .
```


