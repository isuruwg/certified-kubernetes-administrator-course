- [1. Taints and Tolerations](#1-taints-and-tolerations)
  - [1.1. Taints](#11-taints)
  - [1.2. Tolerations](#12-tolerations)
- [2. K8s Reference Docs](#2-k8s-reference-docs)


# 1. Taints and Tolerations
  - Take me to [Video Tutorial](https://kodekloud.com/topic/taints-and-tolerations-2/)
  
In this section, we will take a look at taints and tolerations.
- Pod to node relationship and how you can restrict what pods are placed on what nodes.
- Taints and Tolerations are used to set restrictions on what pods can be scheduled on a node. 
- Only pods which are tolerant to the particular taint on a node will get scheduled on that node.

  ![tandt](../../images/tandt.PNG)
  
## 1.1. Taints
- Use **`kubectl taint nodes`** command to taint a node.

  Syntax
  ```
  $ kubectl taint nodes <node-name> key=value:taint-effect
  ```
 
  Example
  ```
  $ kubectl taint nodes node1 app=blue:NoSchedule
  ```
  
- The taint effect defines what would happen to the pods if they do not tolerate the taint.
- There are 3 taint effects
  - **`NoSchedule`** : Don't schedule new intolerant pods
  - **`PreferNoSchedule`** : Try to avoid placing intolerant pods
  - **`NoExecute`** : Don't schedule new intolerent pods and evict any pod that's already scheduled
  
  ![tn](../../images/tn.PNG)
  
## 1.2. Tolerations
   - Tolerations are added to pods by adding a **`tolerations`** section in pod definition.
     ```yaml
     apiVersion: v1
     kind: Pod
     metadata:
      name: myapp-pod
     spec:
      containers:
      - name: nginx-container
        image: nginx
      tolerations:
      - key: "app" # All tolerations need to be inside double quotes
        operator: "Equal"
        value: "blue" 
        effect: "NoSchedule" 
     ```
    
  ![tp](../../images/tp.PNG)

- an empty key with operator "Exists" matches all keys, values and effects which means it'll tolerate everything
- an empty effect matches all effects with specified key

- Taints and Tolerations do not tell the pod to go to a particular node. Instead, they tell the node to only accept pods with certain tolerations.
- So a pod that's tolerant to a taint can still be scheduled in a node that doesn't have the specified taint.


- The master node comes with a taint by default so that it doesn't accept any pods (you can change this manually if you would like to but it's not recommended). To see this taint, run the below command


  ```bash
  $ kubectl describe node kubemaster |grep Taint
  ```
 
 ![tntm](../../images/tntm.PNG)


  
     
# 2. K8s Reference Docs
- https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/

