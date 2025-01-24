# NAT 기반 

<br>

## 1. 📫 개요 
현재 우리나라에서 애플페이를 지원하는 카드사가 적은 가운데, 많은 카드사들이 이 사업에 뛰어들기 위해 노력하고 있습니다. 
이런 카드사들 가운데, **‘소비자들에게 어떤 혜택을 제공하면 자신의 카드사를 선택할 수 있을지’** 대한 인사이트를 제공하기 위해 카드 실데이터를 이용한 분석을 진행한 프로젝트입니다.

<br>

<br>

## 2. 📍 목표
- Ubuntu 위에 ElasticSearch 및 Kibana 환경 설정하는 방법을 습득합니다.
- 카드사 실데이터를 활용한 Kibana 시각화를 통해 데이터 분석 능력을 기릅니다. 

<br>


## 3. 🗜 기술 스택

![Linux](https://img.shields.io/badge/Linux-FCC624?style=flat&logo=linux&logoColor=black)
![Elasticsearch](https://img.shields.io/badge/Elasticsearch-005571?style=flat&logo=elastic&logoColor=white)
![Kibana](https://img.shields.io/badge/Kibana-005571?style=flat&logo=elastic&logoColor=white)
![VirtualBox](https://img.shields.io/badge/VirtualBox-183A61?style=flat&logo=virtualbox&logoColor=white)
![MobaXterm](https://img.shields.io/badge/MobaXterm-008FBA?style=flat&logoColor=white)




## 4. ✋ 개발 구성원 
|                                         Project Manager                                         |                                        Data Visualization Specialist                                         |                                         UX/UI Designer                                         |                                        Data Analyst                                        |
|:---------------------------------------------------------------------------------------:|:--------------------------------------------------------------------------------------:|:---------------------------------------------------------------------------------------:|:---------------------------------------------------------------------------------------:|
| <img src="https://github.com/user-attachments/assets/20d2f5f4-5e8b-4cc0-a1e5-effbd7ad1650" width=200px alt="오현두"> | <img src="https://github.com/user-attachments/assets/ebf19366-9f48-4408-a23b-73e137bf1507" width=200px alt="임하은"> | <img src="https://github.com/user-attachments/assets/05e882db-2001-4a7c-b4be-b5bf29b69915" width=200px alt="정파란"> | <img src="https://github.com/user-attachments/assets/de720e0b-8d28-4163-b141-0ac6bd8745b3" width=200px alt="김예진"> |
|                          [오현두](https://github.com/HyunDooBoo)                           |                           [임하은](https://github.com/imhaeunim)                            |                           [정파란](https://github.com/BlueRedOrange)                            |                          [김예진](https://github.com/yeejkim)                           |
|                                           PM                                            |                                           DVS                                           |                                           UD                                            |                                           DA                                            |

<br> 






## 📌 목차

0. [**개요**]
1. [**시각화 및 인사이트 도출**]
2. [**환경 세팅**]
3. [**아키텍처**] 
5. [**트러블 슈팅**]
6. [**회고**]
7. [**참고 문헌**]

<br>


## 📫 개요 
현재 우리나라에서 애플페이를 지원하는 카드사가 적은 가운데, 많은 카드사들이 이 사업에 뛰어들기 위해 노력하고 있습니다. 
이런 카드사들 가운데, **‘소비자들에게 어떤 혜택을 제공하면 자신의 카드사를 선택할 수 있을지’** 대한 인사이트를 제공하기 위해 카드 실데이터를 이용한 분석을 진행하였습니다. 



💡 애플페이란 ? 
  - 애플이 제공하는 모바일 결제 서비스로, 아이폰, 애플 워치 등에서 사용할 수 있습니다. 실물 카드나 현금 대신 사용할 수 있어 편리성이 뛰어납니다. 
  - 하지만 현재 대한민국에선 유일하게 카드사 한 곳만 지원하고 있어 소비자들에게 불편함을 주고 있습니다. 



우리는 먼저 애플페이 유저들의 주 연령층은 누구인지 확인했습니다. 애플페이 유저란 곧 아이폰 유저이기 때문에, 아이폰 유저들의 연령층에 대해 조사하였습니다. 
기사에 따르면, 20~30대가 주 연령층이며, 특히 20대 여성은 75%, 30대 여성이 59%로 높은 비율을 차지했습니다.

## 📊 시각화 및 인사이트 도출

- 아이폰 유저는 20~30대가 대부분임을 확인하여 이 연령층을 집중적으로 분석하였다.

1. 20~30대의 대부분은 카드사의 ‘라이프 스테이지’ 구분에서 ‘**UNI(대학생)**’ 혹은 ‘**NEW_JOB(사회초년생)**’임을 확인하였다.
   
   ![image](https://github.com/user-attachments/assets/102e20b5-0c89-4134-8bd6-dc61f5695473)

<br>

2. 대학생 및 사회초년생은 아직 신용카드보다 **체크카드 소비량**이 압도적으로 많음을 확인하였다.  이를 통해 추후 분석부터 이들의 체크카드 사용량을 중심으로 분석하였다. 
    
   ![image](https://github.com/user-attachments/assets/0d49f352-e9e4-4eb6-b74e-884c49c635e1)

<br>
    
3. 20~30대로 필터링하여, **어느 분야에서 가장 많은 지출이 있는지** 확인한 결과, ‘**유통업**’이 압도적으로 많음을 확인할 수 있었다. 이는 유통업이란 도매, 소매, 물류 및 운송 모든 과정을 포함한 폭 넓은 산업이기 때문임을 알 수 있었다. 
    
    ![image](https://github.com/user-attachments/assets/c605e83a-e01b-4693-8397-b63161f9dc1c)
   
<br>
    
4. 유통업 분야가 방대하여 정확한 분석이 어려워, 유통업을 제외하고 시각화를 진행하였다. 그 결과, 순서대로 **요식업**, **영리 유통업**, **보험 및 병원**, 그리고 **일반 휴게음식업**에서 높은 비율을 차지한 것을 알 수 있다. 
    
    ![image](https://github.com/user-attachments/assets/fa02c9fd-4d70-4619-8513-02fd0fd3fad2)


 
    - 따라서 카드사는 이들 분야에 대한 혜택을 강화해야 한다는 인사이트를 얻었다.
    
    - 예를 들어 요식업에서의 할인 혜택, 영리 유통업에서의 캐시백 프로그램, 그리고 보험 및 병원 분야에서 의료비 할인 및 포인트 적립 프로그램을 통해 고객 유입도 및 만족도를 높일 수 있다. 

    
5. 연령별 카드사의 **모바일 서비스(앱, 웹, 홈페이지 채널) 가입 비율**을 나타낸 그래프이다. 20대부터 30대가 다른 연령층에 비해 많은 비율이 가입함을 알 수 있었다. 

    ![image](https://github.com/user-attachments/assets/07e8e355-6590-4465-b3fb-981532bcd2f5)


    
    - 그러므로 젋은 연령층은 모바일 서비스에 대한 거부감이 없기 때문에, 지출 상위 분야에 대해 온라인 혜택까지 확장하면 더 좋은 결과가 있을 수 있음을 알 수 있었다.

<br>


## ✨ 환경 세팅

  - 다중 Ubuntu 환경 구성 및 MobaXterm 연동
    - 링크
      
  - Elasticsearch / kibana 설치
    - 링크

<br>


## 🌐 아키텍처 



<br>

## 🧨 트러블 슈팅

### 🚀 Elasticsearch 설정 시 Kibana 연결 문제 해결

- ⚠️ 문제 발생:  클러스터 구조로 인한 Kibana 연결 오류
  - Elasticsearch 기본 설정 값이 여러 노드간 클러스터를 구성하도록 설계되어있다.
  - 만약, 단일 노드만 실행하려면, 클러스터 기본 설정을 해주어야 한다. 

---

### ✅ 해결 방법 1: **싱글 노드 모드 활성화**

- 단일 노드 환경에서 Elasticsearch와 Kibana를 원활하게 연동하려면  `discovery.type`을 `single-node`로 설정해야 한다.

```yaml
discovery.type: single-node  # 싱글 노드 환경 설정
```

- 이 설정을 적용하면 Elasticsearch가 **클러스터 모드가 아닌 단일 노드 환경**에서 동작하며, Kibana가 데이터를 정상적으로 읽을 수 있다.

---

### ✅ 해결 방법 2: 클러스터 모드 사용 (여러 노드 환경)

- `discovery.seed_hosts` 설정을 추가하여 Elasticsearch가 연결할 노드를 지정해준다.

- ⚡ `discovery.seed_hosts`란?
    - `discovery.seed_hosts`는 **Elasticsearch 클러스터를 구성할 때 사용되는 설정**이다.
    - 이 설정을 사용하면 Elasticsearch가 지정된 호스트를 찾아 다른 노드들과 연결하려고 시도한다.
    - 7.0버전부터 사용 가능하다.

```yaml
discovery.seed_hosts: ["127.0.0.1"] #여러 개의 노드 설정 가능
```

- 위와 같이 설정하면, Elasticsearch는 "127.0.0.1"에 다른 노드가 존재한다고 가정하고 해당 노드와 연결을 시도한다.
- 하지만 **단일 노드 환경에서는 추가 노드가 없으므로, 클러스터 구성이 실패할 수 있다.**
- 이로 인해 Kibana가 Elasticsearch에 접속할 때도 **연결이 제대로 이루어지지 않는 문제**가 발생할 수 있다.

---

### ✅ 해결 방법 3: **싱글 노드 모드 활성화**
- 단일 노드 환경에서 Elasticsearch와 Kibana를 원활하게 연동하려면, 다음과 같이 설정해야 한다.

```yaml
discovery.type: single-node  # 싱글 노드 환경 설정
```

- 이 설정을 적용하면 Elasticsearch가 **클러스터 모드가 아닌 단일 노드 환경**에서 동작하며, Kibana가 데이터를 정상적으로 읽을 수 있다.

---

### 🎯 결론

- **❌ `discovery.seed_hosts`**: 클러스터 환경에서는 필요하지만, 단일 노드에서는 불필요.
- **✅ `discovery.type: single-node`**: 단일 노드 환경에서는 이 설정을 적용하여 Kibana와의 연결 문제 해결.


<br>

## 회고

  - 오현두
    - 내용 추가
      
  - 임하은
    - 내용 추가
      
  - 정파란
    - 내용 추가
  
  - 김예진
    - 내용 추가

<br>

## 📚 참고 문헌

- WILLOW. (2023년 6월 9일). “버티면 승리한다”… 애플페이, 드디어 다른 카드사도 가능할까. https://www.card-gorilla.com/contents/detail/2573.
- Apple Inc. (n.d.). 애플페이. Retrieved January 24, 2025, from https://www.apple.com/kr/apple-pay/
- 김경욱. (2024년 7월 10일). 한국 성인 69% ‘갤럭시 사용자’…20대 64%는 아이폰 쓴다. 한국갤럽 ‘2024년 스마트폰 사용 현황’. Retrieved January 24, 2025, from https://www.hani.co.kr/arti/economy/economy_general/1148604.html
