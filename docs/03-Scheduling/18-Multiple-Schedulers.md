# 1. Multiple Schedulers 
Take me to [Video Tutorial](https://kodekloud.com/topic/multiple-schedulers/)

- [1. Multiple Schedulers](#1-multiple-schedulers)
  - [1.1. Custom Schedulers](#11-custom-schedulers)
  - [1.2. Deploy additional scheduler](#12-deploy-additional-scheduler)
  - [1.3. Deploy additional scheduler - kubeadm](#13-deploy-additional-scheduler---kubeadm)
  - [1.4. View Schedulers](#14-view-schedulers)
  - [1.5. Use the Custom Scheduler](#15-use-the-custom-scheduler)
  - [1.6. View Events](#16-view-events)
  - [1.7. View Scheduler Logs](#17-view-scheduler-logs)
- [2. K8s Reference Docs](#2-k8s-reference-docs)

In this section, we will take a look at multiple schedulers

## 1.1. Custom Schedulers
- Your kubernetes cluster can schedule multiple schedulers at the same time.

  ![ms](../../images/ms.PNG)
  
## 1.2. Deploy additional scheduler
- Download the binary (This is how we deploy the default scheduler)
  ```
  $ wget https://storage.googleapis.com/kubernetes-release/release/v1.12.0/bin/linux/amd64/kube-scheduler
  ```
  ![das](../../images/das.PNG)
  
## 1.3. Deploy additional scheduler - kubeadm
   
We can copy the same `kube-scheduler.yaml` file and change a few parameters to deploy a custom scheduler:

  ![dask](../../images/dask.PNG)
  
Changed parameters: 
- `--scheduler-name=my-custom-scheduler`
- `--leader-elect=true` : If multiple copies of the same scheduler is available set this to false if you don't have multiple masters. If you do have multiple masters pass in a new parameter `--lock-object-name`
- `--lock-object-name=my-custom-scheduler`

  - To create a scheduler pod
    ```
    $ kubectl create -f my-custom-scheduler.yaml
    ```
  
## 1.4. View Schedulers
- To list the scheduler pods
  ```
  $ kubectl get pods -n kube-system
  ```

## 1.5. Use the Custom Scheduler
- Create a pod definition file and add new section called **`schedulerName`** and specify the name of the new scheduler
  ```
  apiVersion: v1
  kind: Pod
  metadata:
    name: nginx
  spec:
    containers:
    - image: nginx
      name: nginx
    schedulerName: my-custom-scheduler
  ```
  ![cs](../../images/cs.png)
  
- To create a pod definition
  ```
  $ kubectl create -f pod-definition.yaml
  ```
- To list pods
  ```
  $ kubectl get pods
  ```

## 1.6. View Events
- To view events
  ```
  $ kubectl get events
  ```
  ![cs1](../../images/cs1.PNG)
  
## 1.7. View Scheduler Logs
- To view scheduler logs
  ```
  $ kubectl logs my-custom-scheduler -n kube-system
  ```
  ![cs2](../../images/cs2.PNG)
  
# 2. K8s Reference Docs
- https://kubernetes.io/docs/tasks/extend-kubernetes/configure-multiple-schedulers/
  
