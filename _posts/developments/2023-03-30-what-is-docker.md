---
title: 도커 란?
excerpt: What is Docker?
categories: development
tags: docker
---

# Overview

![overview](/assets/images/architecture.svg)

## 개념

도커란, 리눅스 컨테이너 애플리케이션으로 프로세스 격리기술을 사용하여 더 쉽게 컨테이너를 실행하고 관리할 수 있게 해주는 오픈소스 프로젝트이다.

## VM vs Docker Container

![vm-vs-docker](/assets/images/docker%20vs%20vm.png)

## VM(Virtual Machine)

가상 머신은 하이퍼바이저(Hypervisor)를 통해 여러개의 운영체제를 하나의 기기에서 사용할 수 있게 해주는 방식

### 문제점

-   다른 OS가 필요할 때 독립된 가상 공간에 OS전체가 설치되기 때문에 성능 손실이 크다.
    -   cpu, ram등의 리소스 또한 할당되어 고정되기 때문에 사용하지 않을 때에도 공간을 잡아 먹고 있게 된다.
-   컴퓨터의 리소스 활용이 유연하지 못함
-   배포 시 용량이 크다 → 배포 시간이 오래 걸린다.

## Docker Container

도커 컨테이너는 가상화된 공간을 생성한다. 단, 가상머신과 다른 점은 독립된 공간이 아닌 격리된 공간을 생성한다. OS 자체를 설치하는 것이 아니고 커널을 공유하여 사용하기 때문에 성능 손실이 적다.

\*도커는 리눅스의 chroot, namespace, cgorup 기능을 기반으로 구동된다.

### 장점

-   OS자체가 아닌 커널을 공유하는 방식으로 성능 손실이 적음
-   커널을 공유하기 때문에 필요한 파일 및 프로그램만 존재하기 때문에 용량이 작다.
    -   용량이 작다 → 배포시간이 빠르다.

### 단점

-   사용자가 VM처럼 용도에 맞지 않게 사용할 경우, 용량이 커지면서 배포가 오래 걸린다.
    -   But, docker는 builder cache가 존재하고 Layer 개념이 존재 하기 때문에 컨테이너 혹은 이미지가 크더라도 배포 속도를 비슷하게 유지할 수 있음.
-   최적화를 못할 경우, 누적되는 캐시 용량이 너무 커진다.
    -   builder 및 더 이상 사용되지 않는 캐시, <none> image 관리를 잘해야 한다.

## Container VS VM

일상 생황에 비유 해서 생각해보면 쉽습니다.

심플하게 가구로 비교 해볼 경우…

의자를 구매 한다고 가정 하여

-   container개념은 의자의 구성품들을 조릾식으로 만들어서 생산
-   VM은 일체형으로 만들어서 생산

조립식의 장점

-   만드는 공정 분리 가능
    -   오래 걸리는 공정은 더 많은 라인을 설치하여 다른 부품들과의 시간 격차를 줄일 수 있음
-   제품 유지보수의 용이성, 고장난 부품만 쉽게 교체 가능

반대로 일체형은

-   만드는 공정 분리 불가능 오래 걸리는 부품을 기다려야 함
-   유지보수 어려움, 제품 전체 교체가 비용이 더 적을 수 있음.

## 지원하는 OS

**Linux**

-   도커를 사용하기 가장 최적화된 OS이다. 애초에 Linux의 존재하는 기능을 기반으로 docker환경을 구성하기 때문이다.

**Windows**

-   윈도우즈 OS에서 docker를 사용하기 위해서는 Hypervisor기반의 WSL 설치가 필수이므로, 생각한 것 보다 성능 및 저장공간 효율이 나오지 않을 수 있다.
    -   잡아 먹는 리소스를 줄일 수 있긴 하나, 설정이 복잡하다.

**Mac OS**

-   mac os의 커널은 unix 기반이기 때문에 docker에 기반이 되는 chroot, namespace, cgroup 기능을 사용할 수 없다.
-   그래서 mac 또한 별도의 리눅스 가상 공간을 생성하여 사용한다.

![docker-container](/assets/images/docker-container.png)
\*Linux OS환경에서도 Docker Engine이 아닌 Docker Desktop을 설치하게 되면 Mac과 Windows와 마찬가지로 가상 OS가 별도 생성 됩니다.

# Install

[https://www.docker.com/](https://www.docker.com/)

## Docker Desktop

**Install on Mac**

[https://docs.docker.com/desktop/install/mac-install/](https://docs.docker.com/desktop/install/mac-install/)

Homebrew를 이용하여 설치 가능

```bash
# *중요: --cask옵션으로 설치해야 docker desktop이 설치됩니다.
# --cask옵션이 없을 경우 docker engine만 설치됩니다.
# docker engine만 설치할 경우 docker compose를 별도로 설치해야하며 네트워크 관련 설정도 직접 해야 합니다.
brew install --cask docker
```

**Install on Windows**

[https://docs.docker.com/desktop/install/windows-install/](https://docs.docker.com/desktop/install/windows-install/)

1. WSL 설치
2. Docker Desktop 설치

**Install on Linux**

[https://docs.docker.com/desktop/install/linux-install/](https://docs.docker.com/desktop/install/linux-install/)

Ubuntu

-   저장소 추가

```bash
# 1. Update apt and requirement package install
sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
# 2. Add Docker official GPG key
sudo mkdir -m 0755 -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

# 3. Use the following command to set up the repository
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

-   Docker Desktop 설치

```bash
sudo apt-get update
curl -O https://desktop.docker.com/linux/main/amd64/docker-desktop-4.17.0-amd64.deb?utm_source=docker&utm_medium=webreferral&utm_campaign=docs-driven-download-linux-amd64
sudo apt-get install ./docker-desktop-<version>-<arch>.deb
```
