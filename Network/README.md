# 네트워크 - 이석복 교수 강의
내용별 정리 - [Velog](https://velog.io/@wwh11111/series/Network)

***

# Network Structure (네트워크 구성요소)

## 1. Network Edge : Applications and Hosts
- **End systems (hosts)**: 애플리케이션 프로그램 실행  
  - 예: Web, Email
- **Client/Server 모델**  
  - 클라이언트는 서비스를 요청하고, 서버는 항상 켜져 있으면서 대기  
  - 예: Web browser/server, Email client/server
- **Peer-to-Peer(P2P) 모델**  
  - 최소한의 전용 서버 사용  
  - 예: Skype, BitTorrent

## 2. Network Core : Routers
- 여러 라우터 간의 연결로 구성된 데이터 전송 경로 집합

## 3. Access Networks, Physical Media
- **통신 링크**: 라우터들을 연결하는 물리적인 매체 (유선, 무선 등)


# 데이터 통신 방식

## 1. Connection-Oriented Service: TCP (Transmission Control Protocol)
- **Reliable, In-order** byte-stream 전송 (신뢰성과 순서 보장)
- **Flow Control**: 수신자 상태에 따라 전송량 조절
- **Congestion Control**: 네트워크 혼잡 시 속도 조절
- **사용 예시**: HTTP, FTP, Telnet, SMTP (Email)

## 2. Connectionless Service: UDP (User Datagram Protocol)
- **Connectionless**: 연결 상태 없음
- **Unreliable**: 유실 가능성, 순서 보장 없음
- **No Flow/Congestion Control**
- **사용 예시**: 스트리밍 미디어, DNS, 실시간 음성
- ✅ 빠른 속도가 장점 (제어 없이 전송)

> 💡 **Protocol**: 통신을 원활히 하기 위한 규약(약속)


# 네트워크 데이터 전송 방식

## 1. Circuit Switching (회선 교환)
- 출발지 → 목적지까지 **경로 사전 설정**, 자원 고정 할당
- 단점: 자원 낭비 (사용자가 실제 전송 안 해도 자원 유지됨)
- 예: 유선 전화
- 예시 상황:  
  - Bandwidth: 1 Mbps  
  - 사용자당 100 kbps 사용 시  
  - ➡️ 최대 10명 동시 사용 가능

## 2. Packet Switching (패킷 교환)
- 요청 시 공유 (통계적 다중화)
- 장점: 자원 낭비 없음
- 단점: 패킷 지연 발생 가능 (경로가 유동적)


# 패킷 딜레이 (Packet Delay) 종류

1. **Nodal Processing Delay**  
   - 오류 검사 등 패킷 처리 시간

2. **Queueing Delay**  
   - 큐에서 대기 (버퍼 존재)  
   - ⚠️ 큐 초과 시 **Packet Loss(유실)** 발생

3. **Transmission Delay**  
   - 패킷이 전송선에 나가기 시작 → 끝나는 데 걸리는 시간  
   - 계산: `Packet size / Transmission rate`

4. **Propagation Delay**  
   - 신호가 매체를 따라 전파되는 시간  
   - 매체의 길이에 비례 (광속 기준)

# 패킷 딜레이 줄이는 방법

| Delay 종류         | 줄이는 방법                             |
|--------------------|------------------------------------------|
| Processing Delay    | 라우터 성능 업그레이드                 |
| Queueing Delay      | ❌ 사용량/사용자 수에 의존 → 제어 불가 |
| Transmission Delay  | 케이블 업그레이드 (Bandwidth 증가)     |
| Propagation Delay   | ❌ 광속 기준 → 제어 불가               |

***

# 📡 TCP/IP 4 Layer와 Application 프로토콜

## ✅ TCP/IP 4계층 구조

| 계층 이름         | 강의 분류     | 설명 |
|------------------|---------------|------|
| Application Layer | Application   | 웹, 이메일, FTP 등의 **애플리케이션 프로토콜** 동작 |
| Transport Layer   | Transport     | TCP/UDP를 통해 데이터 전송을 **신뢰성 있게 혹은 빠르게** |
| Internet Layer    | Network       | IP 주소를 통해 **호스트 간 라우팅** |
| Network Access Layer | Link      | 실제 **물리적 매체 및 MAC 주소** 기반 데이터 전송 |

---

## 🧠 Application 개발자 관점

- 보통 앱 개발자는 **Application Layer만 신경쓰며**, Transport 이하 계층은 OS와 라이브러리가 처리
- **Client-Server Architecture**  
  - **Client**: 필요할 때만 서버에 요청
  - **Server**: 항상 실행되며 고정된 IP 주소로 요청 수신

---

## 🧩 소켓(Socket)

- **Socket**: 프로세스 간 통신을 위한 OS API  
- 소켓을 통해 **Application Layer ↔ Transport Layer** 연결
- **IP**: 인터넷 상에서 컴퓨터를 식별  
- **Port**: 해당 컴퓨터 내의 특정 애플리케이션(프로세스) 식별

---

## 🚚 Transport Layer에서 Application이 원하는 기능

Application Layer는 Transport Layer에게 다음 기능을 원하지만, **항상 보장되는 것은 아님**:

| 요구 사항         | 설명 | TCP 지원 여부 |
|------------------|------|----------------|
| Data Integrity   | 손실 없이 정확한 데이터 전송 | ✅ |
| Timing           | 특정 시간 내에 도착 보장 | ❌ |
| Throughput       | 충분한 전송 속도 | ❌ |
| Security         | 암호화 등 보안 | ❌ (→ Application에서 직접 구현) |

---

## 🌐 HTTP (HyperText Transfer Protocol)

- 웹의 **애플리케이션 계층 프로토콜**
- **TCP 기반**, 기본 포트: `80`
- **Stateless**: 클라이언트 상태를 저장하지 않음 → `Cookie`로 보완

### HTTP 연결 방식

#### 1. Non-persistent HTTP
- 매 HTTP 요청마다 새로운 TCP 연결
- 요청 후 연결 종료
- **비효율적**이고 RTT 증가

#### 2. Persistent HTTP
- 하나의 TCP 연결로 여러 HTTP 요청 처리
- **HTTP/1.1부터 기본 방식**
- **파이프라인(Pipelining)** 기능 가능

---

## 📦 Web Cache (프록시 서버)

- **Proxy Server**: 클라이언트와 서버 사이에서 요청을 중계
- **Cache 기능**을 이용해 자주 요청되는 데이터 저장
- 장점:
  - 응답 시간 단축
  - 트래픽 절감
  - 외부 접속 최소화 → 병목 완화

### 🔁 Conditional GET
- 캐시된 객체가 최신인지 서버에 확인
- `If-Modified-Since` 헤더 사용
- 서버 응답:
  - 변경됨: `200 OK + data`
  - 변경 없음: `304 Not Modified`

---

## 🌍 DNS (Domain Name System)

- **도메인 이름 → IP 주소** 변환 시스템
- 전화번호부 역할

### 계층 구조 (중앙집중형 DNS를 피함)

| 계층 | 설명 |
|------|------|
| Root DNS         | 최상위 DNS 서버, TLD로 위임 |
| TLD (Top-Level Domain) 서버 | `.com`, `.org`, `.kr` 등 도메인 구분 |
| Authoritative DNS 서버 | 실제 IP 주소 정보 보유 (도메인 소유자가 관리) |

### 📄 DNS Record 타입
- **A**: 도메인 이름 → IP 주소
- **NS**: 다음 이름 서버(Name Server) 정보

### DNS 프로토콜 특징
- **Application 계층 프로토콜**
- **UDP 기반** (기본적으로 빠름, 신뢰성 낮음)
- HTTP는 **TCP 기반** → 신뢰성 높음

---

## 🔌 소켓(Socket)의 종류

| 소켓 타입 | 설명 |
|-----------|------|
| **TCP (SOCK_STREAM)** | 연결 지향, 신뢰성 있는 전송 |
| **UDP (SOCK_DGRAM)**  | 비연결, 빠르지만 신뢰성 없음 |

### TCP 소켓 통신 흐름

#### 📡 Server 측
1. `socket()`: 소켓 생성
2. `bind()`: IP + 포트 번호와 바인딩
3. `listen()`: 연결 요청 대기
4. `accept()`: 클라이언트 요청 수락

#### 💻 Client 측
1. `socket()`: 소켓 생성
2. `connect()`: 서버에 연결 요청
- 일반적으로 `bind()` 불필요 (OS가 임시 포트 할당)

---

## ✅ 요약 정리

- 앱 개발자는 주로 **Application Layer**를 다루며, TCP나 UDP는 OS 수준에서 처리
- HTTP, DNS는 모두 **Application 계층 프로토콜**
- TCP는 **데이터 신뢰성**, UDP는 **속도**를 중시
- DNS는 **이름 → IP 주소** 매핑, HTTP는 **리소스 요청/응답**
- **프록시 서버와 캐시**, **Persistent 연결**은 성능 최적화의 핵심
