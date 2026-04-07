<img width="1920" height="960" alt="image" src="https://github.com/user-attachments/assets/0ac201ce-1e36-4bbc-82d2-0abc2ca07d60" /># AST_to_DFD

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
## 2. 외부 트래픽 연결 허용
+ 외부에서 Neo4J로 접속하는 트래픽이 필터링되지 않도록 위의 7474 포트와 7687 포트 대상 트래픽 허용 규칙을 추가
+ Neo4J DB로 원활히 접속되는지 확인을 위하여 ```http://{Neo4J 서버 IP 주소}:7474``` 로 접속
  + Neo4J의 기본 계정 및 비밀번호는 (ID : neo4j, PW : neo4j) 와 같음
 
## 3. Neo4J 형식에 맞는 그래프 작성을 위한 APOC 스크립트 플러그인 설치
+ 먼저, 아래 명령어를 통하여 APOC 플러그인이 동봉되어 있는지 확인해야 함
```
ls -l /var/lib/neo4j/labs | grep -i apoc
```
+ labs 폴더에 있는 APOC 패키지를 Neo4J App 폴더에 복붙
```
sudo cp /var/lib/neo4j/labs/apoc-2025.12.1-core.jar /var/lib/neo4j/plugins/
sudo chown neo4j:neo4j /var/lib/neo4j/plugins/apoc-2025.12.1-core.jar
sudo chmod 644 /var/lib/neo4j/plugins/apoc-2025.12.1-core.jar
```
## 3. 소스코드 파싱 라이브러리(CPG) 설치
+ 사용자 홈 디렉토리(/~)에서 아래 명령어를 입력하여 CPG 저장소 다운로드
```
git clone https://github.com/Fraunhofer-AISEC/cpg.git
```
+ CPG 빌드를 하기 위한 설정파일 작성
```
mv ~/cpg/gradle.properties.example ~/cpg/gradle.properties
```
+ Neo4J 연동 CPG 도구 빌드를 위해 빌드 재료가 있는 폴더로 이동
```
cd ~/cpg/cpg-neo4j
```
+ gradle 도구를 사용하여 CPG 빌드
```
../gradlew installDist
```
+ 이후 아래와 같은 명령어를 통해 여러 코드 파일들에 대한 CPG들을 하나의 파일에 통합 생성
```
build/install/cpg-neo4j/bin/cpg-neo4j --export-json cpg-export.json --no-neo4j --top-level=~/cpg_test/test_proj2
```
