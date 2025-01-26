# 🖥️ VirtualBox를 이용한 다중 Ubuntu 환경 구성 및 MobaXterm 연동 🚀

이 문서는 VirtualBox에서 NAT 네트워크를 사용하여 여러 개의 Ubuntu 인스턴스를 실행하고, MobaXterm을 통해 SSH로 접속, 그리고 Elasticsearch 및 Kibana를 설치하여 Windows에서 접근하는 방법을 정리한 가이드.

❓ NAT(Network Address Translation)

- 사설 IP를 공인 IP로 변환하여 인터넷에 접속할 수 있도록 하는 기술.
- VM간 직접적인 통신 불가.

❓ NAT 네트워크

- 가상 머신(VM)이 호스트의 NAT 기능을 이용해 외부 네트워크와 통신하는 방식.
- VM간 통신이 가능하며, 각 VM은 고유한 사설 ip를 할당 받음.
- 외부에서 접근하려면 포트포워딩이 필요함.

---

## 🔹 1. VirtualBox에서 다중 Ubuntu 인스턴스 생성 및 NAT 네트워크 설정 ⚙️

### 🛠 1.1 Ubuntu 가상 머신 생성

1. VirtualBox 실행 → "새로 만들기" 클릭
2. **이름** 및 **운영 체제** 설정:
    - 이름: Ubuntu-01, Ubuntu-02 등
    - 종류: Linux
    - 버전: Ubuntu (64-bit)
    
    ![image](https://github.com/user-attachments/assets/8c1039a3-5758-49b9-8654-71292093514d)

    
3. 메모리 크기 설정 (최소 2GB 권장)
4. 가상 하드 디스크 생성:
    - VDI (VirtualBox 디스크 이미지) 선택
    - 동적 할당 또는 고정 크기 선택
    - 크기: 최소 10GB 이상
5. "생성" 클릭

### 🌐 1.2 NAT 네트워크 설정

1. VirtualBox NAT 네트워크 생성
    
    ![image](https://github.com/user-attachments/assets/ff9d879a-d88a-413a-9698-cd49145cccb4)

    
2. **네트워크(Network)** 탭 선택 → "NAT 네트워크" 추가
    - 이름: `NatNetword`
    - 네트워크 CIDR: `10.0.2.0/24`
    - DHCP 활성화
3. 생성한 각 ubuntu의 설정 수정을 해줘야한다.
    1. ubuntu 클릭 → 설정 → Expert → 네트워크
    2. 이름 → 위에서 설정한 NAT 네트워크
    
    ![image](https://github.com/user-attachments/assets/4492eaa0-2ace-42d5-81dd-30b3c6509c55)

    

## 🔹 2. 각 Ubuntu에 OpenSSH 서버 설치 및 포트 설정 🔑

### 🖥️ 2.1 OpenSSH 설치 및 활성화

각 가상 머신에서 SSH 서버를 설치하고 활성화한다.

1. SSH 설치:
    
    ```bash
    sudo apt update
    sudo apt install openssh-server -y
    sudo systemctl enable ssh
    sudo systemctl start ssh
    ```
    
2. SSH 설치 상태 확인:
    
    ```bash
    sudo systemctl status ssh
    ```
    

### 🔄 2.2 SSH 포트 변경

1. 각 가상 머신에서 SSH 포트로 변경.
2. `/etc/ssh/sshd_config` 파일을 수정 및 포트 설정. 
    
    ```bash
    sudo nano /etc/ssh/sshd_config
    ```
    
3. 각 VM에 맞는 포트 변경. 
    - MySQL VM: `Port 22`
    - Kibana VM: `Port 23`
    - Elasticsearch VM: `Port 24`
4. 파일 수정 후 SSH 서버를 재시작하여 변경 사항 적용.
    
    ```bash
    sudo systemctl restart ssh
    ```
    
5. 새로운 포트를 방화벽에서 허용해야 합니다. 각각의 ubuntu에 ufw 명령어를 이용하여 포트를 허용. (NAT 네트워크로 포트 포워딩 사용 시 하지 않아도 됨)
    
    ```bash
    sudo ufw allow 22/tcp  # MySQL VM
    sudo ufw allow 23/tcp  # Kibana VM
    sudo ufw allow 24/tcp  # Elasticsearch VM
    ```
    
    - 기존의 `22번` 포트 규칙을 제거하려면:
        
        ```bash
        	sudo ufw delete allow 22/tcp
        
        ```
        

### 🔄 2.3 각 ubuntu ip 주소 확인

1. 📦 net-tools 패키지 설치
    - ifconfig 명령어를 사용하기 위해 net-tools 패키지 설치.
        
        ```bash
        sudo apt update
        sudo apt install net-tools
        ```
        
2. 📡 IP 주소 확인
    - 위의 과정을 진행한 이후 ifconfig 명령어를 사용하여 여러 Ubuntu 인스턴스의 IP 주소 확인.
    - 아래 사진은 10.0.2.5 임을 확인.
    
    ![image](https://github.com/user-attachments/assets/b8ecebe3-0f42-4470-be9f-7229da21b35e)

    

### 🔄 2.4 포트 포워딩 설정

- 위 과정을 통해 알아온 각 ubuntu의 ip 주소와, ssh 포트 번호를 이용해 각 가상 머신마다 SSH, Elasticsearch, Kibana 접근을 위해 포트 포워딩 추가.

| 역할 | 프로토콜 | 호스트 IP | 호스트 포트 | 게스트 IP | 게스트 포트 |
| --- | --- | --- | --- | --- | --- |
| ssh sql | TCP | 127.0.0.1 | 22 | 10.0.2.4 | 22 |
| ssh kibana | TCP | 127.0.0.1 | 23 | 10.0.2.15 | 23 |
| ssh es | TCP | 127.0.0.1 | 24 | 10.0.2.5 | 24 |

---

---

### 3. MobaXterm을 이용한 원격 접속 🔗

### 3.1 MobaXterm을 이용해 각 VM에 접속

1. MobaXterm 실행 → "Session" 클릭
2. **SSH** 선택 후 아래 정보 입력:
    - **Remote Host**: `127.0.0.1`
    - **Port**: 각 VM에 맞는 포트 (예: 22, 23, 24)
    
    ![image](https://github.com/user-attachments/assets/4bb844dc-c21f-4964-8c2e-1dd3e5c1daaf)

    
3. "OK" 클릭 후 ubuntu의 아이디, 비밀번호를 입력하여 접속.
