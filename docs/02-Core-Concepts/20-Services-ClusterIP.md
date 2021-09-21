# Kubernetes Services - ClusterIP <!-- omit in toc -->
  - Take me to [Video Tutorial](https://kodekloud.com/topic/services-cluster-ip-2/)
  
In this section we will take a look at **`services - ClusterIP`** in kubernetes

- [1. ClusterIP](#1-clusterip)
  - [1.1. What is a right way to establish connectivity between these services or tiers](#11-what-is-a-right-way-to-establish-connectivity-between-these-services-or-tiers)
  - [1.2. To create a service of type ClusterIP](#12-to-create-a-service-of-type-clusterip)
  - [1.3. To list the services](#13-to-list-the-services)
         
# 1. ClusterIP

- In this case the service creates a **`Virtual IP`** inside the cluster to enable communication between different services such as a set of frontend servers to a set of backend servers.
    
    ![srvc1](../../images/srvc1.PNG)
    
## 1.1. What is a right way to establish connectivity between these services or tiers  

- A kubernetes service can help us group the pods together and provide a single interface to access the pod in a group.

  ![srvc2](../../images/srvc2.PNG)
  
## 1.2. To create a service of type ClusterIP

```yaml
apiVersion: v1
kind: Service
metadata:
 name: back-end
spec:
 types: ClusterIP # Default type is ClusterIP, so if not specified, it'll still be ClusterIP
 ports:
 - targetPort: 80 # port where back-end is exposed
   port: 80 # port where service is exposed
 selector: # This is how the pods are selected
   app: myapp
   type: back-end
```
```
$ kubectl create -f service-definition.yaml
```

## 1.3. To list the services
```
$ kubectl get services
```
  ![srvc3](../../images/srvc3.PNG)
   
K8s Reference Docs:
- https://kubernetes.io/docs/concepts/services-networking/service/
- https://kubernetes.io/docs/tutorials/kubernetes-basics/expose/expose-intro/
