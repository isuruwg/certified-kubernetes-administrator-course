# Practice Test - Multiple Schedulers
  - Take me to [Practice Test](https://kodekloud.com/topic/practice-test-multiple-schedulers/)
  
Solutions to practice test - multiple schedulers
- Run the command 'kubectl get pods --namespace=kube-system'
  
  <details>

  ```
  $ kubectl get pods --namespace=kube-system
  ```
  </details>

- Run the command 'kubectl describe pod kube-scheduler-master --namespace=kube-system'

  <details>

  ```
  $ kubectl describe pod kube-scheduler-master --namespace=kube-system
  ```
  </details>

- Use the file at /etc/kubernetes/manifests/kube-scheduler.yaml to create your own scheduler. View answer file at /var/answers

Copy kube-scheduler.yaml from the directory /etc/kubernetes/manifests/ to any other location and then change the name to my-scheduler.

Add or update the following command arguments in the YAML file:

- --leader-elect=false
- --port=10282
- --scheduler-name=my-scheduler
- --secure-port=0
Here, we are setting leader-elect to false for our new custom scheduler called my-scheduler.

We are also making use of a different port 10282 which is not currently in use in the controlplane.

The default scheduler uses secure-port on port 10259 to serve HTTPS with authentication and authorization. This is not needed for our custom scheduler, so we can disable HTTPS by setting the value of secure-port to 0.


Finally, because we have set secure-port to 0, replace HTTPS with HTTP and use the correct ports under liveness and startup probes.

The final YAML file would look something like this:
```yaml
---
apiVersion: v1
kind: Pod
metadata:
  labels:
    component: my-scheduler
    tier: control-plane
  name: my-scheduler
  namespace: kube-system
spec:
  containers:
  - command:
    - kube-scheduler
    - --authentication-kubeconfig=/etc/kubernetes/scheduler.conf
    - --authorization-kubeconfig=/etc/kubernetes/scheduler.conf
    - --bind-address=127.0.0.1
    - --kubeconfig=/etc/kubernetes/scheduler.conf
    - --leader-elect=false
    - --port=10282
    - --scheduler-name=my-scheduler
    - --secure-port=0
    image: k8s.gcr.io/kube-scheduler:v1.19.0
    imagePullPolicy: IfNotPresent
    livenessProbe:
      failureThreshold: 8
      httpGet:
        host: 127.0.0.1
        path: /healthz
        port: 10282
        scheme: HTTP
      initialDelaySeconds: 10
      periodSeconds: 10
      timeoutSeconds: 15
    name: kube-scheduler
    resources:
      requests:
        cpu: 100m
    startupProbe:
      failureThreshold: 24
      httpGet:
        host: 127.0.0.1
        path: /healthz
        port: 10282
        scheme: HTTP
      initialDelaySeconds: 10
      periodSeconds: 10
      timeoutSeconds: 15
    volumeMounts:
    - mountPath: /etc/kubernetes/scheduler.conf
      name: kubeconfig
      readOnly: true
  hostNetwork: true
  priorityClassName: system-node-critical
  volumes:
  - hostPath:
      path: /etc/kubernetes/scheduler.conf
      type: FileOrCreate
    name: kubeconfig
status: {}
```

  <details>

  ```
  $ kubectl create -f my-scheduler.yaml
  ```
  </details>

- Set schedulerName property on pod specification to the name of the new scheduler. File is located at /root/nginx-pod.yaml
  
  <details>

  ```
  master $ grep schedulerName /root/nginx-pod.yaml
  schedulerName: my-scheduler
  
  $ kubectl create -f /root/nginx-pod.yaml
  ```
  </details>


