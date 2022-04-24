---
title : "Oracle 19c를 Ubuntu 20.04에 설치하기"
excerpt: "Oracle 19c를 Ubuntu 20.04에 설치하는 방법에 대해 알아본다."
categories:
- Setting
tags:
- [Oracle]
- [Setting]
date: 2022-04-23 18:00:00
---
# Oracle19c의 Ubuntu설치
필요한 도구 : Ubuntu 20.04가 설치된 컴퓨터, Git의 설치, 20GB이상의 저장공간, 좋은 인터넷, 우분투 터미널의 root 권한, Docker의 설치

## Docker 설치
HTTPS를 통해 레포지토리를 이용하기 위해 패키지를 설치해준다.
``` 
$ sudo apt-get update

$ sudo apt-get -y install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
 ```
 Docker의 Official GPG Key 를 등록한다.

 ```
 $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```
Docker Engine을 설치한다.
```
$ sudo apt-get update
$ sudo apt-get install docker-ce docker-ce-cli containerd.io
```
설치가 완료되면 `docker --version` 을 실행해서 버전을 확인한다.

## Docker Compose 설치
Docker Compose는 여러개의 도커 어플리케이션 컨테이너를 정의하고 실행할 수 있게 도와주는 툴이다. 
```bash
$ sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
실행할 수 있는 권한을 부여한다.
```bash
$ sudo chmod +x /usr/local/bin/docker-compose
```
설치가 제대로 되었는지 확인한다.
```
$ docker-compose --version
```
## Docker Oracle-image 다운로드
```
$ mkdir /var/docker 
$ cd /var/docker
$ git clone https://github.com/oracle/docker-images
```
`/var/docker` 경로에 폴더를 만든 뒤, 해당 폴더로 이동하여 `oracle docker-images`를 다운 받는다.
## Oracle 다운로드
[Oracle 다운로드 사이트](https://www.oracle.com/database/technologies/oracle-database-software-downloads.html#19c)에 접속하여 Oracle 19c를 ZIP로 다운받는다. RPM파일의 경우
Ubuntu 운영체제에서 사용할 수 없다.
![image](https://user-images.githubusercontent.com/65166786/164979804-fab8fa2d-4908-470c-80fa-871fa92050b1.png)
다운로드한 ZIP파일을 Docker image 내로 복사한다.
```
$ cp Downloads/LINUX.X64_193000_db_home.zip /var/docker/docker-images/OracleDatabase/SingleInstance/dockerfiles/19.3.0/
```
## Docker 실행
`dockerfiles` 폴더로 이동한 뒤 `buildContainerImage.sh`를 실행해 Docker를 실행한다.
Docker의 실행에는 사용자의 환경에 따라 시간이 좀 걸린다.
```
$ cd /var/docker/docker-images/OracleDatabase/SingleInstance/dockerfiles
$ ./buildContainerImage.sh -v 19.3.0 -e
```
실행이 완료되면 `"Build completed in xxx seconds."`이라는 메시지를 볼 수 있다.
`docker images`를 실행하여,  `19.3.0-ee`이라는 이미지가 존재하는지를 확인해본다.
```
$ docker images
```
![image](https://user-images.githubusercontent.com/65166786/164979773-68f38a51-992f-4542-8acb-cd388b00c49e.png)
## Docker image 실행(oracle 실행)
Docker image를 실행한다.
```
docker run --name oracle19c --network host -p 1521:1521 -p 5500:5500 -v /opt/oracle:/u01/oracle oracle/database:19.3.0-ee
```
 실행 시 출력되는 텍스트의 첫번째 줄에서 패스워드를 볼 수 있다. 찾지 못해도 괜찮다.
 패스워드는 따로 설정 가능하다.
```
ORACLE PASSWORD FOR SYS, SYSTEM AND PDBADMIN: ET10BpTqpVg=1  
```
이미지의 실행에도 사용자의 환경에 따라 시간이 좀 걸린다.
실행이 완료되면, `DATABASE IS READY TO USE`라는 문구를 볼 수 있다.
또한 마지막 줄은 `XDB initialized`이여야 한다.

ctrl + c를 눌러 실행을 종료하고 생성된 이미지들을 확인해본다.
```
docker ps -a
```
![](https://user-images.githubusercontent.com/65166786/164980316-f9bfb05b-2180-4c24-b2f7-73b7ea208e15.png)
`database:19.3.0-ee` 이미지의 이름이 `oracle19c`인것을 확인한다.
`oracle19c` 이미지를 실행한다.(Oracle Database의 실행)
```
docker start oracle19c
```
1분정도 지난 뒤 https://localhost:5500/em/shell 에 접속 시 Oracle Database를 확인 할 수 있다.
![image](https://user-images.githubusercontent.com/65166786/164980577-7e6ef634-803c-4ca6-8b24-c34df5cf3870.png)
이렇게 로그인 창이 뜬다면 
Username : `system` 
Password :  이전에 출력되었던 패스워드, 못 보았을 경우 하단의 코드로 패스워드를
`oracle`로 수정한다.
```
docker exec oracle19c ./setPassword.sh oracle
```
Container : `orclpdb1`
으로 입력하여 로그인 한다.

로그인하면 이런 화면이 보인다
![image](https://user-images.githubusercontent.com/65166786/164980842-1ce19be8-ebb5-4a09-8096-26063ae8e766.png)

만약 터미널에서 Docker database에 연결하고 싶다면 하단의 코드를 입력한다.
```
docker exec -ti oracle19c sqlplus system/oracle@orclpdb1
```
