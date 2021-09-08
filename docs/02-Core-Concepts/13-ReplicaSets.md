# 1. ReplicaSets
  - Take me to [Video Tutorial](https://kodekloud.com/topic/replicasets/)

In this section, we will take a look at the below
- Replication Controller
- ReplicaSet

Controllers are the brain behind kubernetes

## 1.1. What is a Replica and Why do we need a replication controller?

Replication controller can even be used in cases where you only have one pod. In these cases the Replication controller will bring up the pod if the pod fails.

  ![rc](../../images/rc.PNG)
  
Replication controller also helps creating multiple pods for scaling.

  ![rc1](../../images/rc1.PNG)
  
## 1.2. Difference between ReplicaSet and Replication Controller
- **`Replication Controller`** is the older technology that is being replaced by a **`ReplicaSet`**.
- **`ReplicaSet`** is the new way to setup replication.

## 1.3. Creating a Replication Controller

## 1.4. Replication Controller Definition File
  
   ![rc2](../../images/rc2.PNG)
  
```yaml
apiVersion: v1
kind: ReplicationController
metadata:
  name: myapp-rc
  labels:
    app: myapp
    type: front-end
spec:
  template:
    metadata: # This part is the same as the pod definition file without apiVersion and kind fields.
      name: myapp-pod
      labels:
        app: myapp
        type: front-end
    spec:
      containers:
      - name: nginx-container
        image: nginx
  replicas: 3 #number of replicas needed
```
  - To Create the replication controller
    ```
    $ kubectl create -f rc-definition.yaml
    ```
  - To list all the replication controllers
    ```
    $ kubectl get replicationcontroller
    ```
  - To list pods that are launch by the replication controller
    ```
    $ kubectl get pods
    ```
    ![rc3](../../images/rc3.PNG)
    
## 1.5. Creating a ReplicaSet
  
### 1.6. ReplicaSet Definition File

   ![rs](../../images/rs.PNG)

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp-replicaset
  labels:
    app: myapp
    type: front-end
spec:
  template:
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        type: front-end
    spec:
      containers:
      - name: nginx-container
        image: nginx
  replicas: 3
  selector: # this is one of the major differences between replication controller and replicaset
    matchLabels:
      type: front-end
 ```

**ReplicaSet requires a selector definition when compared to Replication Controller.** This is because replicaset can manage pods that were created outside the replicaset definition. If there are any running pods that match the selector specification, the replicaset will manage those too.

Replication controller can still have a selector, but it's not required for replication controllers. But this is **required** for Replicasets.
   
  - To Create the replicaset
    ```
    $ kubectl create -f replicaset-definition.yaml
    ```
  - To list all the replicaset
    ```
    $ kubectl get replicaset
    ```
  - To list pods that are launch by the replicaset
    ```
    $ kubectl get pods
    ```
   
    ![rs1](../../images/rs1.PNG)
    
## 1.7. Labels and Selectors
### 1.7.1. What is the deal with Labels and Selectors? Why do we label pods and objects in kubernetes?

  ![labels](../../images/labels.PNG)
  
## 1.8. How to scale replicaset
- There are multiple ways to scale replicaset
  - First way is to update the number of replicas in the replicaset-definition.yaml definition file. E.g replicas: 6 and then run 
 ```
    apiVersion: apps/v1
    kind: ReplicaSet
    metadata:
      name: myapp-replicaset
      labels:
        app: myapp
        type: front-end
    spec:
     template:
        metadata:
          name: myapp-pod
          labels:
            app: myapp
            type: front-end
        spec:
         containers:
         - name: nginx-container
           image: nginx
     replicas: 6
     selector:
       matchLabels:
        type: front-end
```

  ```
  $ kubectl apply -f replicaset-definition.yaml
  ```
  - Second way is to use **`kubectl scale`** command.
  ```
  $ kubectl scale --replicas=6 -f replicaset-definition.yaml
  ```
  - Third way is to use **`kubectl scale`** command with type and name
  ```
  $ kubectl scale --replicas=6 replicaset myapp-replicaset
  ```
  ![rs2](../../images/rs2.PNG)

## 1.9. K8s Reference Docs:
- https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/
- https://kubernetes.io/docs/concepts/workloads/controllers/replicationcontroller/
