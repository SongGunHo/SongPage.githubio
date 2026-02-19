---
title: File Common Module
subtitle: 대용량 파일 업로드 및 관리 공통 모듈 설계 (Back-end)

image: https://raw.githubusercontent.com/BlackrockDigital/startbootstrap-agency/master/src/assets/img/portfolio/03-full.jpg
alt: 파일 업로드 공통 모듈

caption:
  title: 파일 업로드/다운로드 공통 모듈
  subtitle: Back-end / File System
  thumbnail: https://raw.githubusercontent.com/BlackrockDigital/startbootstrap-agency/master/src/assets/img/portfolio/03-thumbnail.jpg
---

## 프로젝트 개요
- **구분:** 팀 프로젝트
- **인원:** 6명
- **Role:** 파일 업로드/다운로드 공통 모듈 설계 및 구현
- **목표:** 중복 방지, 대용량 처리, 확장 가능한 파일 관리 구조 구축

---

## 주요 기능

### 🔹 파일명 중복 방지
- 업로드 전 파일 정보를 DB에 저장
- 생성된 **증감번호(seq)** 기반으로 서버 저장 파일명 변경
- 동일 파일명 업로드 시 덮어쓰기 문제 해결
- 다운로드 시에는 **원본 파일명 유지**

---

### 🔹 파일 저장 분산 구조
- 단일 폴더 과부하 방지
- `seq % 10` 방식으로 디렉토리 분산 저장
예시:
👉 조회 성능 저하 문제 예방

---

### 🔹 파일 그룹 관리 (gid)
- 게시글 1개 = 파일 그룹
- 상품 1개 = 파일 그룹
- 파일 일괄 관리 및 조회 구조 설계

---

### 🔹 파일 위치(location) 분리
용도별 파일 관리:

- 에디터 이미지 → `editor`
- 일반 첨부파일 → `file`
- 상품 메인 이미지 → `main`
- 상품 목록 이미지 → `list`
- 상세 설명 이미지 → `description`

👉 파일 목적 기반 관리 구조 확립

---

### 🔹 업로드 사용자 자동 기록
- `@CreatedBy` 적용
- `AuditorAware` 인터페이스 구현
- 로그인 사용자 정보 자동 저장

👉 파일 생성 책임 추적 가능

---

### 🔹 파일 다운로드 처리
- 응답 헤더 설정:

Content-Disposition: attachment; filename=원본파일명


- 서버 저장명과 관계없이
  👉 사용자에게는 원본 파일명으로 다운로드 제공

---

### 🔹 썸네일 자동 생성
썸네일 경로 규칙:

uploads/thumbs/{폴더}/{너비}{높이}{seq}.jpg


최적화 전략:

- 동일 크기 썸네일 존재 시 재생성 ❌
- 필요 시에만 생성 ✔

👉 서버 리소스 절약

---

## 문제 해결 성과
- 파일명 충돌로 인한 덮어쓰기 문제 해결
- 대용량 파일 환경에서 디렉토리 조회 성능 개선
- 파일 용도별 관리 체계 구축으로 유지보수성 향상
- 공통 모듈화로 다양한 도메인에서 재사용 가능 구조 확보