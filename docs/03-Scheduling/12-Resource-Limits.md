# 1. Resource Requirements and Limits
  - Take me to [Video Tutorials](https://kodekloud.com/topic/resource-limits/)
  
In this section we will take a look at Resource Limits

- [1. Resource Requirements and Limits](#1-resource-requirements-and-limits)
- [2. Example 3 node kubernetes cluster.](#2-example-3-node-kubernetes-cluster)
- [3. Resource Requirements](#3-resource-requirements)
- [4. Resources - Limits](#4-resources---limits)
  - [4.1. Exceed Limits](#41-exceed-limits)
- [5. Default requests and limits](#5-default-requests-and-limits)
- [6. K8s Reference Docs:](#6-k8s-reference-docs)

# 2. Example 3 node kubernetes cluster.
- Each node has a set of CPU, Memory and Disk resources available.
- If there is no sufficient resources available on any of the nodes, kubernetes holds the scheduling the pod. You will see the pod in pending state. If you look at the events, you will see the reason as insufficient CPU.
  
  ![rl](../../images/rl.PNG)
  
# 3. Resource Requirements
- ~~By default, K8s assume that a pod or container within a pod requires **`0.5`** CPU and **`256Mi`** of memory. This is known as the **`Resource Request` for a container**.~~ <- see section about defaults at the end
  
  ![rr](../../images/rr.PNG)
  
- If your application within the pod requires more than the default resources, you need to set them in the pod definition file.

  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: simple-webapp-color
    labels:
      name: simple-webapp-color
  spec:
   containers:
   - name: simple-webapp-color
     image: simple-webapp-color
     ports:
      - containerPort:  8080
     resources:
       requests:
        memory: "1Gi"
        cpu: "1"
  ```
  ![rr-pod](../../images/rr-pod.PNG) 

- Resource units for CPU: [[REF](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/#resource-units-in-kubernetes)]
> Fractional requests are allowed. A Container with `spec.containers[].resources.requests.cpu` of 0.5 is guaranteed half as much CPU as one that asks for 1 CPU. The expression 0.1 is equivalent to the expression 100m, which can be read as "one hundred millicpu". Some people say "one hundred millicores", and this is understood to mean the same thing. A request with a decimal point, like 0.1, is converted to 100m by the API, and precision finer than 1m is not allowed. For this reason, the form 100m might be preferred.
   
# 4. Resources - Limits
- ~~By default, k8s sets resource limits to 1 CPU and 512Mi of memory~~ <- see section about defaults at the end
  
  ![rsl](../../images/rsl.PNG)
  
- You can set the resource limits in the pod definition file.
  
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: simple-webapp-color
    labels:
      name: simple-webapp-color
  spec:
   containers:
   - name: simple-webapp-color
     image: simple-webapp-color
     ports:
      - containerPort:  8080
     resources:
       requests:
        memory: "1Gi"
        cpu: "1"
       limits:
         memory: "2Gi"
         cpu: "2"
  ```
  ![rsl1](../../images/rsl1.PNG)
  
**Note: Remember Requests and Limits for resources are set per container in the pod.**
  
## 4.1. Exceed Limits
- what happens when a pod tries to exceed resources beyond its limits?
  - For CPU, K8S throttles the cpu usage to use only upto the limit.
  - For Memory, if the pod uses more memory than prescribed consistently, K8S will terminate that pod.

   ![el](../../images/el.PNG)
   
# 5. Default requests and limits

For the POD to pick up the defaults you must have first set those as default values for request and limit by creating a LimitRange in that namespace.

```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: mem-limit-range
spec:
  limits:
  - default:
      memory: 512Mi
    defaultRequest:
      memory: 256Mi
    type: Container
```

```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: cpu-limit-range
spec:
  limits:
  - default:
      cpu: 1
    defaultRequest:
      cpu: 0.5
    type: Container
```

REFS: [CPU](https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/cpu-default-namespace/), [MEM](https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/memory-default-namespace/)
  
# 6. K8s Reference Docs:
- https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/
  
  
