# AWS EKS 시작하기

---

## 목표

해당 기술 문서는 AWS EKS Cluster구축 및 운영을 위한 기초 자료를 작성하기 위하여 AWS EKS학습서를 정리한 내용이다.

---

## EKS란

Kubernetes 제어 플레인(control plane) 또는 노드를 설치, 작동 및 유지 관리할 필요 없이 AWS에서 Kubernetes를 손쉽게 실행할 수 있게 해주는 관리형 서비스 입니다.

EKS는 여러 가용 영역에서 Kubernetes제어 플레인 인스턴스를 실행하여 고가용성을 보장합니다.

EKS는 비정상 제어 플레인 인스턴스를 자동으로 감지하고 교체하며, 자동화된 버전 업그레이드 및 패치를 제공합니다.

EKS는 여러 AWS 서비스와 통합됩니다.

- 컨테이너 이미지용 ECR
- 로드 배포용 Elastic Load Balancing
- 인증용 IAM
- 격리용 VPC

EKS는 Kubernetes 커뮤니티에서 기존 플러그인과 도구를 모두 사용할 수 있습니다.
온프레미스 데이터 센터에서 실행중인지 퍼블릭 클라우드에서 실행중인지 상관없이, EKS에서 실행중인 애플리케이션은 표준 Kubernetes 환경에서 실행 중인 애플리케이션과 완벽하게 호환됩니다.
코드를 수정하지 않고 표준 Kubernetes 애플리케이션을 EKS로 쉽게 마이그레이션 할수 있습니다.

---

## EKS Control Plane 아키텍쳐

Amazon EKS는 각 클러스터에 대해 단일 테넌트 Kubernetes 제어 플레인을 실행하며, 제어 플레인 인프라는 클러스터 또는 AWS 계정에서 공유되지 않습니다.
이 제어 플레인은 리전 내 세 개의 가용 영역에서 실행되는 세 개의 etcd 노드와 두 개 이상의 API 서버 노드로 구성됩니다. Amazon EKS에서는 비정상 제어 플레인 인스턴스를 자동으로 감지하여 교체하고, 필요한 경우 리전의 가용 영역에서 해당 인스턴스를 재시작합니다. Amazon EKS는 고가용성을 유지하기 위해 AWS 리전의 아키텍처를 활용합니다. 따라서 Amazon EKS는 API 서버 엔드포인트 가용성을 위한 SLA를 제공할 수 있습니다.
Amazon EKS는 Amazon VPC 네트워크 정책을 사용하여 제어 플레인 구성 요소 간의 트래픽을 단일 클러스터 내로 제한합니다. 클러스터에 대한 제어 플레인 구성 요소는 Kubernetes RBAC 정책에 따라 권한을 부여받지 않은 경우 다른 클러스터 또는 다른 AWS 계정의 통신을 보거나 수신할 수 없습니다.
이러한 안전하고 가용성이 높은 구성으로 인해 Amazon EKS가 안정적으로 되며 프로덕션 워크로드용으로 권장됩니다.

---

## EKS 작동 방식

### 1. EKS 손쉽게 시작하기

   1.1. AWS Management Console 또는 AWS SDK들을 사용하여 EKS Cluster를 생성한다.

   1.2. Amazone EKS cluster와 함께 등록하는 관리형, 자체 관리형 노드를 시작합니다.
          Amazon EKS 노드를 자동으로 구성하는 AWS CloudFormation 템플릿이 제공됩니다
          만약 노드를 관리할 필요가 없는 경우는 AWS Fargete에 애플리케이션을 배포할 수 있습니다.

   1.3. 클러스터가 준비되었다면, 너는 Kubernetes도구(kubectl)를 통해서 클러스터와 통신할 수 있습니다.

   1.4. 다른 Kubernetes환경에서와 같이 EKS 클러스터에 애플리케이션을 배포 및 관리가 가능합니다.

---

## EKS 시작하기 - Kubernetes 클러스터 구성 방법 2가지

EKS 에서 노드가 Kubernetes 클러스터를 생성하는 두가지 방법은 아래와 같다.

### 1. ekctl로 구성하기

이 시작 안내서는 EKS에서 Kubernetes 클러스터를 생성 및 관리하기 위한 간단한 명령어 유틸리티인 eksctl을 사용하여 EKS를 시작할 때 필요한 모든 리소스를 설치할수 있다.
자습서를 마치면 애플리케이션을 배포할 수 있는 EKS 클러스터를 실행하게 됩니다. 이것이 EKS를 시작하는 가장 빠르고 쉬운 방법입니다.

### 2. AWS Mangement 콘솔로 구성하기

Amazon EKS를 사용하여 AWS Management 콘솔를 시작할 때 필요한 모든 리소스를 만드는 데 도움이 됩니다.
이 자습서를 마치면 애플리케이션을 배포할 수 있는 실행 중인 Amazon EKS 클러스터가 생깁니다.
이 안내서에서는 Amazon EKS 또는 AWS CloudFormation 콘솔에서 수동으로 각 리소스를 만듭니다.
이 절차를 통해서 각 리소스가 만들어 지는 방식과 리소스가 작용하는 방식을 완벽하게 파악 할 수 있다.

---

## eksctl 시작하기

이 시작 안내서는 Amazon EKS에서 Kubernetes 클러스터를 생성 및 관리하기 위한 간단한 명령줄 유틸리티인 eksctl를 사용하여 Amazon EKS를 시작할 때 필요한 모든 리소스를 생성하는 데 도움이 됩니다. 이 자습서를 마치면 애플리케이션을 배포할 수 있는 Amazon EKS 클러스터를 실행하게 됩니다.

### 1. Prerequisties

이 단원은 Amazon EKS 클러스터를 생성하고 관리하는 데 필요한 다음 도구를 설치하고 구성하는 데 도움이 됩니다.

- AWS CLI – EKS를 포함한 서비스 작업을 위한 명령줄 도구
- eksctl – 많은 개별 작업을 자동화하는 EKS 클러스터 작업을 위한 명령줄 도구
- kubectl – Kubernetes 클러스터 작업을 위한 명령줄 도구입니다.

### 2. Linux용 AWS CLI를 설치하려면

2.1. 현재 AWS CLI가 설치되어 있는 경우 설치한 버전을 확인합니다.
     aws –version

2.2. 버전 1.18.163 이상 또는 버전 2.0.59 이상이 설치되어 있지 않으면 AWS CLI 버전 2를 설치합니다. 다른 설치 옵션을 확인하거나 현재 설치된 버전 2를 업그레이드하려면 Linux에서 AWS CLI 버전 2 업그레이드를 참조하십시오.

curl "[https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip](https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip)" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

AWS CLI 버전 2를 사용할 수 없는 경우 다음 명령을 사용하여 AWS CLI 버전 1의 최신 버전이 설치되어 있는지 확인합니다.

pip3 install --upgrade --user awscli

### 3. AWS CLI 자격 증명 구성

eksctl과 AWS CLI 모두 사용자 환경에 AWS 자격 증명이 구성되어 있어야 합니다.

$ aws configure
AWS Access Key ID [None]: <AKIAIOSFODNN7EXAMPLE>
AWS Secret Access Key [None]: <wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY>
Default region name [None]: <region-code>
Default output format [None]: <json>

### 4. eksctl 설치

eksctl를 사용하여 Linux에서 curl를 설치하거나 업그레이드하려면

4.1. 다음 명령으로 eksctl 최신 릴리스를 다운로드하여 압축 해제합니다.

 curl --silent --location "[https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$](https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$)(uname -s)_amd64.tar.gz" | tar xz -C /tmp

4.2. 압축 해제된 이진 파일을 /usr/local/bin으로 옮깁니다.
sudo mv /tmp/eksctl /usr/local/bin

4.3. 다음 명령으로 설치가 제대로 되었는지 테스트합니다.
eksctl version

---

## kubectl 설치 및 구성

Kubernetes는 클러스터 API 서버와 통신하기 위한 kubectl 명령줄 유틸리티를 사용합니다.

### Linux에 kubectl를 설치하려면

1. 클러스터가 있는 리전에 해당하는 EKS판매 kubectl 이진 파일을 다운로드 합니다.
curl -o kubectl [https://amazon-eks.s3.us-west-2.amazonaws.com/1.18.8/2020-09-18/bin/linux/amd64/kubectl](https://amazon-eks.s3.us-west-2.amazonaws.com/1.18.8/2020-09-18/bin/linux/amd64/kubectl)
2. 바이너리에 실행 권한을 적용합니다.
chmod +x ./kubectl
3. kubectl를 경로에 있는 폴더로 이동합니다.
sudo mv ./kubectl /usr/local/bin
4. (선택 사항) 셸 초기화 파일에 $HOME/bin 경로를 추가하면 셸을 열 때 구성됩니다.
echo 'export PATH=$PATH:$HOME/bin' >> ~/.bash_profile
5. kubectl을 설치한 이후 다음 명령을 사용하여 버전을 확인할 수 있습니다.
kubectl version --short --client

---

## EKS 클러스터 및 컴퓨팅 생성

애플리케이션을 실행하는 컴퓨팅 옵션으로 Amazon EKS 클러스터를 생성하는 방법을 설명합니다.

다음 컴퓨팅 옵션 중 하나를 선택합니다.

각 옵션에 대한 내용은 Amazon EKS컴퓨팅 단원을 참조하십시요.
클러스터를 배포한 후 원하는 경우 다른 옵션을 추가할 수 있습니다.

### 1. Fargate

Fargate에서 Linux 애플리케이션을 실행하려면 이 옵션을 선택합니다.

### 2. 관리형 노드

EC2인스턴스에서 Amazon Linux 애플리케이션을 실행하려면 이 옵션을 선택합니다.

### 3. 자체 관리형 노드

Windows 애플리케이션에서 이 옵션을 실행하려면 이 옵션을 선택합니다.
EC2 Windows 애플리케이션만 실행해야 하더라도 클러스터에 Linux 노드가 하나 이상 있어야 하기 때문에 이 옵션을 사용하여 Linux 애플리케이션을 실행할 수도 있습니다.

### 4. Fargate - Linux 구성

다음 명령을 사용하여 Fargate가 지원되는 EKS 클러스터를 생성할수 있다.

eksctl create cluster \
--name <my-cluster> \
--version <1.18> \
--region <us-west-2> \
--fargate

새 EKS 클러스터는 노드 그룹 없이 생성됩니다.
eksctl은 pod 실행 Role를 생성하고, default 그리고 kube-system 네임스페이스에 대한 Forgate Profile은 coredns에서 실행할 수 있도록 Fargate배포를 패치합니다.

### 5. 관리형 노드 - Linux 구성

launch template(시작템플릿)을 사용하지 않고 Node들을 만들수 있다.
launch template(시작템플릿)을 사용하면 사용자 지정 AMI를 배포할 수 있는 기능을 포함하여 보다 세부적인 사용자 지정을 수행할 수 있습니다.

다음 명령을 사용하여 시작 템플릿 EKS없이 클러스터와 Linux노드를 생성합니다.

예)

eksctl create cluster \
--name <my-cluster> \
--version <1.18> \
--region <us-west-2> \
--nodegroup-name <linux-nodes> \
--nodes <3> \
--nodes-min <1> \
--nodes-max <4> \
--ssh-access \
--ssh-public-key <name-of-ec2-keypair> \
--managed

실행명령)

eksctl create cluster \
--name skkim-cluster \
--version 1.17 \
--region ap-northeast-2 \
--nodegroup-name linux-nodes \
--nodes 3 \
--nodes-min 1 \
--nodes-max 4 \
--ssh-access \
--ssh-public-key KEY_Megashop \
--managed

-ssh-public-key는 선택 사항이지만 클러스터를 사용하여 노드 그룹을 생성할 때 지정하는 것이 좋습니다. 이 옵션은 관리형 노드 그룹의 노드에 SSH 액세스를 활성화합니다.
SSH 액세스를 활성화하면 문제가 있는 경우 인스턴스에 연결하여 진단 정보를 수집할 수 있습니다. 노드 그룹을 생성한 후에는 원격 액세스를 활성화할 수 없습니다.

launch template(시작템플릿)을 사용하여 EKS클러스터 및 Linux Node를 생성합니다.
launch template(시작템플릿)을 사전에 준비되어야 하며, 시작템플릿 구성 기본 사항에 지정된 요구 사항을 충족해야 합니다.
다음 콘텐츠가 포함된 <cluster-node-group-lt.yaml> 파일을 만들어 예제 <values>를 사용자의 값으로 바꿉니다.
시작 템플릿 없이 배포할 때 지정하는 몇 가지 설정은 시작 템플릿으로 이동됩니다. 버전을 지정하지 않으면 템플릿의 기본 버전이 사용됩니다.

---

apiVersion: [eksctl.io/v1alpha5](http://eksctl.io/v1alpha5)
kind: ClusterConfig

metadata:
name: <my-cluster>
region: <us-west-2>
version: '<1.18>'
iam:
withOIDC: true
managedNodeGroups:

- name: <ng-linux>
   launchTemplate:
   id: lt-<id>
   version: "<1>“

다음 명령을 사용하여 클러스터와 노드 그룹을 생성합니다.
eksctl create cluster --config-file <cluster-node-group-lt>.yaml

클러스터와 노드가 생성되면 아래와 같이 출력이 됩니다. 출력의 마지막 줄은 다음 예제와 유사합니다.
[✓] EKS cluster "<my-cluster>" in "<us-west-2>" region is ready

---

### 6. 자체 관리형 노드 - Windows

6.1. 아래 텍스트를 cluster-spec.yaml 이라는 파일에 저장합니다.
구성 파일은 자체 관리형 windows 노드 그룹 및 관리형 Linux 노드 그룹이 있는 클러스터를 생성하는 데 사용됩니다.
클러스터에서 Windows 애플리케이션만 실행하려는 경우에도 모든 EKS클러스터에는 하나 이상의 Linux노드가 포함되어야 하지만, 가용성을 위해 최소 두개의 Linux노드를 생성하는 것이 좋습니다.

---

apiVersion: [eksctl.io/v1alpha5](http://eksctl.io/v1alpha5)
kind: ClusterConfig

metadata:
name: <my-cluster>
region: <<us-west-2>
version: '<1.18>'
managedNodeGroups:

- name: <linux-ng>
   instanceType: <m5.large>
   minSize: <2>

   nodeGroups:

- name: <windows-ng>
  instanceType: <m5.large>
   minSize: <2>
   volumeSize: <100>
   amiFamily: <WindowsServer2019FullContainer>

6.2. 다음 명령을 사용하여 EKS 클러스터와 Windows 및 Linux 노드를 생성합니다.

eksctl create cluster -f cluster-spec.yaml --install-vpc-controllers

6.3. 결과 화면
클러스터와 노드가 생성되면 여러 줄의 출력이 표시됩니다. 출력의 마지막 줄은 다음 예제와 유사합니다.

[✓] EKS cluster "<my-cluster>" in "<us-west-2>" region is ready

클러스터 프로비저닝은 일반적으로 10~15분이 소요됩니다.

6.3. 클러스터가 준비되면 kubectl 구성이 올바른지 테스트 하십시요

kubectl get svc

6.4. 결과 화면
NAME                  TYPE          CLUSTER-IP     EXTERNAL-IP      PORT(S)     AGE
svc/kubernetes    ClusterIP   10.100.0.1         <none>             443/TCP     1m

6.5. (Linux 가속 AMI노드에만 해당) 가속 AMI 인스턴스 유형과 EKS 최적화 가속 AMI를 선택한 경우
다음 명령을 사용하여 클러스터에 kubernetes용 NVIDIA 디바이스 플러그인 을 DaemonSet로 적용해야 합니다.
kubectl apply -f [https://raw.githubusercontent.com/NVIDIA/k8s-device-plugin/v0.6.0/nvidia-device-plugin.yml](https://raw.githubusercontent.com/NVIDIA/k8s-device-plugin/v0.6.0/nvidia-device-plugin.yml)

노드가 있는 EKS클러스터가 작동 중이므로 Kubernetes 추가 기능을 설치하고 클러스터에 애플리케이션을 배포함 준비가 되었습니다.

---

### 7. AWS 콘솔 시작하기

해당 단원은 추후 정리한다.


**END**