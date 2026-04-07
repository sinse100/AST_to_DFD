# AST_to_DFD

## 1. Install Neo4J
+ OpenJDK, OpenJRE 21 설치
```
sudo apt update -y
sudo apt install fontconfig openjdk-21-jre openjdk-21-jdk -y
```
+ Neo4J 설치 (2026.02.02 기준 1:2025.12.1 버전 설치 권장)
```
sudo mkdir -p /etc/apt/keyrings
wget -O - https://debian.neo4j.com/neotechnology.gpg.key | sudo gpg --dearmor -o /etc/apt/keyrings/neotechnology.gpg > /dev/null
sudo chmod a+r /etc/apt/keyrings/neotechnology.gpg
echo 'deb [signed-by=/etc/apt/keyrings/neotechnology.gpg] https://debian.neo4j.com stable latest' | sudo tee -a /etc/apt/sources.list.d/neo4j.list > /dev/null
sudo apt-get update -y
sudo apt-get install -y neo4j=1:2025.12.1
```
+ 외부에서 Neo4J 접속이 가능하도록 포트 및 연결 주소를 설정하기 위하여 Neo4J 설정 파일(```/etc/neo4j/neo4j.conf```) 열고 해당 항목의 앞 부분의 주석(#)을 해
```
server.default_listen_address=0.0.0.0
server.http.listen_address=:7474
server.bolt.listen_address=:7687
```
+ Neo4J를 동작시킴
```
 sudo systemctl start neo4j
```
