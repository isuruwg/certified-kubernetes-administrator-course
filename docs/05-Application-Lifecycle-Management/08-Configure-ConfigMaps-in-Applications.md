# 1. Configure ConfigMaps in Applications
  - Take me to [Video Tutorial](https://kodekloud.com/topic/configure-configmaps-in-applications/)
  
In this section, we will take a look at configuring configmaps in applications

- [1. Configure ConfigMaps in Applications](#1-configure-configmaps-in-applications)
- [2. ConfigMaps](#2-configmaps)
  - [2.1. The Imperative way](#21-the-imperative-way)
  - [2.2. The Declarative way](#22-the-declarative-way)
  - [2.3. View ConfigMaps](#23-view-configmaps)
  - [2.4. ConfigMap in Pods](#24-configmap-in-pods)
      - [2.4.0.1. There are other ways to inject configuration variables into pod](#2401-there-are-other-ways-to-inject-configuration-variables-into-pod)
      - [2.4.0.2. K8s Reference Docs](#2402-k8s-reference-docs)

# 2. ConfigMaps
- There are 2 phases involved in configuring ConfigMaps. 
  - First, create the configMaps
  - Second, Inject then into the pod.
- There are 2 ways of creating a configmap.
  - Imperative
  - Declarative


## 2.1. The Imperative way

```bash
# If you want to specify multiple key value pairs you can use --from-literal multiple times
$ kubectl create configmap app-config --from-literal=APP_COLOR=blue --from-literal=APP_MODE=prod

# another way using .properties file
$ kubectl create configmap app-config --from-file=app_config.properties
```

![cmi](../../images/cmi.PNG)
    
## 2.2. The Declarative way
    
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_COLOR: blue
  APP_MODE: prod
```
    
Create a config map definition file and run the 'kubectl create` command to deploy it.

```bash
$ kubectl create -f config-map.yaml
```

![cmd1](../../images/cmd1.PNG)
    
## 2.3. View ConfigMaps
- To view configMaps
  
  ```bash
  $ kubectl get configmaps (or)
  $ kubectl get cm
  ```
  
- To describe configmaps
  
  ```bash
  $ kubectl describe configmaps
  ```
   
   ![cmv](../../images/cmv.PNG)
   
 ## 2.4. ConfigMap in Pods
 - Inject configmap in pod
   ```
   apiVersion: v1
   kind: Pod
   metadata:
     name: simple-webapp-color
   spec:
    containers:
    - name: simple-webapp-color
      image: simple-webapp-color
      ports:
      - containerPort: 8080
      envFrom:
      - configMapRef:
          name: app-config
   ```
   ```
   apiVersion: v1
   kind: ConfigMap
   metadata:
     name: app-config
   data:
     APP_COLOR: blue
     APP_MODE: prod
   ```
   ```
   $ kubectl create -f pod-definition.yaml
   ```
  
   ![cmp](../../images/cmp.PNG)
   
 #### 2.4.0.1. There are other ways to inject configuration variables into pod   
 - You can inject it as a **`Single Environment Variable`** 
 - You can inject it as a file in a **`Volume`**
 
   ![cmp1](../../images/cmp1.PNG)
   
 #### 2.4.0.2. K8s Reference Docs
 - https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/
 - https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/#define-container-environment-variables-using-configmap-data
 - https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/#create-configmaps-from-files
