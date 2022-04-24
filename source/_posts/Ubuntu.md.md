---
title: "Ubuntu 20.04 설치하기"
excerpt: "Ubuntu 20.04를 듀얼 부팅으로 설치하는 방법을 알아본다."
categories:
- Setting
tags:
- [Setting]
- [Ubuntu]
date: 2022-04-12 20:00:00
---
﻿# Ubuntu 20.04 듀얼 부팅으로 설치하기

## 설치 전 해야할 일
우분투를 듀얼 부팅으로 설치하기 위해서는 부팅용 USB가 필요하다.
가급적 4GB 이상의 USB를 권장한다. 부팅용으로 만드는 과정에서 포맷이 된다는 점에 주의한다. 

## 부팅 디스크 만들기
https://ubuntu.com/#download
우분투 공식사이트에 접속하여 iso파일을 다운로드 한다.
https://rufus.ie/ko/ 
다운로드한 iso를 부팅 USB로 만들기 위해 rufus를 다운로드 한다.
![[출처 : rufus 공식 홈페이지 ]](https://rufus.ie/pics/rufus_ko.png)
부팅 디스크를 만들 usb와 iso를 선택한 뒤 시작 버튼을 눌러주면 자동으로 부팅 디스크가 생성된다.
## 파티션 분할하기
키보드의 윈도우버튼(시작버튼)을 누른 뒤, **하드디스크 파티션 만들기 및 포맷**을 검색한다. 
![https://ccm3.net/archives/23854](http://ccm3.net/wp-content/uploads/04-137.png)
설치를 원하는 파티션에서 우클릭을 눌러, 불륨 축소를 선택하여 파티션을 분할한다.
 ![https://ccm3.net/archives/23854](http://ccm3.net/wp-content/uploads/05-117.png)
축소할 공간 입력에 MB(메가바이트)단위로 새로 만들 파티션의 크기를 입력 한 후 축소를 클릭해준다.
![https://ccm3.net/archives/23854](http://ccm3.net/wp-content/uploads/05_6.png)
입력한 크기 만큼의 공간이 새로 만들어지게 된다.
## BIOS 설정
https://blog.naver.com/tmdcjfdl3/221366662549
상단의 링크를 참조하여 바이오스에 진입해준다.
그 후 바이오스 설정에서 boot~ 관련 메뉴로 진입하여 
USB를 최상단으로 이동한 뒤 save 및 exit를 해준다.
이 부분은 바이오스마다 다르므로 각자 찾아보길 바란다.

## 우분투 설치
바이오스를 설정하고, 부팅 USB를 꽂은 다음 부팅을 하면
로딩 후에, Ubuntu로고가 뜨며, 설치 화면이 뜰 것이다.!![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F23425D3E596746D220)
해당 화면이 뜨면 체험하기가 아닌 설치를 눌러준다.
![](https://blog.kakaocdn.net/dn/42Zhk/btqxYVs3ynL/K4Kp5hjMNGbcI8fARA3Gh0/img.jpg)
일반 설치와 최소 설치 중 자신이 원하는 옵션을 선택한다.
게임이나 미디어 플레이어 등을 필요로 하지 않기에 최소 설치로 선택하는 편이다. 
기타 설정의 체크 항목은 둘 다 체크 해주는 것이 좋다.

![https://jimnong.tistory.com/676](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F24250F4B59674A3E08)
앞서 윈도우에서 분할한 별도의 파티션에 윈도우를 설치 할 예정이기 때문에 기타를 선택한다.
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F9987C84F5CAD8F6412)

먼저 스왑 파티션을 생성한다. [스왑 파티션이란?](https://sergeswin.com/1034/)
하단의 남은 공간을 클릭한 후 + 를 클릭하면 대화창이 뜬다. 
용도는 스왑 영역, 파티션 종류는 논리 파티션, 파티션 위치는 이 공간이 시작하는 지점, 크기는 RAM이 2GB 미만일 경우 RAM의 2배, 2~8GB일 경우 RAM과 동일, RAM이 8GB 초과일 경우 최소 4GB 이상으로 설정 한다.
그리고 OK버튼을 누르면 스왑 파티션 생성이 완료된다.

![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F2408D63D596753C427)
이제 루트 파티션을 생성한다. 크기를 남은 공간 전체로 세팅하고, 파티션의 종류는 논리 파티션, 파티션의 위치는 이 공간이 시작하는 지점, 용도는 ext4, 마운트 위치는 " / " 로 지정한다. 그리고 OK버튼을 누르면 루트 파티션 생성이 완료된다.
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F257C03455967559F23)
파티션 생성이 완료 되었으면 루트 파티션을 클릭한 후, 지금 설치를 클릭하면 되는데, 이때 중요한 점이 부트로더의 위치이다. 여러개의 하드디스크를 사용하는 경우 가끔 오류로 부트로더가 다른 위치에 잡히는 경우가 있는데, 꼭 **윈도우가 설치된** 하드디스크의 최상단에 부트로더를 설치해야 부팅 실패를 겪지 않는다.

## 나머지 설치
지도 그림이 나올 경우 영어로 서울을 입력하거나 대한민국을 마우스로 찍는다.

키보드 레이아웃의 경우 "한국어 101/104키"가 아닌 그냥 "한국어"를 선택해준다.

우분투 로그인 계정을 설정한다. 사용자 이름과 암호의 경우 많은 곳에서 쓰이기 때문에 잊어버리지 않게 주의한다.

## 재부팅 
![https://jimnong.tistory.com/676](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F2751853C5967614D2C)
재부팅 후 모니터 상에 GRUB가 뜬다면 우분투가 제대로 설치 된 것이다.
↑ 키와 ↓키를 이용하여 우분투와 윈도우가 제대로 작동하는지 확인해 보길 바란다.








