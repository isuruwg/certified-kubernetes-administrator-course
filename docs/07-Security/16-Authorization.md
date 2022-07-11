# 1. Authorization
  - Take me to [Video Tutorial](https://kodekloud.com/topic/authorization/)
  
- [1. Authorization](#1-authorization)
  - [1.1. Why do you need Authorization in your cluster?](#11-why-do-you-need-authorization-in-your-cluster)
  - [1.2. Authorization Mechanisms](#12-authorization-mechanisms)
  - [1.3. Node Authorization](#13-node-authorization)
  - [1.4. ABAC](#14-abac)
  - [1.5. RBAC](#15-rbac)
  - [1.6. Webhook](#16-webhook)
  - [1.7. Authorization Modes](#17-authorization-modes)
- [2. K8s Reference Docs](#2-k8s-reference-docs)

In this section, we will take a look at authorization in kubernetes

## 1.1. Why do you need Authorization in your cluster?
- As an admin, you can do all operations
  ```
  $ kubectl get nodes
  $ kubectl get pods
  $ kubectl delete node worker-2
  ```
  
  ![at1](../../images/at1.PNG)
  
## 1.2. Authorization Mechanisms
- There are different authorization mechanisms supported by kubernetes
  - Node Authorization
  - Attribute-based Authorization (ABAC)
  - Role-Based Authorization (RBAC)
  - Webhook
  
## 1.3. Node Authorization

  ![node-auth](../../images/node-auth.png)
  
## 1.4. ABAC

  ![abac](../../images/abac.PNG)
  
## 1.5. RBAC

  ![rbac](../../images/rbac.PNG)

## 1.6. Webhook
  
- You can outsource the authorization mechanism you can use a tool like Open Policy Agent. 

  ![webhook](../../images/webhook.PNG)
  
## 1.7. Authorization Modes
- The mode options can be defined on the kube-apiserver. The default value is `AlwaysAllow`. `AlwaysAllow` allows all requests without performing any authorization checks. 

  ![mode](../../images/mode.PNG)
  
- When you specify multiple modes, it will authorize in the order in which it is specified

  ![mode1](../../images/mode1.PNG)
  
eg: if a user creates a request in a scenario where we  have `--authorization-mode=Node,RBAC,Webhook` the request goes to `NODE`, gets rejected and forwarded to `RBAC` and is approved.

You can check the current authorization modes configured on the cluster by inspecting the `kube-apiserver` settings. (eg: `kubectl describe pod kube-apiserver-master -n kube-system`)

  # 2. K8s Reference Docs
  - https://kubernetes.io/docs/reference/access-authn-authz/authorization/
  
