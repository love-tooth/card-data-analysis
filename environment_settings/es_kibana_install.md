# 🖥️Ubuntu 환경에 Elasticsearch & Kibana 설치

## 📌 **1. Elasticsearch 7.11.1 설치**

### 1️⃣ **GPG 키 추가**

- 먼저 Elasticsearch 패키지의 GPG 키를 추가.
- 이 키는 패키지의 무결성을 검증하는 데 사용.

```bash
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg
```

### 2️⃣ **Elasticsearch 저장소 추가**

- Elasticsearch 패키지의 소스를 APT 소스 목록에 추가.

```bash
echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-7.x.list
```

### 3️⃣ **패키지 목록 갱신**

새로 추가한 저장소를 반영하기 위해 패키지 목록을 갱신.

```bash
sudo apt update
```

### 4️⃣ **Elasticsearch 7.11.1 설치**

```bash
sudo apt install elasticsearch=7.11.1
```

### 5️⃣ **Elasticsearch 설정**

Elasticsearch 설정 파일을 수정하여 **기본 설정**을 맞춰준다. 예를 들어, 로컬 네트워크에서만 접근 가능하도록 설정할 수 있다.

```bash
sudo vi /etc/elasticsearch/elasticsearch.yml
```

**설정 예시**:

```yaml
network.host: 0.0.0.0
http.port: 9200
discovery.type: single-node # 싱글 노드 환경 설정
```

### 6️⃣ **Elasticsearch 서비스 시작 및 활성화**

서비스를 시작하고, 시스템 부팅 시 자동으로 시작되도록 활성화.

```bash
sudo systemctl start elasticsearch
sudo systemctl enable elasticsearch
```

### 7️⃣ **서비스 상태 확인**

Elasticsearch가 제대로 실행되는지 확인.

```bash
sudo systemctl status elasticsearch
```

### 8️⃣ **방화벽에서 포트 열기**

만약 UFW(Uncomplicated Firewall)를 사용 중이라면, 9200번 포트를 열어줘야 한다.

```bash
sudo ufw allow 9200/tcp
```

### 9️⃣ **Elasticsearch 접근 확인**

브라우저나 `curl` 명령어로 Elasticsearch의 상태를 확인 가능.

```bash
curl 127.0.0.1:9200
```

---

## 📌 **2. Kibana 7.11.1 설치**

### 1️⃣ **Kibana 저장소 추가**

Kibana를 설치하기 위해 GPG 키를 추가하고 저장소를 설정.

```bash
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-7.x.list
```

### 2️⃣ **패키지 목록 갱신**

```bash
sudo apt update
```

### 3️⃣ **Kibana 7.11.1 설치**

```bash
sudo apt install kibana=7.11.1
```

### 4️⃣ **Kibana 설정**

설정 파일을 열어 Kibana의 기본 포트와 네트워크 설정을 조정.

```bash
sudo vi /etc/kibana/kibana.yml
```

**설정 예시**:

```yaml
server.host: "127.0.0.1"
server.port: 5601

# 다른 ubuntu 환경에 es를 설치했을 경우 해당 ubuntu의 ip 주소를 적어줘야함
elasticsearch.hosts: ["http://10.0.2.4:9200"]
```

### 5️⃣ **Kibana 서비스 시작 및 활성화**

Kibana 서비스를 시작하고, 시스템 부팅 시 자동으로 시작되도록 설정.

```bash
sudo systemctl start kibana
sudo systemctl enable kibana
```

### 6️⃣ **서비스 상태 확인**

Kibana가 정상적으로 실행되고 있는지 확인.

```bash
sudo systemctl status kibana
```

### 7️⃣ **방화벽에서 포트 열기**

Kibana 기본 포트(5601)를 방화벽에서 허용.

```bash
sudo ufw allow 5601/tcp
```

### 8️⃣ **Kibana 접근 확인**

브라우저에서 `http://127.0.0.1:5601`로 Kibana에 접근하여 대시보드를 확인.

---

## ⚡ **최종**

- **Elasticsearch**는 `127.0.0.1:9200`에서 접근 가능.
- **Kibana**는 `127.0.0.1:5601`에서 접근 가능.
- 방화벽(UFW)을 사용 중이라면 **포트 9200, 5601을 열어야 정상 접근 가능.**
