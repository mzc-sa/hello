# [협력테스트]k8s_04_tutorial_test.md

Last_Updated: 2020-11-16

# 쿠버네티스 기초 학습

[쿠버네티스 기초 학습](https://kubernetes.io/ko/docs/tutorials/kubernetes-basics/)

## 0. Minikube 클러스터 만들기

[Hello Minikube](https://kubernetes.io/ko/docs/tutorials/hello-minikube/)

- 링크를 클릭한 후, Launch Termainal 클릭 (start.sh 실행)

### 과정

```python
Your Interactive Learning Environment Bash Terminal

start.sh
$
$ start.sh
Starting Kubernetes...minikube version: v1.8.1
commit: cbda04cf6bbe65e987ae52bb393c10099ab62014
* minikube v1.8.1 on Ubuntu 18.04
* Using the none driver based on user configuration
* Running on localhost (CPUs=2, Memory=2460MB, Disk=145651MB) ...
* OS release is Ubuntu 18.04.4 LTS
* Preparing Kubernetes v1.17.3 on Docker 19.03.6 ...
  - kubelet.resolv-conf=/run/systemd/resolve/resolv.conf
* Launching Kubernetes ...
* Enabling addons: default-storageclass, storage-provisioner
* Configuring local host environment ...
* Done! kubectl is now configured to use "minikube"
* The 'dashboard' addon is enabled
Kubernetes Started

$ minikube dashboard
* Verifying dashboard health ...
* Launching proxy ...
* Verifying proxy health ...
http://127.0.0.1:45603/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/

# Katacoda 환경에서는 Preview Port 30000 선택

# 디플로이먼트 만들기

$ kubectl create deployment hello-node --image=k8s.gcr.io/echoserver:1.4
deployment.apps/hello-node created

$ kubectl get deployments
NAME         READY   UP-TO-DATE   AVAILABLE   AGE
hello-node   1/1     1            1           24s

$ kubectl get pods
NAME                          READY   STATUS    RESTARTS   AGE
hello-node-7dc7987866-tfztp   1/1     Running   0          46s

$ kubectl get events
LAST SEEN   TYPE     REASON                    OBJECT                             MESSAGE
68s         Normal   Scheduled                 pod/hello-node-7dc7987866-tfztp    Successfully assigned default/hello-node-7dc7987866-tfztp to minikube
67s         Normal   Pulling                   pod/hello-node-7dc7987866-tfztp    Pulling image "k8s.gcr.io/echoserver:1.4"
66s         Normal   Pulled                    pod/hello-node-7dc7987866-tfztp    Successfully pulled image "k8s.gcr.io/echoserver:1.4"
66s         Normal   Created                   pod/hello-node-7dc7987866-tfztp    Created container echoserver
65s         Normal   Started                   pod/hello-node-7dc7987866-tfztp    Started container echoserver
68s         Normal   SuccessfulCreate          replicaset/hello-node-7dc7987866   Created pod: hello-node-7dc7987866-tfztp
68s         Normal   ScalingReplicaSet         deployment/hello-node              Scaled up replica set hello-node-7dc7987866 to 1
4m45s       Normal   NodeHasSufficientMemory   node/minikube                      Node minikube status is now: NodeHasSufficientMemory
4m45s       Normal   NodeHasNoDiskPressure     node/minikube                      Node minikube status is now: NodeHasNoDiskPressure
4m45s       Normal   NodeHasSufficientPID      node/minikube                      Node minikube status is now: NodeHasSufficientPID
4m25s       Normal   Starting                  node/minikube                      Starting kubelet.
4m25s       Normal   RegisteredNode            node/minikube                      Node minikube event: Registered Node minikube in Controller
4m25s       Normal   NodeHasSufficientMemory   node/minikube                      Node minikube status is now: NodeHasSufficientMemory
4m25s       Normal   NodeHasNoDiskPressure     node/minikube                      Node minikube status is now: NodeHasNoDiskPressure
4m25s       Normal   NodeHasSufficientPID      node/minikube                      Node minikube status is now: NodeHasSufficientPID
4m24s       Normal   NodeAllocatableEnforced   node/minikube                      Updated Node Allocatable limit across pods
4m21s       Normal   NodeReady                 node/minikube                      Node minikube status is now: NodeReady
4m13s       Normal   Starting                  node/minikube                      Starting kube-proxy.

$ kubectl config view
apiVersion: v1
clusters:
- cluster:
    certificate-authority: /root/.minikube/ca.crt
    server: https://172.17.0.31:8443
  name: minikube
contexts:
- context:
    cluster: minikube
    user: minikube
  name: minikube
current-context: minikube
kind: Config
preferences: {}
users:
- name: minikube
  user:
    client-certificate: /root/.minikube/client.crt
    client-key: /root/.minikube/client.key

# 서비스 만들기

$ kubectl expose deployment hello-node --type=LoadBalancer --port=8080
service/hello-node exposed

$ kubectl get services
hello-node   LoadBalancer   10.97.103.230   <pending>     8080:31818/TCP   24s
kubernetes   ClusterIP      10.96.0.1       <none>        443/TCP          6m47s

$ minikube service hello-node
|-----------|------------|-------------|--------------------------|
| NAMESPACE |    NAME    | TARGET PORT |           URL            |
|-----------|------------|-------------|--------------------------|
| default   | hello-node |             | http://172.17.0.31:31818 |
|-----------|------------|-------------|--------------------------|
* Opening service default/hello-node in default browser...
Minikube Dashboard is not supported via the interactive terminal experience.

Please click the 'Preview Port 30000' link above to access the dashboard.
This will now exit. Please continue with the rest of the tutorial.
*
X open url failed: http://172.17.0.31:31818: exit status 1
*
* minikube is exiting due to an error. If the above message is not useful, open an issue:
  - https://github.com/kubernetes/minikube/issues/new/choose

# Katacoda 환경에서는 Select port to view on Host 1 클릭 -> 8080:31818 -> 31818 입력

# 아래와 같 애플리케이션의 응답 확인 가능
CLIENT VALUES:
client_address=172.18.0.1
command=GET
real path=/
query=nil
request_version=1.1
request_uri=http://2886795295-31818-elsy05.environments.katacoda.com:8080/

SERVER VALUES:
server_version=nginx: 1.10.0 - lua: 10001

HEADERS RECEIVED:
accept=text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
accept-encoding=gzip
accept-language=ko-KR,ko;q=0.9,en-US;q=0.8,en;q=0.7
host=2886795295-31818-elsy05.environments.katacoda.com
referer=https://2886795295-elsy05.environments.katacoda.com/
sec-ch-ua="Chromium";v="86", "\"Not\\A;Brand";v="99", "Google Chrome";v="86"
sec-ch-ua-mobile=?0
sec-fetch-dest=document
sec-fetch-mode=navigate
sec-fetch-site=same-site
sec-fetch-user=?1
upgrade-insecure-requests=1
user-agent=Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.183 Safari/537.36
via=1.1 google
x-client-data=CgSM6ZsV
x-cloud-trace-context=64d20c8c7b80b85935c077675547f508/13642678448913339928
x-forwarded-for=221.148.35.240, 35.201.124.219, 130.211.2.200, 35.186.156.29, 172.17.0.54
x-forwarded-proto=https
x-katacoda-host=elsy05
x-real-ip=35.186.156.29
BODY:
-no body in request-

# 애드온 사용하기

$ minikube addons list    # 현재 지원하는 애드온 목록 확인
|-----------------------------|----------|--------------|
|         ADDON NAME          | PROFILE  |    STATUS    |
|-----------------------------|----------|--------------|
| dashboard                   | minikube | enabled ✅   |
| default-storageclass        | minikube | enabled ✅   |
| efk                         | minikube | disabled     |
| freshpod                    | minikube | disabled     |
| gvisor                      | minikube | disabled     |
| helm-tiller                 | minikube | disabled     |
| ingress                     | minikube | disabled     |
| ingress-dns                 | minikube | disabled     |
| istio                       | minikube | disabled     |
| istio-provisioner           | minikube | disabled     |
| logviewer                   | minikube | disabled     |
| metrics-server              | minikube | disabled     |
| nvidia-driver-installer     | minikube | disabled     |
| nvidia-gpu-device-plugin    | minikube | disabled     |
| registry                    | minikube | disabled     |
| registry-creds              | minikube | disabled     |
| storage-provisioner         | minikube | enabled ✅   |
| storage-provisioner-gluster | minikube | disabled     |
|-----------------------------|----------|--------------|

$ minikube addons enable metrics-server    # 애드온 활성화
* The 'metrics-server' addon is enabled

$ kubectl get pod,svc -n kube-system    # 생성한 파드와 서비스 확인
NAME                                   READY   STATUS    RESTARTS   AGE
pod/coredns-6955765f44-qn6dp           1/1     Running   0          13m
pod/coredns-6955765f44-vhfgb           1/1     Running   0          13m
pod/etcd-minikube                      1/1     Running   0          13m
pod/kube-apiserver-minikube            1/1     Running   0          13m
pod/kube-controller-manager-minikube   1/1     Running   0          13m
pod/kube-proxy-g7qrj                   1/1     Running   0          13m
pod/kube-scheduler-minikube            1/1     Running   0          13m
pod/metrics-server-6754dbc9df-nfdt5    1/1     Running   0          50s
pod/storage-provisioner                1/1     Running   0          13m

NAME                     TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                  AGE
service/kube-dns         ClusterIP   10.96.0.10      <none>        53/UDP,53/TCP,9153/TCP   13m
service/metrics-server   ClusterIP   10.103.144.84   <none>        443/TCP                  50s

$ minikube addons disable metrics-server
* "The 'metrics-server' addon is disabled

# 제거하기

$ kubectl delete service hello-node
service "hello-node" deleted

$ kubectl delete deployment hello-node
deployment.apps "hello-node" deleted

$ minikube stop
* Stopping "minikube" in none ...
* "" stopped.

$ minikube delete
* Uninstalling Kubernetes v1.17.3 using kubeadm ...
* Deleting "minikube" in none ...
* Removed all traces of the "minikube" cluster.
```

- 대시보드

    ![k8s_04_tutorial_Module[1-5]_images/Untitled.png](k8s_04_tutorial_Module[1-5]_images/Untitled.png)

# 1. 클러스터 생성하기

## 1. Minikube를 사용해서 클러스터 생성하기

### 목표

- 쿠버네티스 클러스터가 무엇인지 배운다.
- Minikube가 무엇인지 배운다.
- 온라인 터미널을 사용해서 쿠버네티스 클러스터를 시작한다.

### 쿠버네티스 클러스터

- 쿠버네티스는 서로 연결되어 단일 유닛처럼 동작하는 고가용성의 컴퓨터 클러스터를 상호 조정한다.
- 쿠버네티스의 추상화된 개념을 통해 개별 머신에 얽매이지 않고 컨테이너화된 애플리케이션을 클러스터에 배포할 수 있다.
- 이러한 새로운 배포 모델을 활용하려면 애플리케이션을 개별 호스트에 결합되지 않도록 컨테이너화가 필요.
    - 컨테이너화된 애플리케이션은 이전의 배포 모델보다 유연하고 가용성이 높다
- 쿠버네티스는 애플리케이션 컨테이너를 클러스터에 분산시키고 스케줄링하는 일을 보다 효율적으로 자동화.
- 쿠버네티스 클러스터는 두 가지 형태의 자원으로 구성된다.
    1. 마스터는 클러스터를 상호조정한다.
    2. 노드는 애플리케이션을 구동하는 작업자다.

마스터:

클러스터 관리 + 노드 관리 + 스케일링 + 스케쥴링 + 쿠버네티스 API 제공

노드:

클러스터 내 워커 머신 + kubelet 에이전트(마스터와 통신용) + 제공 받은 API로 마스터와 통신

## 1. Module 1 - Create a Kubernetes cluster

- 사전에 셋팅된 Linux 환경에서 진행 (Powered by katakoda)
- minikube 사전 설치 환경
- [참고] 테스트 모듈 환경

    $ cat /etc/os-release
    NAME="Ubuntu"
    VERSION="18.04.4 LTS (Bionic Beaver)"
    ID=ubuntu
    ID_LIKE=debian
    PRETTY_NAME="Ubuntu 18.04.4 LTS"
    VERSION_ID="18.04"
    HOME_URL="[https://www.ubuntu.com/](https://www.ubuntu.com/)"
    SUPPORT_URL="[https://help.ubuntu.com/](https://help.ubuntu.com/)"
    BUG_REPORT_URL="[https://bugs.launchpad.net/ubuntu/](https://bugs.launchpad.net/ubuntu/)"
    PRIVACY_POLICY_URL="[https://www.ubuntu.com/legal/terms-and-policies/privacy-policy](https://www.ubuntu.com/legal/terms-and-policies/privacy-policy)"
    VERSION_CODENAME=bionic
    UBUNTU_CODENAME=bionic

```python
# Kubernetes Bootcamp Terminal

$ minikube version
minikube version: v1.8.1
commit: cbda04cf6bbe65e987ae52bb393c10099ab62014

$ minikube start
* minikube v1.8.1 on Ubuntu 18.04
* Using the none driver based on user configuration
* Running on localhost (CPUs=2, Memory=2460MB, Disk=145651MB) ...
* OS release is Ubuntu 18.04.4 LTS
* Preparing Kubernetes v1.17.3 on Docker 19.03.6 ...
  - kubelet.resolv-conf=/run/systemd/resolve/resolv.conf
* Launching Kubernetes ...
* Enabling addons: default-storageclass, storage-provisioner
* Configuring local host environment ...
* Waiting for cluster to come online ...
* Done! kubectl is now configured to use "minikube"

$ kubectl version    # Kubernetes와 interact를 위해 kubectl CLI 사용
Client Version: version.Info{Major:"1", Minor:"17", GitVersion:"v1.17.3", GitCommit:"06ad960bfd03b39c8310aaf92d1e7c12ce618213", GitTreeState:"clean", BuildDate:"2020-02-11T18:14:22Z", GoVersion:"go1.13.6", Compiler:"gc", Platform:"linux/amd64"}
Server Version: version.Info{Major:"1", Minor:"17", GitVersion:"v1.17.3", GitCommit:"06ad960bfd03b39c8310aaf92d1e7c12ce618213", GitTreeState:"clean", BuildDate:"2020-02-11T18:07:13Z", GoVersion:"go1.13.6", Compiler:"gc", Platform:"linux/amd64"}

$ kubectl cluster-info
Kubernetes master is running at https://172.17.0.54:8443
KubeDNS is running at https://172.17.0.54:8443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.

$ kubectl get nodes
NAME       STATUS   ROLES    AGE     VERSION
minikube   Ready    master   9m45s   v1.17.3

```

# 2. 앱 배포하기

## 2. kubectl을 사용해서 디플로이먼트 생성하기

### 목표

- 애플리케이션 Deployment에 대해 배운다.
- kubectl로 첫 애플리케이션을 쿠버네티스에 배포한다.

### 쿠버네티스 디플로이먼트

- 쿠버네티스 클러스터를 구동시키면, 그 위에 컨테이너화된 애플리케이션을 배포할 수 있다.
    - 배포를 위해서는 쿠버네티스 디플로이먼트 설정 생성 필요
    - 디플로이먼트는 쿠버네티스가 애플리케이션의 인스턴스를 어떻게 생성하고 업데이트 해야 하는지 지시
- 디플로이먼트 생성 시, 쿠버네티스 마스터가 해당 디플로이먼트에 포함된 애플리케이션 인스턴스가 클러스터의 개별 노드에서 실행되도록 스케쥴링
- 애플리케이션 인스턴스가 생성되면 쿠버네티스 디플로이먼트 컨트롤러는 지속적으로 인스턴스를 모니터링
- 애플리케이션에 문제가 생길 경우, 컨트롤러가 인스턴스를 클러스터 내부의 다른 노드의 인스턴스로 교체
    - 자동 복구 (self-healing) 메커니즘 제공

### 쿠버네티스에 첫 번째 애플리케이션 배포하기

- kubectl이라는 쿠버네티스 CLI를 통해 디플로이먼트를 생성하고 관리할 수 있다.
- kubectl은 클러스터와 상호 작용하기 위해 쿠버네티스 API를 사용한다.
- 디플로이먼트를 생성할 때, 애플리케이션에 대한 컨테이너 이미지와 구동시키고자 하는 복제 수 지정 필요
- 첫 번째 디플로이먼트로, 도커 컨테이너로 패키지된 Node.js 애플리케이션을 사용

## 2. Module 2 - Deploy an App (앱 배포하기)

kubectl을 이용하여 첫 번째 앱을 쿠버네티스에 배포하기

```python
# 대화형 튜토리얼 진행

Kubernetes Bootcamp Terminal

$
$ sleep 1; launch.sh
Starting Kubernetes. This is expected to take less than a minute
Kubernetes Started

# kubectl의 일반적인 명령어 -> $ kubectl <action> <resource>

$ kubectl version
Client Version: version.Info{Major:"1", Minor:"17", GitVersion:"v1.17.3", GitCommit:"06ad960bfd03b39c8310aaf92d1e7c12ce618213", GitTreeState:"clean", BuildDate:"2020-02-11T18:14:22Z", GoVersion:"go1.13.6", Compiler:"gc", Platform:"linux/amd64"}
Server Version: version.Info{Major:"1", Minor:"17", GitVersion:"v1.17.3", GitCommit:"06ad960bfd03b39c8310aaf92d1e7c12ce618213", GitTreeState:"clean", BuildDate:"2020-02-11T18:07:13Z", GoVersion:"go1.13.6", Compiler:"gc", Platform:"linux/amd64"}

$ kubectl get nodes
NAME       STATUS   ROLES    AGE     VERSION
minikube   Ready    master   4m46s   v1.17.3

# 앱 배포하기 -> $ kubectl create deployment <deployment name> <app image location>
$ kubectl create deployment kubernetes-bootcamp --image=gcr.io/google-samples/kubernetes-bootcamp:v1
deployment.apps/kubernetes-bootcamp created

# 어플리케이션이 돌아갈 적당한 노드 확인 + 해당 노드에서 돌아갈 어플리케이션 스케쥴링 + 새로운 노드에서 돌아갈 때를 위한 스케쥴링

$ kubectl get deployments    # 배포 리스트 확인 명령어
NAME                  READY   UP-TO-DATE   AVAILABLE   AGE
kubernetes-bootcamp   1/1     1            1           2m30s

# View our app -> 두 번째 터미널을 열어서 진행

$ echo -e "\n\n\n\e[92mStarting Proxy. After starting it will not output a response. Please click the first Terminal Tab\n";

Starting Proxy. After starting it will not output a response. Please click the first Terminal Tab

$ kubectl proxy
Starting to serve on 127.0.0.1:8001

# 다시 Terminal 1으로 돌아와서 진행
$ curl http://localhost:8001/version
{
  "major": "1",
  "minor": "17",
  "gitVersion": "v1.17.3",
  "gitCommit": "06ad960bfd03b39c8310aaf92d1e7c12ce618213",
  "gitTreeState": "clean",
  "buildDate": "2020-02-11T18:07:13Z",
  "goVersion": "go1.13.6",
  "compiler": "gc",
  "platform": "linux/amd64"
}

$ export POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{{.meadata.name}}{{"\n"}}{{end}}')

$ echo Name of the Pod: $POD_NAME
Name of the Pod: kubernetes-bootcamp-69fbc6f4cf-zfb5v

```

# 3. 앱 조사하기

## 3. 파드와 노드 보기

### 목표

- 쿠버네티스 파드에 대해 학습한다.
- 쿠버네티스 노드에 대해 학습한다.
- 배포된 애플리케이션의 문제를 해결한다.

### 쿠버네티스 파드

Deployment를 생성했을 때, 쿠버네티스는 애플리케이션 인스턴스에 파드를 생성한다.

파드는 애플리케이션 컨테이너들의 그룹을 나타내는 쿠버네티스의 추상적 개념으로 일부는 컨테이너에 대한 자원을 공유한다. (공유 스토리지 + 클러스터 IP 주소와 같은 네트워킹 + 이미지 버전, 사용 포트 등 동작 정보)

- ex.Node.js 앱과 함께 Node.js 웹서버에 의해 생성되는 데이터를 공급하는 다른 컨테이너를 함께 수용 가능
- 파드 내 컨테이너는 IP 주소, 포트 스페이스를 공유하고 항상 함께 위치 및 스케쥴링되고 컨텍스트 공유 동작
- 파드는 쿠버네티스 플랫폼 상에서 최소 단위
- 쿠버네티스에서 deployment를 생성할 때, 컨테이너 내부에서 컨테이너와 함께 파드를 생성
- 각 파드는 스케쥴 되어진 노드에 묶임 (소멸 및 삭제되기 전까지 해당 노드에 유지)
- 파드는 기본적으로 독립(private)적으로 작동

### 노드

- 노드는 쿠버네티스에서 워커 머신을 의미하며 각 노드는 마스터에 의해 관리
- 파드는 언제나 노드 상에서 동작
- 하나의 노드는 여러 개의 파드를 가질 수 있으며 쿠버네티스 마스터는 클러스터 내 노드를 통해서 파드에 대한 스케쥴링을 자동으로 처리

![k8s_04_tutorial_Module[1-5]_images/Untitled%201.png](k8s_04_tutorial_Module[1-5]_images/Untitled%201.png)

### kubectl로 문제 해결하기

- kubectl get : 자원을 나열
- kubectl describe : 자원에 대한 상세한 정보 확인
- kubectl logs : 파드 내 컨테이너의 로그 출력
- kubectl exec : 파드 내 컨테이너에 대한 명령 실행

## 3. Module 3 - 앱 조사하기 (Explore your app)

kubectl [get, descirbe, logs, exec] 명령어를 통해 쿠버네티스 어플리케이션 트러블슈팅 진행

```python
Kubernetes Bootcamp Terminal

$ sleep 1; launch.sh
Starting Kubernetes. This is expected to take less than a minute
Kubernetes Started

$ kubectl get pods    # 존재하는 파드 확인 명령어
NAME                                   READY   STATUS    RESTARTS   AGE
kubernetes-bootcamp-765bf4c7b4-45x7s   1/1     Running   0          2m33s

$ kubectl describe pods    # IP 주소, 사용 포트, 라이프사이클, 노드, 파드, 배포 정보 등을 확인 가능
Name:         kubernetes-bootcamp-765bf4c7b4-45x7s
Namespace:    default
Priority:     0
Node:         minikube/172.17.0.21
Start Time:   Wed, 11 Nov 2020 02:04:35 +0000
Labels:       pod-template-hash=765bf4c7b4
              run=kubernetes-bootcamp
Annotations:  <none>
Status:       Running
IP:           172.18.0.3
IPs:
  IP:           172.18.0.3
Controlled By:  ReplicaSet/kubernetes-bootcamp-765bf4c7b4
Containers:
  kubernetes-bootcamp:
    Container ID:   docker://faf6e7bd3514426c4d124eebff877c734a84369180f70f8d541cd759b8c414d5
    Image:          gcr.io/google-samples/kubernetes-bootcamp:v1
    Image ID:       docker-pullable://jocatalin/kubernetes-bootcamp@sha256:0d6b8ee63bb57c5f5b6156f446b3bc3b3c143d233037f3a2f00e279c8fcc64af
    Port:           8080/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Wed, 11 Nov 2020 02:04:37 +0000
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-tcwvk (ro)
Conditions:
  Type              Status
  Initialized       True
  Ready             True
  ContainersReady   True
  PodScheduled      True
Volumes:
  default-token-tcwvk:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-tcwvk
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type     Reason            Age                    From               Message
  ----     ------            ----                   ----               -------
  Warning  FailedScheduling  6m36s (x2 over 6m36s)  default-scheduler  0/1 nodes are available: 1 node(s) had taints that the pod didn't tolerate.
  Normal   Scheduled         6m29s                  default-scheduler  Successfully assigned default/kubernetes-bootcamp-765bf4c7b4-45x7s to minikube
  Normal   Pulled            6m27s                  kubelet, minikube  Container image "gcr.io/google-samples/kubernetes-bootcamp:v1" already present on machine
  Normal   Created           6m27s                  kubelet, minikube  Created container kubernetes-bootcamp
  Normal   Started           6m27s                  kubelet, minikube  Started container kubernetes-bootcamp

# 프록시 명령어는 새 터미널에(Terminal 2)서 입력
Extra Interactive Bash Terminal

$ echo -e "\n\n\n\e[92mStarting Proxy. After starting it will not output a response. Please click the first Terminal Tab\n"; kubectl proxy

Starting Proxy. After starting it will not output a response. Please click the first Terminal Tab

Starting to serve on 127.0.0.1:8001

# Terminal 1 에서 명령어 입력

$ export POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}')

$ echo Name of the Pod: $POD_NAME
Name of the Pod: kubernetes-bootcamp-765bf4c7b4-45x7s

# URL은 파드의 API로 향하는 라우트
$ curl http://localhost:8001/api/v1/namespaces/default/pods/$POD_NAME/proxy/
Hello Kubernetes bootcamp! | Running on: kubernetes-bootcamp-765bf4c7b4-45x7s | v=1

# 로그 검색 명령어
$ kubectl logs $POD_NAME
Kubernetes Bootcamp App Started At: 2020-11-11T02:04:37.785Z | Running On:  kubernetes-bootcamp-765bf4c7b4-45x7s

Running On: kubernetes-bootcamp-765bf4c7b4-45x7s | Total Requests: 1 | App Uptime: 1000.092 seconds | Log Time: 2020-11-11T02:21:17.877Z

# 파드가 올라와서 작동 중일 때, 바로 명령어를 실행하기 (exec)
$ kubectl exec $POD_NAME env
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
HOSTNAME=kubernetes-bootcamp-765bf4c7b4-45x7s
KUBERNETES_SERVICE_PORT_HTTPS=443
KUBERNETES_PORT=tcp://10.96.0.1:443
KUBERNETES_PORT_443_TCP=tcp://10.96.0.1:443
KUBERNETES_PORT_443_TCP_PROTO=tcp
KUBERNETES_PORT_443_TCP_PORT=443
KUBERNETES_PORT_443_TCP_ADDR=10.96.0.1
KUBERNETES_SERVICE_HOST=10.96.0.1
KUBERNETES_SERVICE_PORT=443
NPM_CONFIG_LOGLEVEL=info
NODE_VERSION=6.3.1
HOME=/root

# bash session > 돌아가고 있는 컨테이너(Node.js)에서 콘솔 띄우기 
$ kubectl exec -ti $POD_NAME bash
root@kubernetes-bootcamp-765bf4c7b4-45x7s:/#

# app의 소스코드 확인
root@kubernetes-bootcamp-765bf4c7b4-45x7s:/# cat server.js
var http = require('http');
var requests=0;
var podname= process.env.HOSTNAME;
var startTime;
var host;
var handleRequest = function(request, response) {
  response.setHeader('Content-Type', 'text/plain');
  response.writeHead(200);
  response.write("Hello Kubernetes bootcamp! | Running on: ");
  response.write(host);
  response.end(" | v=1\n");
  console.log("Running On:" ,host, "| Total Requests:", ++requests,"| App Uptime:", (new Date() - startTime)/1000 , "seconds", "| Log Time:",new Date());
}
var www = http.createServer(handleRequest);
www.listen(8080,function () {
    startTime = new Date();;
    host = process.env.HOSTNAME;
    console.log ("Kubernetes Bootcamp App Started At:",startTime, "| Running On: " ,host, "\n" );
});

# curl 명령어로 확인
root@kubernetes-bootcamp-765bf4c7b4-45x7s:/# curl localhost:8080
Hello Kubernetes bootcamp! | Running on: kubernetes-bootcamp-765bf4c7b4-45x7s | v=1

root@kubernetes-bootcamp-765bf4c7b4-45x7s:/# exit
exit
$

```

# 4. 앱 외부로 노출하기

## 4. 앱 노출을 위해 서비스 이용하기

[앱 노출을 위해 서비스 이용하기](https://kubernetes.io/ko/docs/tutorials/kubernetes-basics/expose/expose-intro/)

### 목표

- 쿠버네티스의 서비스에 대해 배운다.
- 레이블과 레이블셀랙터 오브젝트가 어떻게 서비스와 연관되는지 이해한다.
- 서비스를 이용하여 쿠버네티스 클러스터 외부로 애플리케이션을 노출한다.

### 쿠버네티스 서비스들에 대한 개요

- 쿠버네티스의 서비스는 하나의 논리적인 파드 셋(set)과 해당 파드들에 접근할 수 있는 정책을 정의 (추상적)
- 서비스는 종속적인 파드들 사이를 느슨하게 결합 (YAML, JSON을 이용하여 정의)
- 서비스가 대상으로 하는 파드 셋은 보통 LabelSelector에 의해 결정

ClusterIP(기본값) → NodePort → LoadBalancer + ExternalName

### 서비스와 레이블

![k8s_04_tutorial_Module[1-5]_images/Untitled%202.png](k8s_04_tutorial_Module[1-5]_images/Untitled%202.png)

- 서비스는 파드 셋에 걸쳐서 트래픽을 라우트
- 애플리케이션에 영향을 주지 않으면서 쿠버네티스의 파드들이 죽게도하고 복제가 되게도 해주는 추상적 개념
- 서비스는 쿠버네티스 객체들에 대해 논리 연산을 허용해주는 기본 그룹핑 단위인, 레이블과 셀렉터를 이용하여 파드 셋과 매치, 레이블은 오브젝트들에게 붙여진 Key : Value 쌍으로 다양한 방식으로 이용 가능

![k8s_04_tutorial_Module[1-5]_images/Untitled%203.png](k8s_04_tutorial_Module[1-5]_images/Untitled%203.png)

- 레이블은 오브젝트의 생성 시점 또는 이후 시점에 붙여질 수 있으며 언제든지 수정이 가능

## 4. Module 4 - 앱 노출하기 (Expose your app publicly)

- kubectl 명령어를 활용하여 쿠버네티스 어플리케이션을 클러스터 외부로 노출하는 방법 학습
- kubectl label 명령어를 활용하여 label을 오브젝트에 할당하고 확인하는 방법 학습

```python
Kubernetes Bootcamp Terminal

$
$ sleep 1; launch.sh
Starting Kubernetes. This is expected to take less than a minute
Kubernetes Started

# Step 1. Create a new service

# 어플리케이션 동작 확인
$ kubectl get pods    # 어플리케이션 동작 확인
NAME                                   READY   STATUS    RESTARTS   AGE
kubernetes-bootcamp-765bf4c7b4-2m9p4   1/1     Running   0          2m21s

# 클러스터 서비스 확인
$ kubectl get services    # 클러스터의 서비스 확인 명령어
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   3m42s

# NodePort를 파라미터로 외부 노출
$ kubectl expose deployment/kubernetes-bootcamp --type="NodePort" --port 8080
service/kubernetes-bootcamp exposed

# 클러스터 서비스 확인
$ kubectl get services
NAME                  TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
kubernetes            ClusterIP   10.96.0.1        <none>        443/TCP          7m38s
kubernetes-bootcamp   NodePort    10.111.224.255   <none>        8080:30751/TCP   2m47s

# 어떤 포트가 외부로 열렸는지 확인
$ kubectl describe services/kubernetes-bootcamp
Name:                     kubernetes-bootcamp
Namespace:                default
Labels:                   run=kubernetes-bootcamp
Annotations:              <none>
Selector:                 run=kubernetes-bootcamp
Type:                     NodePort
IP:                       10.111.224.255
Port:                     <unset>  8080/TCP
TargetPort:               8080/TCP
NodePort:                 <unset>  30751/TCP
Endpoints:                172.18.0.4:8080
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>

# NODE_PORT 변수 설정
$ export NODE_PORT=$(kubectl get services/kubernetes-bootcamp -o go-template='{{(index .spec.ports 0).nodePort}}')

$ echo NODE_PORT=$NODE_PORT
NODE_PORT=30751

$ curl $(minikube ip):$NODE_PORT
Hello Kubernetes bootcamp! | Running on: kubernetes-bootcamp-765bf4c7b4-2m9p4 | v=1

# Step 2. Using labels

# Label 확인, Deployment는 파드의 label을 자동 생성
$ kubectl describe deployment
Name:                   kubernetes-bootcamp
Namespace:              default
CreationTimestamp:      Mon, 16 Nov 2020 01:50:23 +0000
Labels:                 run=kubernetes-bootcamp
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               run=kubernetes-bootcamp
Replicas:               1 desired | 1 updated | 1 total | 1 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  run=kubernetes-bootcamp
  Containers:
   kubernetes-bootcamp:
    Image:        gcr.io/google-samples/kubernetes-bootcamp:v1
    Port:         8080/TCP
    Host Port:    0/TCP
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  <none>
NewReplicaSet:   kubernetes-bootcamp-765bf4c7b4 (1/1 replicas created)
Events:
  Type    Reason             Age   From                   Message
  ----    ------             ----  ----                   -------
  Normal  ScalingReplicaSet  12m   deployment-controller  Scaled up replica set kubernetes-bootcamp-765bf4c7b4 to 1

# 파드 정보 확인 시, -l 옵션을 활용
$ kubectl get pods -l run=kubernetes-bootcamp
NAME                                   READY   STATUS    RESTARTS   AGE
kubernetes-bootcamp-765bf4c7b4-2m9p4   1/1     Running   0          12m

# 서비스 정보 확인 시, -l 옵션을 활용
$ kubectl get services -l run=kubernetes-bootcamp
NAME                  TYPE       CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
kubernetes-bootcamp   NodePort   10.111.224.255   <none>        8080:30751/TCP   9m7s

# POD_NAME 환경 변수 설정
$ export POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}')

$ echo Name of the Pod: $POD_NAME
Name of the Pod: kubernetes-bootcamp-765bf4c7b4-2m9p4

# 새로운 label 적용 명령어 [object type, object name, new label]
$ kubectl label pod $POD_NAME app=v1
pod/kubernetes-bootcamp-765bf4c7b4-2m9p4 labeled

# 변경된 label 확인
$ kubectl describe pods $POD_NAME
Name:         kubernetes-bootcamp-765bf4c7b4-2m9p4
Namespace:    default
Priority:     0
Node:         minikube/172.17.0.51
Start Time:   Mon, 16 Nov 2020 01:50:37 +0000
Labels:       app=v1
              pod-template-hash=765bf4c7b4
              run=kubernetes-bootcamp
Annotations:  <none>
Status:       Running
IP:           172.18.0.4
IPs:
  IP:           172.18.0.4
Controlled By:  ReplicaSet/kubernetes-bootcamp-765bf4c7b4
Containers:
  kubernetes-bootcamp:
    Container ID:   docker://1e5a9b243194757edbb6790a8ed4a0382fc5c70efa4e210576b93f037e329cfa
    Image:          gcr.io/google-samples/kubernetes-bootcamp:v1
    Image ID:       docker-pullable://jocatalin/kubernetes-bootcamp@sha256:0d6b8ee63bb57c5f5b6156f446b3bc3b3c143d233037f3a2f00e279c8fcc64af
    Port:           8080/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Mon, 16 Nov 2020 01:50:40 +0000
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-j6m67 (ro)
Conditions:
  Type              Status
  Initialized       True
  Ready             True
  ContainersReady   True
  PodScheduled      True
Volumes:
  default-token-j6m67:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  default-token-j6m67
    Optional:    false
QoS Class:       BestEffort
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
Events:
  Type     Reason            Age                From               Message
  ----     ------            ----               ----               -------
  Warning  FailedScheduling  17m (x2 over 17m)  default-scheduler  0/1 nodes are available: 1 node(s) had taints that the pod didn't tolerate.
  Normal   Scheduled         16m                default-scheduler  Successfully assigned default/kubernetes-bootcamp-765bf4c7b4-2m9p4 to minikube
  Normal   Pulled            16m                kubelet, minikube  Container image "gcr.io/google-samples/kubernetes-bootcamp:v1" already present on machine
  Normal   Created           16m                kubelet, minikube  Created container kubernetes-bootcamp
  Normal   Started           16m                kubelet, minikube  Started container kubernetes-bootcamp

# 변경한 label로 파드 검색
$ kubectl get pods -l app=v1
NAME                                   READY   STATUS    RESTARTS   AGE
kubernetes-bootcamp-765bf4c7b4-2m9p4   1/1     Running   0          18m

# Step 3. Deleting a service

# 명령어를 이용한 service 삭제
$ kubectl delete service -l run=kubernetes-bootcamp
service "kubernetes-bootcamp" deleted

# 서비스 삭제 확인
$ kubectl get services
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   20m

# 라우트가 외부로 노출되고 있는지 확인
$ curl $(minikube ip):$NODE_PORT
curl: (7) Failed to connect to 172.17.0.51 port 30751: Connection refused

# 어플리케이션이 아직 파드 안에서 작동하는지 확인
$ kubectl exec -ti $POD_NAME curl localhost:8080
Hello Kubernetes bootcamp! | Running on: kubernetes-bootcamp-765bf4c7b4-2m9p4 | v=1

```

# 5. 앱 스케일링하기

[복수의 앱 인스턴스를 구동하기](https://kubernetes.io/ko/docs/tutorials/kubernetes-basics/scale/scale-intro/)

## 5. 복수의 앱 인스턴스를 구동하기

### 목표

- kubectl을 사용하여 애플리케이션을 스케일한다.