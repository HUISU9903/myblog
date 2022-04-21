---
title: "Ubuntu 20.04에 Hadoop 설치 및 환경 구성" 
excerpt: "Ubuntu 20.04 버전에 Hadoop를 설치하는 방법을 알아본다."
categories:
- Setting
tags:
- [Setting]
- [Hadoop]
date: 2022-04-16 20:00:00
---
# Hadoop 설치하기 및 환경 구성하기
## JAVA 설치
```
$ sudo apt update
$ sudo apt install openjdk-11-jdk
$ java -version
```
상단의 코드를 실행하여 자바를 설치한 후, 제대로 설치되었는지 확인한다.
```
$ vi ~/.bashrc
```
```
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
```
제대로 설치되었다면, .bashrc 파일에 자바 환경변수를 추가해준다.
## SSH 설치
```
sudo apt-get install ssh
sudo apt-get install pdsh
```
##  ssh 키 생성 및 권한 부여
```
  $ sudo chmod -R 777 /home/huisu/.bashrc 
  $ ssh-keygen -t rsa -P "" 
  $ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys 
  $ ssh localhost 
  exit 
  $ sudo apt-get update  
  ```
## Hadoop 다운로드 및 압축해제
[Hadoop 공식 다운로드 사이트](https://hadoop.apache.org/releases.html)에 접속하여 원하는 버전을 binary로 다운 받은 후 압축을 해제한다.
```
$ wget https://dlcdn.apache.org/hadoop/common/hadoop-3.3.2/hadoop-3.3.2.tar.gz
$ tar zxf hadoof-3.3.2.tar.gz 
``` 
## Hadoop 설정하기
### bashrc 
```
$ vi ~/.bashrc
```
```
export HADOOP_HOME="/home/huisu/hadoop"
export HADOOP_INSTALL=$HADOOP_HOME
export HADOOP_MAPRED_HOME=${HADOOP_HOME}
export HADOOP_COMMON_HOME=${HADOOP_HOME}
export HADOOP_HDFS_HOME=${HADOOP_HOME}
export YARN_HOME=${HADOOP_HOME}
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
export PATH=$PATH:$HADOOP_HOME/bin
export PATH=$PATH:$HADOOP_HOME/sbin
export PDSH_RCMD_TYPE=ssh
```
.bashrc 파일에 환경변수를 추가해준다.
### hadoop-env.sh
```
$ vi $HADOOP_HOME/etc/hadoop/hadoop-env.sh 
```
```
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
```
`hadoop-env.sh`파일에 JAVA 환경변수를 추가해준다.
### core-site.xml
```
$ vi $HADOOP_HOME/etc/hadoop/core-site.xml
```
```
<configuration>
	<property>
       <name>hadoop.tmp.dir</name>
       <value>/home/huisu/hadoop/tmpdata</value>
    </property>
    <property>
       <name>fs.default.name</name>
       <value>hdfs://127.0.0.1:9000</value>
    </property>
</configuration>
```
`core-site.xml`파일에 상단의 코드를 추가하며, 만약 `/home/huisu/hadoop/tmpdata` 해당 경로에 tmpdata 폴더가 존재하지 않을 경우 `mkdir`을 이용하여 폴더를 만들어주어야 한다.
```
mkdir -p /home/huisu/hadoop/tmpdata
```
### hdfs-site.xml
```
$ vi $HADOOP_HOME/etc/hadoop/hdfs-site.xml
```
```
<configuration>
      <property>
         <name>dfs.data.dir</name>
         <value>/home/huisu/hadoop/hdfs/namenode</value>
      </property>
      <property>
          <name>dfs.data.dir</name>
          <value>/home/huisu/hadoop/hdfs/datanode</value>
      </property>
      <property>
          <name>dfs.replication</name>
          <value>1</value>
      </property>
</configuration>
```
`hdfs-site.xml`파일에 상단의 코드를 추가하며, 만약 `/home/huisu/hdfs/namenode` `/home/huisu/hdfs/datanode`에 각각 namenode, datanode가 존재하지 않을 경우, `mkdir`을 이용하여 폴더를 만들어주어야 한다.
```
mkdir -p /home/huisu/hadoop/hdfs/namenode 
mkdir -p /home/huisu/hadoop/hdfs/datanode  
```
### mapred-site.xml
```
$ vi $HADOOP_HOME/etc/hadoop/mapred-site.xml
```
```
<configuration>
      <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
      </property>
      <property>
        <name>yarn.app.mapreduce.am.env</name>
        <value>HADOOP_MAPRED_HOME=/home/huisu/hadoop</value>
      </property>
      <property>
        <name>mapreduce.map.env</name>
        <value>HADOOP_MAPRED_HOME=/home/huisu/hadoop</value>
      </property>
      <property>
        <name>mapreduce.reduce.env</name>
        <value>HADOOP_MAPRED_HOME=/home/huisu/hadoop</value>
      </property>
</configuration>
```
`mapred-site.xml`파일에 상단의 코드를 추가한다.

### yarn-site.xml
```
$ vi $HADOOP_HOME/etc/hadoop/yarn-site.xml
```
```
 <configuration>
	  <!-- Site specific YARN configuration properties -->
      <property>
         <name>yarn.nodemanager.aux-services</name>
         <value>mapreduce_shuffle</value>
      </property>
      <property>
         <name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
         <value>org.apache.hadoop.mapred.ShuffleHandler</value>
      </property>
      <!--
      <property> 
	     <name>yarn.resourcemanager.hostname</name>
	     <value>127.0.0.1</value>  
	  </property>
	  <property> 
		<name>yarn.acl.enable</name> 
	    <value>0</value> 
     </property> 
     <property> 
	    <name>yarn.nodemanager.env-whitelist</name>             
	    <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,
	    HADOOP_CONF_DIR,CLASSPATH_PERPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME
	    </value> 
	 </property>
</configuration>
```
`yarn-site.xml`파일에 상단의 코드를 추가한다.
## Hadoop 실행하기
NameNode를 한번 포맷 한다.(Hadoop 서비스를 처음 시작하기 전에 NameNode의 포맷이 중요하다.)
```
hdfs namenode -format
```
NameNode 및 DataNode를 실행한다.
```
sbin/start-dfs.sh
```
![dfs](https://user-images.githubusercontent.com/65166786/164246153-e43c1845-81c1-4c36-9e1b-019b81abdb21.png)

YARN도 실행하여 확인해 준다.
```
sbin/start-yarn.sh
```
![yarn.sh](https://user-images.githubusercontent.com/65166786/164246320-f054fb8e-3888-4021-967a-b0b4ffb8466a.png)

모든 데몬이 활성 상태이고 프로그램이 JAVA 프로세스로 실행 중인지 확인한다.
```
jps
```
![jps](https://user-images.githubusercontent.com/65166786/164246356-b2f4a58f-81a4-45ec-b2c8-99f0be061f9f.png)

## 웹에서 Hadoop 사용
### NameNode UI 
제대로 실행이 되었을 경우 http://localhost:9870로 접속하면 아래와 같은 화면이 뜨는 것을 확인할 수 있다.
NameNode UI는 전체 클러스터에 대한 포괄적인 개요를 제공한다.
![NameNode UI](https://user-images.githubusercontent.com/65166786/164247306-c6ea8d8e-2053-4ab0-96c3-adccfa88a18f.png)

http://localhost:9864의 경우 직접 개별 DataNode에 액세스하는데 사용된다.
![DataNode](https://user-images.githubusercontent.com/65166786/164248281-2453456a-7454-46d1-abe6-f4efb8f396a7.png)

YARN Resource Manager의 경우 http://localhost:8088에서 가능하다.
Resource Manager는 Hadoop 클러스터에서 실행 중인 모든 프로세스를 모니터링할 수 있다.
![image](https://user-images.githubusercontent.com/65166786/164248932-6e4f9f36-ae95-4633-bb3c-2361600bd146.png)

