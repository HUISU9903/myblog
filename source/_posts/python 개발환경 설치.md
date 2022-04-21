# 파이썬 개발 환경 설정하기

## Pycharm 설치하기
[Pycharm 공식 사이트](https://www.jetbrains.com/ko-kr/pycharm/download/#section=windows)에 방문하여 자신의 운영체제 및 컴퓨터 사양에 맞는 프로그램을  Community 버전으로 다운 받는다.
### Windows
설치 파일이 다운로드 되면 Next > Next > 체크 옵션에서 모두 체크 > Install > 재부팅을 거쳐 설치한다.
### Mac
다운로드 된 dmg파일을 실행한 후 Pycharm.app을 Applications으로 옮긴다.
그 후 Applications 폴더 내에서 설치 된 Pycharm을 확인한다.

### Ubuntu
터미널을 열고 하단의 코드를 순서대로 입력한다.
```
$ cd Downloads
$ tar xvf pycharm-community-2022.1.tar.gz
$ cd pycharm-community-2022.1
$ cd bin
$ sh pycharm.sh
```
파이참이 실행되면 상단 메뉴에서 Tools > Create Desktop Entry 를 선택하여 바탕화면 바로가기를 만들어준다. 

## 아나콘다 설치하기

[아나콘다 공식 사이트](https://www.anaconda.com/products/distribution)에 방문하여 자신의 운영체제 및 컴퓨터 사양에 맞는  프로그램을 다운 받는다.
### Windows
설치 파일이 다운로드 되면 Next > I Agree> **All users**> Next > **체크 옵션에서는 두 가지 모두 체크** > Install  >  Next > Next > Finish 순으로 설치한다.
### Mac
설치 파일이 다운로드 되면 Next를 계속 눌러 설치를 진행한다.
### Ubuntu
wget을 이용하여 프로그램을 다운받은 후 sh 명령어로 설치한다.
다운로드 링크에서 우클릭하여 링크 주소를 복사한다.
```
wget https://repo.anaconda.com/archive/Anaconda3-2021.11-Linux-x86_64.sh
sh Anaconda3-2021.11-Linux-x86_64.sh
```
라이센스 설명 enter , yes > 디렉토리 설치 enter > .bashrc yes 순으로 하면 설치가완료된다.

## VSCode 설치하기

[VSCode 공식 사이트](https://code.visualstudio.com/download)에 방문하여
자신의 운영체제 및 컴퓨터 사양에 맞는 프로그램을 다운 받는다.
### Windows
System Installer으로 다운 받는다.
Next > 체크 옵션에서는 다 체크 >  Next > Finish 로 설치를 완료한다.
### Mac
다운로드 된 파일의 압축을 해제한 후 Applications으로 복사해주면 설치가 완료된다. 
### Ubuntu
다운로드 사이트에서 .deb 파일을 다운로드 받거나, 
터미널을 열어서
```
sudo apt install code
```
을 입력해주면 된다.



