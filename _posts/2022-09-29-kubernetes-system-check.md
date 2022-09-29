---
layout: single
title: "[kubernetes] 클러스터 장애시 확인 TIP"
#subtitle: ""
date: 2022-09-29 17:15:00 +0900
last_modified_at: 2022-09-29 17:15:00 +0900 # sitemap.xml 에서 사용 됨. 

categories: kubernetes
tags:
  - kubernetes  
#  - test
---

Kubernetes 클러스터에 장애가 발생 했을경우 어떤 절차로 점검을 수행 해 볼 수 있을까?



## Kubernetes
### 1. node 확인
- 노드 상태를 점검하자!!!

```bash
$ kubectl get node

NAME          STATUS   ROLES              AGE    VERSION
testm01   Ready    control-plane,master   21d   v1.22.8
testm02   Ready    control-plane,master   79d   v1.22.8
testm03   Ready    control-plane,master   21d   v1.22.8
tests01   Ready    <none>                 21d   v1.22.8
tests02   Ready    <none>                 79d   v1.22.8
testw01   Ready    <none>                 19d   v1.22.8
testw02   Ready    <none>                 19d   v1.22.8
...
```

### 2. pod 확인
```bash
$ kubectl -n kube-system get pod -o wide

NAME                                              READY   STATUS      RESTARTS       AGE    IP                NODE          NOMINATED NODE   READINESS GATES
calico-kube-controllers-9f99675b4-c2f5h           1/1     Running     0              20d    xx.xxx.xx.28      tests02   <none>           <none>
calico-node-22vr5                                 1/1     Running     1 (10d ago)    10d    xx.xxx.xx.45      testw13   <none>           <none>
calico-node-272sx                                 1/1     Running     1 (10d ago)    10d    xx.xxx.xx.34      testw02   <none>           <none>
calico-node-4gzkn                                 1/1     Running     2 (3d7h ago)   3d7h   xx.xxx.xx.71      testw39   <none>           <none>
calico-node-5bcwg                                 1/1     Running     2 (10d ago)    10d    xx.xxx.xx.54      testw22   <none>           <none>
calico-node-5sx8z                                 1/1     Running     2 (10d ago)    
...
```

- 만약 특정 pod에서 특이사항이 보인다면 pod log를 확인해 보자
  ```bash
  $ kubectl -n kube-system logs calico-node-272sx

  2022-09-29 18:05:10.870 [INFO][67] tunnel-ip-allocator/allocateip.go 305: Current address is still valid, do nothing currentAddr="192.168.245.0" type="ipipTunnelAddress"
  Calico node started successfully
  bird: Unable to open configuration file /etc/calico/confd/config/bird6.cfg: No such file or directorybird:
  Unable to open configuration file /etc/calico/confd/config/bird.cfg: No such file or directory
  2022-09-29 18:05:12.527 [INFO][149] tunnel-ip-allocator/watchersyncer.go 89: Start called
  2022-09-29 18:05:12.527 [INFO][149] tunnel-ip-allocator/watchersyncer.go 127: Sending status update Status=wait-for-ready
  2022-09-29 18:05:12.527 [INFO][149] tunnel-ip-allocator/watchersyncer.go 147: Starting main event processing loop  
  ...
  ```

- pod에 복수개의 컨테이너가 구동중인 경우 아래와 같이 컨테이너 로그를 확인할 수 있다.
  ```bash
  $ kubectl -n kube-system logs -c [컨테이너이름]
  ```

- 이슈가 식별되는 pod가 Static pod 인지를 확인하자.  
  - Static pod의 경우 `/etc/kubernetes/manifests/` 하위에 yaml 파일들이 존재한다.
  - kubenetes가 해당 경로의 yaml 파일들을 주기적으로 확인/적용하고 있다.
  - 따라서 `/etc/kubernetes/manifests/` 하위 경로의 yaml 파일을 수정할 경우  
    별도로 apply 가 필요 없이 **자동 반영** 되며 특정 pod의 yaml 파일을  
    `/etc/kubernetes/manifests/` 에서 이동 시키면 해당 pod가 종료 된다.
  

  > **Kubernetes Static Pod**  
  > - node의 kubelet 데몬에 의해 관리되는 pod.  
  > - Static Pod 관련 yaml 은 node 별 `/etc/kubernetes/manifests/` 경로에 존재 한다.  
  > - eg. etcd, api-server ...


### 3. events 확인
- event log를 확인하자.
- pod 생성에 실패하여 pod log를 확인할 수 없을 경우에도 유용한 접근이다.

```bash
$ kubectl -n kube-system get event
$ kubectl -n kube-system get events --sort-by='.lastTimestamp 
```

### 4. kubelet 확인
- 노드 재구동 등의 상황에서 `kubelet`에 오류가 남는 경우들을 경험 했다.
- `systemctl`을 통해 `kubelet` 상태를 확인하자.
  ```bash
  $ systemctl status kubelet
  ● kubelet.service - Kubernetes Kubelet Server
      Loaded: loaded (/etc/systemd/system/kubelet.service; enabled; vendor preset: enabled)
      Active: active (running) since Sat 2022-07-30 14:42:40 KST; 1 months 5 days ago
        Docs: https://github.com/GoogleCloudPlatform/kubernetes
    Main PID: 1898 (kubelet)
        Tasks: 33 (limit: 76673)
      Memory: 146.6M
      CGroup: /system.slice/kubelet.service
              └─1898 /usr/local/bin/kubelet --logtostderr=true --v=3 --node-ip=xx.xxx.xx.xx --hostname-override=testm02p --bootstrap-kubeconfig=/etc/kubernetes/bootstrap-kubelet.conf --config=/etc/kubernetes/kubelet-config.yaml --kubeconf>

  Sep 14 10:52:32 testm02 kubelet[1898]: I0914 10:52:32.477516    1898 kubelet_getters.go:176] "Pod status updated" pod="kube-system/kube-controller-manager-testm02" status=Running
  Sep 14 10:52:32 testm02 kubelet[1898]: I0914 10:52:32.477524    1898 kubelet_getters.go:176] "Pod status updated" pod="kube-system/kube-scheduler-testm02" status=Running
  Sep 14 10:52:32 testm02 kubelet[1898]: I0914 10:52:32.477533    1898 kubelet_getters.go:176] "Pod status updated" pod="kube-system/kube-apiserver-testm02" status=Running
  Sep 14 10:52:32 testm02 kubelet[1898]: I0914 10:52:32.593541    1898 prober.go:125] "Probe succeeded" probeType="Readiness" pod="kube-system/kube-apiserver-testm02" podUID=e76c3b00c5f31111db69ds32688434b4 containerName="kube-apiserv>
  Sep 14 10:52:32 testm02 kubelet[1898]: I0914 10:52:32.642642    1898 prober.go:125] "Probe succeeded" probeType="Liveness" pod="monitoring/po-prometheus-node-exporter-d4hs8" podUID=b852d518-eda2-4913-b589-2c9468778bd1 containerName="nod>
  Sep 14 10:52:32 testm02 kubelet[1898]: I0914 10:52:32.642661    1898 prober.go:125] "Probe succeeded" probeType="Readiness" pod="monitoring/po-prometheus-node-exporter-d4hs8" podUID=b852d518-eda2-4913-b589-2c9468778bd1 containerName="no>
  Sep 14 10:52:32 testm02 kubelet[1898]: I0914 10:52:32.660968    1898 prober.go:125] "Probe succeeded" probeType="Liveness" pod="trident/trident-csi-2245t" podUID=ec5450c3-8ed1-4859-b313-ca353c42071d containerName="trident-main"
  Sep 14 10:52:32 testm02 kubelet[1898]: I0914 10:52:32.679132    1898 prober.go:125] "Probe succeeded" probeType="Readiness" pod="trident/trident-csi-2245t" podUID=ec5450c3-8ed1-4859-b313-ca353c42071d containerName="trident-main"
  Sep 14 10:52:33 testm02 kubelet[1898]: I0914 10:52:33.213344    1898 container_manager_linux.go:979] "CPUAccounting not enabled for process" pid=1898
  Sep 14 10:52:33 testm02 kubelet[1898]: I0914 10:52:33.213383    1898 container_manager_linux.go:982] "MemoryAccounting not enabled for process" pid=1898
  ```
- 필요에 따라..  
  가장 기본적인(?) 수리 방법인 **재구동**을 해 줄 수 있다.
  ```bash
  $ systemctl restart kubelet
  ```

## OS
### 1. node 로그 확인
```bash
$ view /var/log/syslog
$ view /var/log/kern.log
```

### 2. 재구동 이력 확인
```bash
$ last reboot
```

### 3. storage 상황 확인
- system 경로, temp 영역이 Full 나는 경험이 있었다.
```bash
$ df -h
```

### 4. network 확인
```bash
$ ifconfig -a

$ route -n

$ iscsiadm -m node

$ multipath -l
```
- mtu 값: 보통 storage 의 경우 9000 / 그 외는 1500 으로 설정이 되어 있다.



## 원인 확인 후 대응/조치 방법
- 이렇게 찾은 특이점에 대한 대응은?&nbsp;&nbsp;&nbsp;[CLICK](https://lmgtfy.app/?q=%EA%B5%AC%EA%B8%80+%EA%B2%80%EC%83%89+%EB%B0%A9%EB%B2%95&iie=1)
- : )
