# Static Pods 
  - Take me to [Video Tutorial](https://kodekloud.com/topic/static-pods/)
  
In this section, we will take a look at Static Pods

#### How do you provide a pod definition file to the kubelet without a kube-apiserver?
- You can configure the kubelet to read the pod definition files from a directory on the server designated to store information about pods.

## Configure Static Pod
- The designated directory can be any directory on the host and the location of that directory is passed in to the kubelet as an option while running the service.
  - The option is named as **`--pod-manifest-path`**.
  
  ![sp](../../images/sp.PNG)
  
## Another way to configure static pod 
- Instead of specifying the option directly in the **`kubelet.service`** file, you could provide a path to another config file using the config option, and define the directory path as staticPodPath in the file.

  ![sp1](../../images/sp1.PNG)

## View the static pods
- To view the static pods (We have to use the docker command since in a situation where only the kubelet service is configured, there's no `kube-apiserver` in this case and therefore no `kubectl`)
  ```
  $ docker ps
  ```
  ![sp2](../../images/sp2.PNG)

#### The kubelet can create both kinds of pods - the static pods and the ones from the api server at the same time.

  ![sp3](../../images/sp3.PNG)

in this case, the `kube-apiserver` will still be aware of the static pods. So you can use `kubectl get pods` to view static pods too. When kubelet creates a static pod, it creates a **read only** mirror object in the `kube-apiserver`. The static pod name will also contain the name of the node when you view it using `kubectl` command. 

## Static Pods - Use Case

  ![sp4](../../images/sp4.PNG)
  
  ![sp5](../../images/sp5.PNG)
  
## Static Pods vs DaemonSets

   ![spvsds](../../images/spvsds.PNG)
  

#### K8s Reference Docs
- https://kubernetes.io/docs/tasks/configure-pod-container/static-pod/
