# K8s 환경설정

## Instance 설정

### Spec

```
CPU 4
MEMORY 12GB
STORAGE 50GB
```

KubeFlow 내부에서는 위 스펙을 권장하나 학습 과정을 위해 Ncloud의 인스턴스를 활용

```
[Compact] 1vCPU, 2GB Mem, 50GB Disk [g1]
```

Compact 인스턴스이며 월 26,000원 정도 예상

### ACG 설정

openssh 연결을 위한 포트 개방 (단, 외부포트를 기준으로 포트를 개방해야한다.


### 관리자 정보 확인

인스턴스를 생성했다면 가장 먼저 관리자 비밀번호를 확인해야한다.
네이버 클라우드 Server페이지로가서 아이디와 비밀번호를 알고 싶은 인스턴스를 체크한 후 상단에 "서버관리 및 설정 변경" 중 "관리자 비밀번호"를 선택한다.
이후 사전에 다운받았던 pem 파일을 입력하면 최초 생성시 발급되는 관리자 이름과 비밀번호를 확인할 수 있다.

### 접속

사전에 설정한 외부포트와 아이피를 바탕으로 openssh에서 접속한다. 다만 포트가 22가 아닌 새로운 포트임으로 아래 명령어를 사용해야한다.

```
ssh -l 관리자이름 -p 외부포트 아이피
```

이후 password를 물어보면 사전에 저장해두었던 password를 입력하면 접속이 완료된다.

## 환경 설정

### K3s 설치

```
mkdir kubeflow
cd kubeflow
```

### Docker 설치

```
sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

apt update 및 https 허용 설정

```
sudo mkdir -m 0755 -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

GPG 설정

### Docker Engine 설치

```
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### K3s 설치

```
curl -sfL https://get.k3s.io | sh -s - --docker
```

K3s 설치하기

```
sudo cat /etc/rancher/k3s/k3s.yaml
```

K3s 설치여부 확인하기

### helm 설치

```
wget https://get.helm.sh/helm-v3.7.1-linux-amd64.tar.gz
tar -zxvf helm-v3.7.1-linux-amd64.tar.gz
sudo mv linux-amd64/helm /usr/local/bin/helm
helm help
```

### Kustomize 설치

```
wget https://github.com/kubernetes-sigs/kustomize/releases/download/kustomize%2Fv3.10.0/kustomize_v3.10.0_linux_amd64.tar.gz
tar -zxvf kustomize_v3.10.0_linux_amd64.tar.gz
sudo mv kustomize /usr/local/bin/kustomize
kustomize help
```
