# Edge_SmartHome_AIoT_Materials
자연어 기반 스마트홈 AIoT 제어 시뮬레이터 - 발표자료 및 프로젝트 자료 저장소
# Edge — 자연어 기반 스마트홈 AIoT 제어 시스템

> Design and Implementation of a Natural Language-Based Smart Home AIoT Control System

복잡한 앱·버튼 대신 **"집이 너무 덥네"** 같은 일상 언어 한 문장으로 스마트홈을 제어하는 AIoT 시스템입니다.
음성을 Whisper STT로 인식하고, Claude(LLM)가 의도를 해석해 검증된 JSON 명령으로 변환한 뒤,
3D 시뮬레이터(React + Three.js)에 가전 동작을 실시간 반영합니다.

---

## 시연 영상

- [시연 영상 보기](https://www.youtube.com/watch?si=sWxcdUNyN71W6f69&v=Sjq9o_rweI0&feature=youtu.be)

## 프로젝트 자료

- [발표자료 보기](./캡스톤디자인_제출용_Edge.pdf)
- [최종 보고서 보기](./Project Final Report_최종본.pdf)

## 관련 레포지토리

- Frontend: https://github.com/Capstone-Edge/Frontend
- Backend: https://github.com/Capstone-Edge/Backend
- Edge: https://github.com/Capstone-Edge/Edge

---

## 핵심 기능

- **자연어 음성 제어** — 버튼/메뉴 없이 일상 언어로 가전 제어 (접근성 향상)
- **Clarification 대화** — 명령이 모호하면 바로 실행하지 않고 되물어 완성
  - 예: `"집이 너무 덥네"` → `"몇 도로 맞춰드릴까요?"` → `"22도"` → 실행
- **DB 기반 명령 검증** — DB 명령 사전(commands + command_parameters)을 MCP 툴 스키마로 LLM에 제공, 안전한 범위 내 JSON 명령만 생성
- **3D 시뮬레이터 실시간 동기화** — 에어컨 온도, TV 콘텐츠, 로봇청소기 이동, 세탁기 상태 등을 WebSocket으로 즉시 반영

---

## 시스템 구성

```
[사용자 음성]
     │
     ▼
[Edge - Jetson Orin Nano]
  Wake-word → VAD → Whisper STT → (HTTP) → Backend
                                   ◀── TTS 응답 출력
     │
     ▼
[Backend - FastAPI]
  의도 분석(Rule + LLM Parser) → DB 명령 검증 → JSON 명령 생성/실행 → 상태 관리
     │
     ▼ WebSocket
[Frontend - React + Three.js]
  3D 룸 시뮬레이터에 가전 상태/애니메이션 실시간 반영
```

3계층 구조(**Edge / Backend / Frontend**)로, 표준화된 JSON 명령 스키마로 모듈 간 연동합니다.

---

## 기술 스택

| 영역 | 기술 |
|------|------|
| Edge / 음성 | Jetson Orin Nano, VAD, Whisper(faster-whisper), gTTS, Wake-word |
| AI / 명령 해석 | Claude API (Haiku), Rule + LLM Parser, MCP 툴 스키마, Clarification |
| Backend | FastAPI, Python, MySQL, REST API, WebSocket |
| Frontend | React, TypeScript, Three.js, Zustand, Blender(GLB) |
| Network | Tailscale VPN (로컬 실행) |

---

## 팀 구성 (엣지 / Edge)

| 이름 | 역할 |
|------|------|
| 강민수 | AI / NLU — 명령 파서, JSON 스키마, Clarification 로직, DB 명령 정의 |
| 노현수 | Backend — FastAPI 서버/API, DB 모델·상태 관리, WebSocket 동기화 |
| 손건영 | Frontend — React 대화형 UI, 3D 시뮬레이터, 디바이스 애니메이션 |
| 황지민 | Edge AI — 음성 입출력, Whisper STT, Wake-word, TTS, 백엔드 연동 |
