# OS Upgrades
  - Take me to [Video Tutorial](https://kodekloud.com/topic/os-upgrades/)
  
In this section, we will take a look at OS upgrades.

If the node was down for more than 5 minutes, then the pods are terminated from that node

  ![os](../../images/os.PNG)

- If a node doesn't come back online and a pod inside it was not part of a replicaset, the pod would be dead at that point.
-  The 'drain' evicts or deletes all pods except mirror pods (which cannot be deleted through the API server). If there are daemon set-managed pods, drain will not proceed without --ignore-daemonsets, and regardless it will not delete any daemon set-managed pods, because those pods would be immediately replaced by the daemon set controller, which ignores unschedulable markings. If there are any pods that are neither mirror pods nor managed by a replication controller, replica set, daemon set, stateful set, or job, then drain will not delete any pods unless you use --force. --force will also allow deletion to proceed if the managing resource of one or more pods is missing. [[ref](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#drain)]

- You can purposefully **`drain`** the node of all the workloads so that the workloads are moved to other nodes.
  ```
  $ kubectl drain node-1
  ```
- The node is also cordoned or marked as unschedulable.
- When the node is back online after a maintenance, it is still unschedulable. You then need to uncordon it.
  ```
  $ kubectl uncordon node-1
  ```
- There is also another command called cordon. Cordon simply marks a node unschedulable. Unlike drain it does not terminate or move the pods on an existing node.

  ![drain](../../images/drain.PNG)
  
  
#### K8s Reference Docs
- https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
