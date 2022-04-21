# Ubuntu 20.04에 Kafka 설치 및 환경 구성 
# kafka
## kafka 설치
보안을 위해 kafka 계정을 따로 만든다.
```
sudo adduser kafka
sudo adduser kafka sudo
su -l kafka
```
kafka 계정을 생성하고, sudo group에 추가함으로써, sudoer 권한을 부여한다.
그 다음 kafka 계정으로 로그인한다.

## kafka 다운로드

[kafka 공식 사이트](https://kafka.apache.org/downloads)에 방문하여 binary를 다운 받는다. 
```
$ wget https://dlcdn.apache.org/kafka/3.1.0/kafka_2.12-3.1.0.tgz
$ tar -xvzf kafka_2.12-3.1.0.tgz
$ mv kafka_2.12-3.1.0 kafka
$ cd kafka
```
kafka topic 으로 message를 발행하게 되어있으나, topic은 기본적으로 지울 수 없게 되어있다. 
```
$ vi ~/kafka/config/server.properties
```
```
###########################
# topic을 삭제할 수 있게 한다.
delete.topic.enable = true
# log path를 변경한다.
log.dirs=/home/kafka/logs
```
vi 편집기로 상단의 코드를 추가하여, topic 삭제 및 log path를 변경한다.

## 실행 
먼저 zookeeper를 실행한다.
```bash
$ bin/zookeeper-server-start.sh config/zookeeper.properties
```
그 다음 다른 터미널을 열고 kafka를 실행한다.
```bash
$ bin/kafka-server-start.sh config/server.properties
```

## 테스트

quickstart-events이라는 새로운 토픽을 생성한다.
```
bin/kafka-topics.sh --create --topic quickstart-events --bootstrap-server localhost:9092
```
quickstart-events 토픽의 대략적인 정보를 파악한다.
```
bin/kafka-topics.sh --describe --topic quickstart-events --bootstrap-server localhost:9092
```
quickstart-events 토픽에 메시지를 입력한다.
```
bin/kafka-console-producer.sh --topic quickstart-events --bootstrap-server localhost:9092
```
quickstart-events 토픽에 입력한 메시지를 본다.
```bash
bin/kafka-console-consumer.sh --topic quickstart-events --from-beginning --bootstrap-server localhost:9092
```



