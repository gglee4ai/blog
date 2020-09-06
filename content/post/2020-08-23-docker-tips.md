---
title: Docker 팁 정리
author: 'gglee'
date: '2020-08-23'
slug: docker-tips
categories:
  - Web
tags: 
  - docker
  - tips
draft: no
---

쓸수록 매력있는 것이 도커 인 것 같다. 처음에 세팅과 개념이 어려워서 그렇지 적절한 명령어로 조합을 하고 나면, 리눅스 서버의 능력을 최고로 사용할 수 있는 것 같다.

하지만, 일단 설정하고 나면, 자주 안쓰기 때문에 까먹기 쉽다. 일반적으로 많이 사용하는 도커 관련 기본 명령어를 간단히 정리해 보고, 실제 내가 사용하는 gpu 서버상의 도커 명령어도 간단하게 정리해 보았다.

## 1 기초 사용법

### 1.1 도커 이미지 검색

```bash
docker search nginx  
```

보통 docker hub에서 이미지 확인을 하는 게 보통이나, 터미널 상에서 확인이 필요할 경우 사용한다.

### 1.2 도커 이미지 받기

```bash
docker pull nginx  # nginx:latest 가져옴
```

허브상의 도커 이미지를 다운로드. 이미지의 다양한 옵션은 docker image tag를 확인해야 한다.

### 1.3 도커 이미지 목록 출력

```bash
docker images  
```

보유하고 있는 이미지 전체를 출력한다. 생각보다는 이미지 크기가 그리 작지 않은 것에 유의하자.

### 1.4 컨테이너 실행

```bash
docker run --name MY-CONTAINER -d -p 8080:80 nginx  
```

* `--name MY-CONTAINER`: 컨테이너의 이름을 지정
* `-d`: 백그라운드에서 컨테이너를 실행하면서 컨테이너 ID를 출력, 참고로 detach된 컨테이너는 `attach`가 의미 없음
* `-p 8080:80`: host의 8080 포트를 nginx 80 포트에 연결

브라우저에서 `localhost:8080` 실행하면, nginx 기본 홈페이지가 나온다.

### 1.5 컨테이너 정지

```bash
docker stop MY-CONTAINER  
```

작동중인 이미지를 정지한다. 실행을 일시적으로 멈추는 것으로 만들어진 이미지가 삭제되는 것은 아니다.

### 1.6 컨테이너 목록 확인

```bash
docker ps
```

현재 작동하고 있는 컨테이너를 보여준다.

### 1.7 전체 컨테이너 목록 확인

```bash
docker ps -a  # stop으로 정지된 컨테이너도 보임
```

실행 및 정지된 컨테이너 전체를 보여준다.

### 1.8 컨테이너 실행

```bash
docker start MY-CONTAINER
```

정지된 컨테이너를 다시 시작한다.

### 1.9 컨테이너 재실행

```bash
docker restart MY-CONTAINER
```

재부팅과 같이 완전히 컨테이너를 재시작한다.

### 1.10 컨테이너 안의 명령 실행

```bash
docker exec MY-CONTAINER /bin/bash
```

컨테이너 안의 명령어를 외부에서 직접 사용할 수 있다. 참고로 `attach`를 쓸 경우, 기존에 실행하고 있는 프로세스에 재접속하며 `ctrl-P, Q`로 빠저나올수 있다.

### 1.11 컨테이너의 log를 확인

```bash
docker logs MY-CONTAINER
```

실행한 컨테이너에서 발생한 로그를 확인한다.

### 1.12 컨테이너 삭제

```bash
docker rm MY-CONTAINER
```

정지된 컨테이너를 삭제한다. 실제로 많이 일어나는 상황이, 컨테이너를 같은 이름으로 두번 만들 때이다. 이때는 기존의 이름을 가진 컨테이너를 삭제해야 신규로 만들 수 있다.

## 2 실전 사용법

### 2.1 hugo로 만들어진 웹사이트 포함 nginx 실행

```bash
docker run --name MY-CONTAINER -d -p 80:80 -v ${HOME}/my-files/github/blog/public:/usr/share/nginx/html:ro nginx
```

### 2.2 gpu 전부 이용 tensorflow jupyter notebook 실행

```bash
docker run --name gpu_all --gpus all --restart=always --detach -v "${HOME}/my-files/tf/notebooks:/tf/notebooks" -p 8888:8888 tensorflow/tensorflow:latest-gpu-jupyter
```

* gpu 1개 만 이용시, `--gpus '"device=0"'` or `--gpus '"device=1"'`

### 2.3 현재 gpu 서버의 gpu 사용율 확인

```bash
docker run --rm --gpus all nvidia/cuda nvidia-smi
```

현재 gpu 사용율을 보여줌

### 2.4 jupyter notebook 토큰 확인

```bash
docker exec MY-CONTAINER jupyter notebook list
docker logs MY-CONTAINER  # 보통 log를 보면 토큰이 나와 있음
```

Jupyter notebook을 웹으로 접속하기 위해서는 token이 필요한 경우가 있다. 이때, 토큰은 jupyter notebook 의 list 명령 또는 컨테이너 생성 당시에 log를 보면 알 수 있다.
