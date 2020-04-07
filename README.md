# oracledb-k8s

Running Oracle DB on top of Kubernetes

## Usage

Clone repository

```bash
$ git clone https://github.com/invaleed/oracledb-k8s.git
```

Edit configuration file (.yml)

```yml
...
    server: __NFS_SERVER_IP_OR_HOSTNAME__
    path: __YOUR_NFS_PATH__
...
    - name : ORACLE_SID
      value : __CHANGEME__
    - name : ORACLE_PDB
      value : __CHANGEME__        
    - name : ORACLE_PWD
      value : __CHANGEME__
```

Apply configuration

```bash
$ kubectl apply -f oracledb-k8s.yaml

$ kubectl get deployment orcldb
NAME     READY   UP-TO-DATE   AVAILABLE   AGE
orcldb   1/1     1            1           48m

$ kubectl get pods
NAME                      READY   STATUS    RESTARTS   AGE
orcldb-6c98dd6dd7-cc4bj   1/1     Running   0          48m
```


## Additional - Create NFS Server using Centos 7

Install nfs server

```bash
$ sudo yum install nfs-utils nfs-utils-lib
```

Enable and start NFS services

```bash
$ sudo systemctl enable rpcbind
$ sudo systemctl enable nfs-server
$ sudo systemctl enable nfs-lock
$ sudo systemctl enable nfs-idmap
$ sudo systemctl start rpcbind
$ sudo systemctl start nfs-server
$ sudo systemctl start nfs-lock
$ sudo systemctl start nfs-idmap
```

Create shared directory
```bash
$ sudo mkdir /var/nfsshare
$ sudo chmod 755 /var/nfsshare
$ sudo chown nfsnobody:nfsnobody /var/nfsshare
```

Export NFS shared directory
```bash
$ sudo vi /etc/exports (add the folowing line)
/var/nfsshare     *(rw,sync,no_root_squash,no_all_squash)
```

Restart service NFS Server

```bash
$ sudo systemctl restart nfs-server
```
Done
