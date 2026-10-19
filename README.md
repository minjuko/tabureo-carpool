# 타부러

[![Rails CI](https://github.com/minjuko/tabureo-carpool/actions/workflows/ci.yml/badge.svg)](https://github.com/minjuko/tabureo-carpool/actions/workflows/ci.yml)

> **호남지역의 이동 불편을 개선하기 위해 기획한 모바일 웹 기반 카풀 서비스**

제2회 OASIS 해커톤에서 4명이 함께 만든 프로젝트입니다.

출발지와 목적지가 비슷한 사용자가 함께 이동할 수 있도록 카풀 탐색·등록·상세 조회·참여 기능을 구현하고, **TMAP 경로 API**를 활용해 출발지와 도착지 사이의 이동 경로를 지도에 시각화했습니다.

## 프로젝트 개요

| 항목 | 내용 |
| --- | --- |
| 개발 기간 | 2021.08.09 ~ 2021.08.20 |
| 프로젝트 | 제2회 OASIS 해커톤 |
| 팀 구성 | 4명 |
| 담당 역할 | Frontend |
| 서비스 형태 | 모바일 웹 |

## 주요 기능 및 화면

| **카풀 목록** | **카풀 등록** | **카풀 상세 · 경로 조회** |
| :---: | :---: | :---: |
| <img width="200" alt="카풀 목록 화면" src="docs/images/carpool-list.PNG"> | <img width="200" alt="카풀 등록 화면" src="docs/images/carpool-new.PNG"> | <img width="200" alt="카풀 상세 및 TMAP 경로 조회 화면" src="docs/images/carpool-tmap-detail.PNG"> |
| 등록된 카풀 목록 조회 및 출발지·도착지·날짜 조건을 통한 카풀 탐색 | 출발지·목적지·출발 시간 등 카풀 모집에 필요한 정보 등록 | 카풀 모집 정보 확인 및 TMAP을 활용한 출발지·도착지 이동 경로 조회 |

- 카풀 목록 조회 및 조건 검색
- 카풀 등록 및 상세 조회
- 카풀 참여·취소 및 모집 상태 관리
- 로그인·회원가입·프로필 관리
- TMAP 기반 출발지·도착지 및 이동 경로 확인

## 기술 스택

| 구분 | 기술 |
| --- | --- |
| Frontend | HTML5, CSS3, JavaScript, jQuery |
| Backend / Template | Ruby on Rails 6.1, ERB, Devise |
| Database | SQLite |
| External API | TMAP JavaScript API, TMAP 경로 탐색 API |
| Collaboration | Slack, Git, GitHub |

## 담당 역할 및 핵심 구현

- HTML/CSS 기반 모바일 웹 UI 구현
- 사용자 **프로필 화면 UI 및 CSS 직접 구현**
- Frontend 팀원들과 카풀 탐색·등록·상세 화면 공동 구현
- TMAP 경로 API 연동 및 지도 시각화 공동 작업
- Rails ERB View와 Frontend 화면 통합 참여
- Slack 및 Git/GitHub 기반 4인 팀 협업

### 1. 모바일 카풀 서비스 UI

HTML5, CSS3, JavaScript, jQuery를 활용해 모바일 환경에 맞춘 **카풀 탐색·등록·상세 조회 및 사용자 프로필 화면**을 구현했습니다.

초기 정적 Frontend 화면은 `FE/`에서 확인할 수 있으며, 일부 화면은 Rails의 ERB View에 적용하여 서버 데이터와 사용자 상태가 화면에 반영되도록 통합했습니다.

### 2. TMAP 경로 API 연동

Frontend 팀원과 함께 **TMAP JavaScript API와 경로 탐색 API**를 연동했습니다.

경로 탐색 결과의 `features[]`에서 거리·시간·경로 좌표를 처리하고, EPSG3857 경로 좌표를 WGS84 좌표로 변환한 뒤 Marker와 Polyline을 생성하여 이동 경로를 지도에 시각화했습니다.

```text
출발 · 도착 좌표 (WGS84)
        ↓
TMAP 경로 API 요청
        ↓
Response / features[]
        ↓
거리 · 시간 · 경로 좌표 추출
        ↓
EPSG3857 → WGS84 좌표 변환
        ↓
Marker · Polyline 생성
        ↓
지도 시각화
```

> **구현 범위** — 시연용 출발·도착 좌표(WGS84)를 기준으로 경로 탐색 및 지도 시각화를 구현했으며, 사용자 입력 주소의 좌표 변환은 구현 범위에 포함하지 않았습니다.

### 3. Rails View 통합

HTML/CSS/JavaScript로 제작한 정적 화면을 Rails의 **ERB View에 적용**하고, `@ride`, `current_user` 등의 서버 데이터를 활용하여 카풀 정보와 사용자 상태에 따른 화면을 구성했습니다.

Rails Route 및 Form과 화면을 연결하면서 Frontend와 Backend가 하나의 사용자 흐름으로 동작하도록 통합했습니다.

## 트러블슈팅

### TMAP 경로 요청 실패에 대한 화면 처리

- **문제:** 외부 경로 요청이 지연되거나 빈 응답을 반환할 때 경로를 표시할 수 없었습니다.
- **해결:** 경로 요청을 비동기로 처리하고 시간 초과·빈 응답·실패 상황을 안내하도록 보완했습니다.
- **검증:** Rails 테스트 21건·55 assertions 및 CI를 확인했습니다. 실제 TMAP 호출 검증은 포함하지 않았습니다.

## 시작하기 (Getting Started)

Ruby 2.7 · Rails 6.1 · SQLite 기반의 해커톤 프로젝트입니다. 의존성 설치 후 Rails 서버를 실행합니다.

```bash
bundle install
bundle exec rails db:migrate
bundle exec rails server
```


## 테스트 및 코드 품질

기존 사용자 흐름을 유지하면서 권한 검증과 외부 경로 요청의 예외 처리를 보완했습니다.

- 카풀 수정·삭제를 작성자만 가능하도록 제한
- 댓글 작성자를 서버의 `current_user`로 지정하고, 댓글 삭제를 댓글 작성자 또는 카풀 작성자로 제한
- TMAP 경로 요청을 비동기로 전환하고 시간 초과·빈 응답·실패 안내 추가
- 잘못된 날짜 검색을 빈 결과로 처리

| 검증 | 결과 |
| --- | --- |
| Rails 테스트 | 21 runs · 55 assertions · 0 failures / 0 errors / 0 skips |
| Frontend 자산 | Webpack production pack 9개 · 약 413KB |
| GitHub Actions | Ruby 2.7 · Node 16 · Rails 테스트 · production asset precompile |

TMAP API의 실제 호출은 검증 범위에 포함하지 않았습니다.

## 산출물 및 관련 문서

- [원본 프로젝트](https://github.com/minjuuko/2021Hackathon)
