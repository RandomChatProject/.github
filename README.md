# 🗨️ Simple Random Chat

랜덤으로 매칭된 사용자 간 1:1 실시간 채팅을 제공하는 웹 서비스입니다.  
로그인 없이 접속 즉시 채팅이 가능하며, Spring WebSocket과 STOMP 기반의 실시간 통신을 경험하기 위해 제작되었습니다.

---

## 📌 프로젝트 개요

- **프로젝트명**: Simple Random Chat
- **개발 기간**: 2주
- **개발 인원**: 1명 (개인 프로젝트)
- **개발 언어**: Kotlin
- **JDK 버전**: 21
- **목적**
  - Spring Boot 기반 WebSocket 실시간 통신 이해
  - STOMP 메시지 흐름 구조 학습
  - 간단한 랜덤 매칭 로직 구현 경험

---

## ⚙️ 기술 스택

### Backend
- Kotlin
- Spring Boot
- Spring WebSocket (STOMP)
- Spring MVC
- JDK 21

### Frontend
- HTML / CSS
- JavaScript
- SockJS
- Stomp.js

### Database
- 사용하지 않음 (In-Memory 관리)

---

## 🏗️ 아키텍처

Client (Browser)
└─ SockJS / STOMP
└─ WebSocket Endpoint (/ws)
├─ /queue/match
└─ /topic/chat/{roomId}


- 로그인 없이 세션 기반으로 사용자 식별
- 서버 메모리에서 대기열 및 채팅방 관리

---

## ✨ 주요 기능

### 1️⃣ 랜덤 매칭
- 채팅 시작 시 대기열 등록
- 대기 중인 사용자 2명을 랜덤으로 매칭
- 매칭 완료 시 임시 채팅방 생성

### 2️⃣ 실시간 채팅
- WebSocket + STOMP 기반 메시지 송수신
- 채팅방 단위 메시지 브로드캐스트

### 3️⃣ 채팅 종료
- 사용자가 채팅방 나가기
- 상대방에게 종료 알림 전송
- 채팅방 및 대기열 정리

---

## 📂 패키지 구조

com.example.randomchat
├─ config
│ └─ WebSocketConfig.kt
├─ controller
│ └─ ChatController.kt
├─ service
│ └─ MatchingService.kt
├─ domain
│ ├─ ChatMessage.kt
│ └─ ChatRoom.kt
└─ RandomChatApplication.kt


---

## 🧩 핵심 도메인 설명

### ChatMessage
```kotlin
data class ChatMessage(
    val type: MessageType,
    val roomId: String,
    val sender: String,
    val content: String
)
```

### ChatRoom
```kotlin
data class ChatRoom(
    val roomId: String,
    val userA: String,
    val userB: String
)
```

---
🔄 메시지 흐름

클라이언트 WebSocket 연결

채팅 시작 요청 → 대기열 등록

매칭 완료 시 roomId 전달

/topic/chat/{roomId} 구독

실시간 메시지 송수신

채팅 종료 시 방 삭제


