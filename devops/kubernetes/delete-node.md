# How to safely remove a node from a cluster

To safely remove a node from your Kubernetes cluster and ensure that all pods running on that node are evicted and rescheduled on other nodes, follow these steps:

### 1. **Drain the Node**

Draining a node will safely evict all pods, ensuring that they are rescheduled on other nodes in the cluster.

```bash
kubectl drain <node-name> --ignore-daemonsets --delete-emptydir-data
```

- `--ignore-daemonsets`: This flag tells Kubernetes to ignore DaemonSet-managed pods when evicting pods, as DaemonSets are expected to run on specific nodes.
- `--delete-emptydir-data`: This flag tells Kubernetes to delete pods that use emptyDir volumes, as the data in these volumes is ephemeral and not shared across nodes.

  **Note**: Draining a node can take some time, depending on the number of pods running and the time it takes to reschedule them.

### 2. **Verify Pod Eviction**

After issuing the drain command, verify that all non-DaemonSet pods have been successfully evicted from the node:

```bash
kubectl get pods --all-namespaces -o wide | grep <node-name>
```

The node should only have DaemonSet pods (if any) left.

### 3. **Cordon the Node (Optional)**

Cordon the node to prevent any new pods from being scheduled on it. This is useful if you want to drain the node without immediately removing it from the cluster.

```bash
kubectl cordon <node-name>
```

This marks the node as unschedulable, but does not evict any existing pods.

### 4. **Remove the Node from the Cluster**

If you want to permanently remove the node from the cluster, you can do so after draining:

```bash
kubectl delete node <node-name>
```

This removes the node from the cluster, but it will still be running. You may need to stop or decommission the node from your infrastructure management tool (e.g., cloud provider console).

### 5. **Optional: Power Down the Node**

If you want to shut down the node completely, you can power it down or delete the virtual machine, depending on your infrastructure setup.

### 6. **Monitor the Cluster**

After the node is removed, monitor the cluster to ensure that all workloads have been rescheduled successfully:

```bash
kubectl get pods --all-namespaces -o wide
kubectl get nodes
```

This procedure should safely remove a node from your Kubernetes cluster while ensuring minimal disruption to your running applications.
