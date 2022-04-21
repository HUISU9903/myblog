---
title: "Ubuntu 20.04에 Spark 설치 및 환경 구성"
excerpt: "Ubuntu 20.04 버전에 Spark를 설치하는 방법을 알아본다."
categories:
- Setting
tags:
- [Setting]
- [Spark]
date: 2022-04-16 20:00:00
---
﻿# Ubuntu 20.04에 spark 설치하기

## 1. java 설치
spark는 java를 이용하기 때문에 spark를 설치하기 전에 java를 먼저 설치해야 한다.

java 버전을 확인 할 수 있는 명령어를 이용해서 java가 설치 되어 있는지를 확인한다.
```
$ java -version
```
java가 설치되어 있지 않은 경우 java를 설치 한다.
 ```
$ sudo apt-get install openjdk-11-jdk-headless
```

자바의 환경변수도 설정한다.  아래의 명령어를 통해 vi 편집기를 연다.
```
$ vi ~/.bashrc
``` 
그리고 아래의 경로를 붙여 넣는다.
```
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
```
## 2. spark 설치
스파크 공식 다운로드 사이트에 접속하여 원하는 버전의 링크를 복사한 뒤 wget로 파일을 다운 받는다.
```
$ wget https://downloads.apache.org/spark/spark-3.2.1/spark-3.2.1-bin-hadoop3.2.tgz
```
파일이 다운로드 되면 파일의 압축을 해제 한다.
```
$ sudo tar -xf spark-3.2.1-bin-hadoop3.2.tgz
```
압축을 푼 spark 파일을 /opt/spark 경로로 옮긴다
```
$ sudo mv spark-3.2.1-bin-hadoop3.2/ /opt/spark
```
spark의 환경변수를 지정한다.
```
export SPARK_HOME=/opt/spark  
export PATH=$SPARK_HOME/bin:$PATH
export PATH=$PATH:$SPARK_HOME/bin:$SPARK_HOME/sbin
export PYSPARK_PYTHON=/usr/bin/python3
```
spark를 실행해본다.
```
huisu@huisu:/opt/spark $ bin/spark-shell
```
아래처럼  웹 주소와 spark가 뜨면서 정상적으로 구동되는것을 확인 할 수 있다.
```
Spark context Web UI available at http://192.168.0.29:4041
Spark context available as 'sc' (master = local[*], app id = local-1650339265211).
Spark session available as 'spark'.
Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /___/ .__/\_,_/_/ /_/\_\   version 3.2.1
      /_/
         
Using Scala version 2.12.15 (OpenJDK 64-Bit Server VM, Java 11.0.14.1)
Type in expressions to have them evaluated.
Type :help for more information.

scala> 
```









