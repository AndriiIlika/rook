# Log

```sh
# Remove prev installetion
$ kubectl -n test delete pvc vol1
$ kubectl delete ns test
$ kubectl delete storageclass rook-ceph-block rook-ceph-block-r2
$ kubectl -n rook-ceph delete cephcluster rook-ceph
$ kubectl -n rook-ceph delete deployment rook-ceph-operator
$ kubectl -n rook-ceph delete daemonset rook-ceph-agent rook-discover
$ kubectl -n rook-ceph get volumes,cephblockpools,cephclusters,cephfilesystems,cephnfses,cephobjectstores,cephobjectstoreusers
$ kubectl -n rook-ceph get pods
```

```sh
# Running Ceph CSI drivers with Rook

# Prerequisites:
# - a Kubernetes v1.13+ is needed in order to support CSI Spec 1.0.
# - --allow-privileged flag set to true in kubelet and your API server
# - An up and running Rook instance (see Rook - Ceph quickstart guide)

# Create RBAC used by CSI drivers in the same namespace as Rook Ceph Operator
# create rbac. Since rook operator is not permitted to create rbac rules,
# these rules have to be created outside of operator
$ kubectl apply -f cluster/examples/kubernetes/ceph/csi/rbac/rbd/
serviceaccount/rook-csi-rbd-plugin-sa created
clusterrole.rbac.authorization.k8s.io/rbd-csi-nodeplugin created
clusterrole.rbac.authorization.k8s.io/rbd-csi-nodeplugin-rules created
clusterrolebinding.rbac.authorization.k8s.io/rbd-csi-nodeplugin created
serviceaccount/rook-csi-rbd-provisioner-sa created
clusterrole.rbac.authorization.k8s.io/rbd-external-provisioner-runner created
clusterrole.rbac.authorization.k8s.io/rbd-external-provisioner-runner-rules created
clusterrolebinding.rbac.authorization.k8s.io/rbd-csi-provisioner-role created

$ kubectl apply -f cluster/examples/kubernetes/ceph/csi/rbac/cephfs/
serviceaccount/rook-csi-cephfs-plugin-sa created
clusterrole.rbac.authorization.k8s.io/cephfs-csi-nodeplugin created
clusterrole.rbac.authorization.k8s.io/cephfs-csi-nodeplugin-rules created
clusterrolebinding.rbac.authorization.k8s.io/cephfs-csi-nodeplugin created
serviceaccount/rook-csi-cephfs-provisioner-sa created
clusterrole.rbac.authorization.k8s.io/cephfs-external-provisioner-runner created
clusterrole.rbac.authorization.k8s.io/cephfs-external-provisioner-runner-rules created
clusterrolebinding.rbac.authorization.k8s.io/cephfs-csi-provisioner-role created

# Start Rook Ceph Operator
$ kubectl apply -f cluster/examples/kubernetes/ceph/operator-with-csi.yaml
deployment.apps/rook-ceph-operator created

# Verify CSI drivers and Operator are up and running
$ kubectl get all -n rook-ceph
```

```sh
# Running Ceph CSI drivers with Rook (2nd run)

# Prerequisites:
# - a Kubernetes v1.13+ is needed in order to support CSI Spec 1.0.
# - --allow-privileged flag set to true in kubelet and your API server
# - An up and running Rook instance (see Rook - Ceph quickstart guide)
$ kubectl create -f cluster/examples/kubernetes/ceph/common.yaml
namespace/rook-ceph created
customresourcedefinition.apiextensions.k8s.io/cephclusters.ceph.rook.io created
customresourcedefinition.apiextensions.k8s.io/cephfilesystems.ceph.rook.io created
customresourcedefinition.apiextensions.k8s.io/cephnfses.ceph.rook.io created
customresourcedefinition.apiextensions.k8s.io/cephobjectstores.ceph.rook.io created
customresourcedefinition.apiextensions.k8s.io/cephobjectstoreusers.ceph.rook.io created
customresourcedefinition.apiextensions.k8s.io/cephblockpools.ceph.rook.io created
customresourcedefinition.apiextensions.k8s.io/volumes.rook.io created
clusterrole.rbac.authorization.k8s.io/rook-ceph-cluster-mgmt created
clusterrole.rbac.authorization.k8s.io/rook-ceph-cluster-mgmt-rules created
role.rbac.authorization.k8s.io/rook-ceph-system created
clusterrole.rbac.authorization.k8s.io/rook-ceph-global created
clusterrole.rbac.authorization.k8s.io/rook-ceph-global-rules created
clusterrole.rbac.authorization.k8s.io/rook-ceph-mgr-cluster created
clusterrole.rbac.authorization.k8s.io/rook-ceph-mgr-cluster-rules created
serviceaccount/rook-ceph-system created
rolebinding.rbac.authorization.k8s.io/rook-ceph-system created
clusterrolebinding.rbac.authorization.k8s.io/rook-ceph-global created
serviceaccount/rook-ceph-osd created
serviceaccount/rook-ceph-mgr created
role.rbac.authorization.k8s.io/rook-ceph-osd created
clusterrole.rbac.authorization.k8s.io/rook-ceph-mgr-system created
clusterrole.rbac.authorization.k8s.io/rook-ceph-mgr-system-rules created
role.rbac.authorization.k8s.io/rook-ceph-mgr created
rolebinding.rbac.authorization.k8s.io/rook-ceph-cluster-mgmt created
rolebinding.rbac.authorization.k8s.io/rook-ceph-osd created
rolebinding.rbac.authorization.k8s.io/rook-ceph-mgr created
rolebinding.rbac.authorization.k8s.io/rook-ceph-mgr-system created
clusterrolebinding.rbac.authorization.k8s.io/rook-ceph-mgr-cluster created

# Create RBAC used by CSI drivers in the same namespace as Rook Ceph Operator
# create rbac. Since rook operator is not permitted to create rbac rules,
# these rules have to be created outside of operator
$ kubectl apply -f cluster/examples/kubernetes/ceph/csi/rbac/rbd/
serviceaccount/rook-csi-rbd-plugin-sa created
clusterrole.rbac.authorization.k8s.io/rbd-csi-nodeplugin created
clusterrole.rbac.authorization.k8s.io/rbd-csi-nodeplugin-rules created
clusterrolebinding.rbac.authorization.k8s.io/rbd-csi-nodeplugin created
serviceaccount/rook-csi-rbd-provisioner-sa created
clusterrole.rbac.authorization.k8s.io/rbd-external-provisioner-runner created
clusterrole.rbac.authorization.k8s.io/rbd-external-provisioner-runner-rules created
clusterrolebinding.rbac.authorization.k8s.io/rbd-csi-provisioner-role created

$ kubectl apply -f cluster/examples/kubernetes/ceph/csi/rbac/cephfs/
serviceaccount/rook-csi-cephfs-plugin-sa created
clusterrole.rbac.authorization.k8s.io/cephfs-csi-nodeplugin created
clusterrole.rbac.authorization.k8s.io/cephfs-csi-nodeplugin-rules created
clusterrolebinding.rbac.authorization.k8s.io/cephfs-csi-nodeplugin created
serviceaccount/rook-csi-cephfs-provisioner-sa created
clusterrole.rbac.authorization.k8s.io/cephfs-external-provisioner-runner created
clusterrole.rbac.authorization.k8s.io/cephfs-external-provisioner-runner-rules created
clusterrolebinding.rbac.authorization.k8s.io/cephfs-csi-provisioner-role created

# Start Rook Ceph Operator
$ kubectl apply -f cluster/examples/kubernetes/ceph/operator-with-csi.yaml
deployment.apps/rook-ceph-operator created

# 2019-06-24 09:57:56.480262 I | rookcmd: starting Rook v1.0.2 with arguments '/usr/local/bin/rook ceph operator'
# 2019-06-24 09:57:56.480387 I | rookcmd: flag values: --alsologtostderr=false, --csi-attacher-image=quay.io/k8scsi/csi-attacher:v1.0.1, --csi-cephfs-image=quay.io/cephcsi/cephfsplugin:v1.0.0, --csi-cephfs-plugin-template-path=/etc/ceph-csi/cephfs/csi-cephfsplugin.yaml, --csi-cephfs-provisioner-template-path=/etc/ceph-csi/cephfs/csi-cephfsplugin-provisioner.yaml, --csi-enable-cephfs=true, --csi-enable-rbd=true, --csi-provisioner-image=quay.io/k8scsi/csi-provisioner:v1.0.1, --csi-rbd-image=quay.io/cephcsi/rbdplugin:v1.0.0, --csi-rbd-plugin-template-path=/etc/ceph-csi/rbd/csi-rbdplugin.yaml, --csi-rbd-provisioner-template-path=/etc/ceph-csi/rbd/csi-rbdplugin-provisioner.yaml, --csi-registrar-image=quay.io/k8scsi/csi-node-driver-registrar:v1.0.2, --csi-snapshotter-image=quay.io/k8scsi/csi-snapshotter:v1.0.1, --help=false, --log-flush-frequency=5s, --log-level=INFO, --log_backtrace_at=:0, --log_dir=, --log_file=, --logtostderr=true, --mon-healthcheck-interval=45s, --mon-out-timeout=10m0s, --skip_headers=false, --stderrthreshold=2, --v=0, --vmodule=
# 2019-06-24 09:57:56.561090 I | cephcmd: starting operator
# 2019-06-24 09:57:56.759095 I | op-agent: getting flexvolume dir path from FLEXVOLUME_DIR_PATH env var
# 2019-06-24 09:57:56.759129 I | op-agent: flexvolume dir path env var FLEXVOLUME_DIR_PATH is not provided. Defaulting to: /usr/libexec/kubernetes/kubelet-plugins/volume/exec/
# 2019-06-24 09:57:56.759137 I | op-agent: discovered flexvolume dir path from source default. value: /usr/libexec/kubernetes/kubelet-plugins/volume/exec/
# 2019-06-24 09:57:56.759145 I | op-agent: no agent mount security mode given, defaulting to 'Any' mode
# 2019-06-24 09:57:56.759152 W | op-agent: Invalid ROOK_ENABLE_SELINUX_RELABELING value "". Defaulting to "true".
# 2019-06-24 09:57:56.759159 W | op-agent: Invalid ROOK_ENABLE_FSGROUP value "". Defaulting to "true".
# 2019-06-24 09:57:56.780213 I | op-agent: rook-ceph-agent daemonset started
# 2019-06-24 09:57:56.785263 I | op-discover: rook-discover daemonset started
# 2019-06-24 09:57:56.786174 I | operator: Ceph CSI driver is enabled, validate csi param
# 2019-06-24 09:57:57.164173 I | operator: successfully started Ceph csi drivers
# 2019-06-24 09:57:57.165162 I | operator: rook-provisioner ceph.rook.io/block started using ceph.rook.io flex vendor dir
# I0624 09:57:57.165325       6 leaderelection.go:217] attempting to acquire leader lease  rook-ceph/ceph.rook.io-block...
# 2019-06-24 09:57:57.165500 I | operator: rook-provisioner rook.io/block started using rook.io flex vendor dir

# Rook agent daemonset always created with flexvolume mount (rook/pkg/operator/ceph/agent/agent.go)
# For CoreOS/RHCOS use new default flexvolume plugins location (https://github.com/rook/rook/issues/2612)
# - name: FLEXVOLUME_DIR_PATH
#   value: "/etc/kubernetes/kubelet-plugins/volume/exec"
# - name: ROOK_HOSTPATH_REQUIRES_PRIVILEGED
#   value: "true"

# Reapply Rook Ceph Operator manifests
$ kubectl apply -f cluster/examples/kubernetes/ceph/operator-with-csi.yaml
deployment.apps/rook-ceph-operator configured

# Verify CSI drivers and Operator are up and running
$ kubectl get all -n rook-ceph

NAME                                      READY   STATUS    RESTARTS   AGE
pod/csi-cephfsplugin-22mkc                2/2     Running   1          134m
pod/csi-cephfsplugin-pfl27                2/2     Running   1          134m
pod/csi-cephfsplugin-pfn5n                2/2     Running   0          134m
pod/csi-cephfsplugin-provisioner-0        2/2     Running   0          134m
pod/csi-rbdplugin-p9t9k                   2/2     Running   1          134m
pod/csi-rbdplugin-pmlxh                   2/2     Running   1          134m
pod/csi-rbdplugin-provisioner-0           4/4     Running   2          134m
pod/csi-rbdplugin-s2tpb                   2/2     Running   0          134m
pod/nsbox-566ff4d9cd-95gh2                1/1     Running   0          3h5m
pod/rook-ceph-agent-d5j9g                 1/1     Running   0          102s
pod/rook-ceph-agent-kdl8z                 1/1     Running   0          102s
pod/rook-ceph-agent-rlcb5                 1/1     Running   0          102s
pod/rook-ceph-operator-6cb8bf9f95-fj9tc   1/1     Running   0          106s
pod/rook-discover-9vwlw                   1/1     Running   0          134m
pod/rook-discover-c9z4n                   1/1     Running   0          134m
pod/rook-discover-wwc6w                   1/1     Running   0          134m

NAME                                   TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)     AGE
service/csi-cephfsplugin-provisioner   ClusterIP   10.3.101.238   <none>        1234/TCP    134m
service/csi-rbdplugin-provisioner      ClusterIP   10.3.227.224   <none>        1234/TCP    134m
service/tiller-deploy                  ClusterIP   10.3.157.201   <none>        44134/TCP   3h5m

NAME                              DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
daemonset.apps/csi-cephfsplugin   3         3         3       3            3           <none>          134m
daemonset.apps/csi-rbdplugin      3         3         3       3            3           <none>          134m
daemonset.apps/rook-ceph-agent    3         3         3       3            3           <none>          134m
daemonset.apps/rook-discover      3         3         3       3            3           <none>          134m

NAME                                 READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nsbox                1/1     1            1           3h5m
deployment.apps/rook-ceph-operator   1/1     1            1           135m

NAME                                            DESIRED   CURRENT   READY   AGE
replicaset.apps/nsbox-566ff4d9cd                1         1         1       3h5m
replicaset.apps/rook-ceph-operator-6cb8bf9f95   1         1         1       108s
replicaset.apps/rook-ceph-operator-7fdc556944   0         0         0       135m

NAME                                            READY   AGE
statefulset.apps/csi-cephfsplugin-provisioner   1/1     134m
statefulset.apps/csi-rbdplugin-provisioner      1/1     134m

# Create the cluster: manually adding nodes starting with 1 monitor and preffered count of 3.
$ kubectl create -f cluster/examples/kubernetes/ceph/cluster.yaml
cephcluster.ceph.rook.io/rook-ceph created

# 2019-06-25 14:11:48.385562 I | rookcmd: starting Rook v1.0.2 with arguments '/rook/rook ceph osd start -- --foreground --id 0 --osd-uuid b58756a6-61d0-4275-90bb-3edfa376d25f --conf /var/lib/rook/osd0/rook-ceph.config --cluster ceph --default-log-to-file false'
# 2019-06-25 14:11:48.385686 I | rookcmd: flag values: --help=false, --log-flush-frequency=5s, --log-level=INFO, --osd-id=0, --osd-store-type=bluestore, --osd-uuid=b58756a6-61d0-4275-90bb-3edfa376d25f
# 2019-06-25 14:11:48.385693 I | op-mon: parsing mon endpoints: 
# 2019-06-25 14:11:48.385699 W | op-mon: ignoring invalid monitor 
# 2019-06-25 14:11:48.386035 I | exec: Running command: stdbuf -oL ceph-volume lvm activate --no-systemd --bluestore 0 b58756a6-61d0-4275-90bb-3edfa376d25f
# 2019-06-25 14:11:51.628914 I | Running command: /bin/mount -t tmpfs tmpfs /var/lib/ceph/osd/ceph-0
# 2019-06-25 14:11:53.771420 I | Running command: /usr/sbin/restorecon /var/lib/ceph/osd/ceph-0
# 2019-06-25 14:11:55.970277 I | Running command: /bin/chown -R ceph:ceph /var/lib/ceph/osd/ceph-0
# 2019-06-25 14:11:58.078964 I | Running command: /bin/ceph-bluestore-tool --cluster=ceph prime-osd-dir --dev /dev/ceph-abf02f38-4c36-4afd-85f5-cc8e9ec9eb59/osd-data-449f1246-efc0-4a87-b245-b63a92743f69 --path /var/lib/ceph/osd/ceph-0 --no-mon-config
# 2019-06-25 14:12:00.470966 I | Running command: /bin/ln -snf /dev/ceph-abf02f38-4c36-4afd-85f5-cc8e9ec9eb59/osd-data-449f1246-efc0-4a87-b245-b63a92743f69 /var/lib/ceph/osd/ceph-0/block
# 2019-06-25 14:12:02.480764 I | Running command: /bin/chown -h ceph:ceph /var/lib/ceph/osd/ceph-0/block
# 2019-06-25 14:12:04.571880 I | Running command: /bin/chown -R ceph:ceph /dev/mapper/ceph--abf02f38--4c36--4afd--85f5--cc8e9ec9eb59-osd--data--449f1246--efc0--4a87--b245--b63a92743f69
# 2019-06-25 14:12:06.576593 I | Running command: /bin/chown -R ceph:ceph /var/lib/ceph/osd/ceph-0
# 2019-06-25 14:12:08.585189 I | --> ceph-volume lvm activate successful for osd ID: 0
# 2019-06-25 14:12:08.676731 I | exec: Running command: ceph-osd --foreground --id 0 --osd-uuid b58756a6-61d0-4275-90bb-3edfa376d25f --conf /var/lib/rook/osd0/rook-ceph.config --cluster ceph --default-log-to-file false
# failed to start osd. Failed to complete '': signal: segmentation fault (core dumped). 

# 2019-06-25 14:21:52.278679 I | op-osd: osd.0 is marked 'DOWN'
# 2019-06-25 14:21:52.278715 W | op-osd: waiting for the osd.0 to exceed the grace period
# 2019-06-25 14:21:52.278722 I | op-osd: osd.1 is marked 'DOWN'
# 2019-06-25 14:21:52.278728 W | op-osd: waiting for the osd.1 to exceed the grace period
# 2019-06-25 14:22:55.683490 I | op-osd: osd.0 is marked 'DOWN'
# 2019-06-25 14:22:55.683546 W | op-osd: osd.0 has been down for longer than the grace period (down since 2019-06-25 14:12:21.574966836 +0000 UTC m=+1912.868985930)
# 2019-06-25 14:22:55.683554 I | op-osd: osd.1 is marked 'DOWN'
# 2019-06-25 14:22:55.683563 W | op-osd: osd.1 has been down for longer than the grace period (down since 2019-06-25 14:12:21.574974793 +0000 UTC m=+1912.868993851)

# Got https://github.com/rook/rook/issues/3157
$ kubectl -n rook-ceph get cm rook-ceph-mon-endpoints -ojsonpath='{.data.data}'
a=10.3.186.25:6789,b=10.3.185.104:6789,c=10.3.153.9:6789

# debug 2019-06-25 15:09:32.999 7fe279ab3a00 -1 ERROR: on disk data includes unsupported features: compat={},rocompat={},incompat={11=nautilus ondisk layout}
# debug 2019-06-25 15:09:32.999 7fe279ab3a00 -1 error checking features: (1) Operation not permitted

# https://tracker.ceph.com/versions/574

# Add remaining nodes to the cluster manually
$ kubectl apply -f cluster/examples/kubernetes/ceph/cluster.yaml
cephcluster.ceph.rook.io/rook-ceph configured

# 2019-06-26 16:43:16.289341 I | op-mgr: skipping enabling orchestrator modules on releases older than nautilus
# https://github.com/rook/rook/pull/2187
# https://github.com/rook/rook/issues/2213

# Launch the rook-ceph-tools pod (after cluster is running)
$ kubectl create -f cluster/examples/kubernetes/ceph/toolbox.yaml
deployment.apps/rook-ceph-tools created

$ kubectl -n rook-ceph get cephcluster rook-ceph -o yaml
apiVersion: ceph.rook.io/v1
kind: CephCluster
metadata:
  name: rook-ceph
  namespace: rook-ceph
spec:
  cephVersion:
    allowUnsupported: false
    image: ceph/ceph:v14.2.1-20190430
  dashboard:
    enabled: true
  dataDirHostPath: /var/lib/rook
  mon:
    count: 1
    preferredCount: 3
    allowMultiplePerNode: false
  network:
    hostNetwork: false
  rbdMirroring:
    workers: 0
  storage:
    useAllDevices: false
    useAllNodes: false
    deviceFilter: ^sd.
    config:
      databaseSizeMB: "1024"
      journalSizeMB: "1024"
      osdsPerDevice: "1"
    nodes:
    - deviceFilter: ^sd.
      name: server12
    - deviceFilter: ^sd.
      name: server15
    - deviceFilter: ^sd.
      name: server31
status:
  state: Creating

```

```yaml
# core@server31 ~ $ kubectl get storageclass rook-ceph-block -oyaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  creationTimestamp: "2019-06-13T12:14:41Z"
  name: rook-ceph-block
  resourceVersion: "270716"
  selfLink: /apis/storage.k8s.io/v1/storageclasses/rook-ceph-block
  uid: d2678c9e-8dd4-11e9-a101-0e4bd1f2a3d0
parameters:
  blockPool: replicapool
  clusterNamespace: rook-ceph
  fstype: xfs
provisioner: ceph.rook.io/block
reclaimPolicy: Delete
volumeBindingMode: Immediate
# core@server31 ~ $ kubectl get storageclass rook-ceph-block-r2 -oyaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  creationTimestamp: "2019-06-13T12:27:53Z"
  name: rook-ceph-block-r2
  resourceVersion: "273488"
  selfLink: /apis/storage.k8s.io/v1/storageclasses/rook-ceph-block-r2
  uid: aa72987f-8dd6-11e9-a101-0e4bd1f2a3d0
parameters:
  blockPool: replicapool-r2
  clusterNamespace: rook-ceph
  fstype: xfs
provisioner: ceph.rook.io/block
reclaimPolicy: Delete
volumeBindingMode: Immediate
# core@server31 ~ $ kubectl -n rook-ceph get cephcluster rook-ceph -oyaml
apiVersion: ceph.rook.io/v1
kind: CephCluster
metadata:
  annotations:
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"ceph.rook.io/v1","kind":"CephCluster","metadata":{"annotations":{},"name":"rook-ceph","namespace":"rook-ceph"},"spec":{"annotations":null,"cephVersion":{"allowUnsupported":false,"image":"ceph/ceph:v13.2.6-20190604"},"dashboard":{"enabled":true},"dataDirHostPath":"/var/lib/rook","mon":{"allowMultiplePerNode":false,"count":3},"network":{"hostNetwork":false},"rbdMirroring":{"workers":0},"resources":null,"storage":{"config":{"databaseSizeMB":"1024","journalSizeMB":"1024","osdsPerDevice":"1"},"deviceFilter":"^sd.","location":null,"useAllDevices":false,"useAllNodes":true}}}
  creationTimestamp: "2019-06-12T15:56:53Z"
  finalizers:
  - cephcluster.ceph.rook.io
  generation: 12409
  name: rook-ceph
  namespace: rook-ceph
  resourceVersion: "2597972"
  selfLink: /apis/ceph.rook.io/v1/namespaces/rook-ceph/cephclusters/rook-ceph
  uid: b2a7f759-8d2a-11e9-a101-0e4bd1f2a3d0
spec:
  cephVersion:
    image: ceph/ceph:v13.2.6-20190604
  dashboard:
    enabled: true
  dataDirHostPath: /var/lib/rook
  mon:
    allowMultiplePerNode: false
    count: 3
    preferredCount: 0
  network:
    hostNetwork: false
  rbdMirroring:
    workers: 0
  storage:
    config:
      databaseSizeMB: "1024"
      journalSizeMB: "1024"
      osdsPerDevice: "1"
    deviceFilter: ^sd.
    useAllDevices: false
    useAllNodes: true
status:
  ceph:
    health: HEALTH_OK
    lastChanged: "2019-06-19T09:42:36Z"
    lastChecked: "2019-06-21T10:50:56Z"
    previousHealth: HEALTH_WARN
  state: Created
```

```yaml
apiVersion: ceph.rook.io/v1
kind: CephCluster
metadata:
  name: rook-ceph
  namespace: rook-ceph
spec:
  cephVersion:
    image: ceph/ceph:v14.2.1-20190430
  dashboard:
    enabled: true
  dataDirHostPath: /var/lib/rook
  mon:
    allowMultiplePerNode: false
    count: 3
    preferredCount: 0
  network:
    hostNetwork: false
  rbdMirroring:
    workers: 0
  storage:
    config:
      databaseSizeMB: "1024"
      journalSizeMB: "1024"
      osdsPerDevice: "1"
    deviceFilter: ^sd.
    useAllDevices: false
    useAllNodes: true
```
