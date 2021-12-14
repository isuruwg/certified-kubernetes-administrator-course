# 1. Backup and Restore Methods
[Video Tutorial](https://kodekloud.com/topic/backup-and-restore-methods/)

- [1. Backup and Restore Methods](#1-backup-and-restore-methods)
  - [1.1. Backup Candidates](#11-backup-candidates)
  - [1.2. Resource Configuration](#12-resource-configuration)
  - [1.3. Backup - Resource Configs](#13-backup---resource-configs)
  - [1.4. Backup - ETCD](#14-backup---etcd)
  - [1.5. Restore - ETCD](#15-restore---etcd)
- [1.5.0.2. K8s Reference Docs](#1502-k8s-reference-docs)


In this section, we will take a look at backup and restore methods

## 1.1. Backup Candidates
 
 ![bc](../../images/bc.PNG)
 
## 1.2. Resource Configuration
- Imperative way
  
  ![rci](../../images/rci.PNG)

- Declarative Way (Preferred approach)
  ```
  apiVersion: v1
  kind: Pod
  metadata:
    name: myapp-pod
    labels:
      app: myapp
      type: front-end
  spec:
    containers:
    - name: nginx-container
      image: nginx
  ```
 ![rcd](../../images/rcd.PNG)
 
- A good practice is to store resource configurations on source code repositories like github.

  ![rcd1](../../images/rcd1.PNG)

## 1.3. Backup - Resource Configs

  ```bash
  $ kubectl get all --all-namespaces -o yaml > all-deploy-services.yaml # (only for few resource groups)
  ```

- There are many other resource groups that must be considered. There are tools like **`ARK`** or now called **`Velero`** by Heptio that can do this for you.

  ![brc](../../images/brc.PNG)
  
## 1.4. Backup - ETCD
- So, instead of backing up resources as before, you may choose to backup the ETCD cluster itself. 
  
  ![be](../../images/be.PNG)
  
- You can take a snapshot of the etcd database by using **`etcdctl`** utility snapshot save command.
  ```
  $ ETCDCTL_API=3 etcdctl snapshot save snapshot.db
  ```
  ```
  $  ETCDCTL_API=3 etcdctl snapshot status snapshot.db
  ```
  ![be1](../../images/be1.PNG)
  
## 1.5. Restore - ETCD
- To restore etcd from the backup at later in time. First stop kube-apiserver service
  ```
  $ service kube-apiserver stop
  ```
- Run the etcdctl snapshot restore command
- Update the etcd service
- Reload system configs
  ```
  $ systemctl daemon-reload
  ```
- Restart etcd
  ```
  $ service etcd restart
  ```
  
  ![er](../../images/er.PNG)
  
- Start the kube-apiserver
  ```
  $ service kube-apiserver start
  ```

**With all etcdctl commands specify the cert,key,cacert and endpoint for authentication.**

```
$ ETCDCTL_API=3 etcdctl --endpoints=https://[127.0.0.1]:2379 --cacert=/etc/kubernetes/pki/etcd/ca.crt \
  --cert=/etc/kubernetes/pki/etcd/etcd-server.crt \
  --key=/etc/kubernetes/pki/etcd/etcd-server.key snapshot save /tmp/snapshot.db
```

  ![erest](../../images/erest.PNG)
  
# 1.5.0.2. K8s Reference Docs
- https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/


 
