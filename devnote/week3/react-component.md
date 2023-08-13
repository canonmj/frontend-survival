# 컴포넌트 계층구조

<!-- ## 선언형 프로그래밍

## 명령형 프로그래밍 -->

## SRP (단일 책임 원칙)

> 모든 클래스는 각각 하나의 변경 원인,근거(책임)를 가지고, 그것을 캡슐화(속성 행위를 묶어서 외부에 보이지 않게 숨김)해야 한다.

- React의 경우에는 관련 있는 UI/작업 들만 component 로 묶어버린다.
- IA 구조에 따라 API에서 받아오는 구조화된 데이터(JSON Schema)를 적용해 컴포넌트 계층을 나눈다. 나누는 과정에서 관련 있는 데이터 끼리 묶게 되다보면 SRP 원칙이 자연스럽게 녹아든다.

### DRY 원칙

Don't repeat yourself

정보의 반복을 줄이는 것을 목표로 하는 소프트웨어 개발의 기본 원칙

### SSOT(Single Source of Truth)

단일 진실 공급원(SSOT)은 정보와 스키마를 오직 하나의 출처에서만 생성 또는 편집하도록 하는 방법론이다.

단일 출처를 통해 데이터를 생성하고 편집하고 접근하므로 데이터의 정합성을 지키고 잘못된 데이터 유통을 방지하고 모두가 동일한 데이터를 참고하도록 하고 있다.

- [DRY (Don’t Repeat Yourself)](https://ko.wikipedia.org/wiki/중복배제)
- [SSOT (Single Source of Truth)](https://ko.wikipedia.org/wiki/단일_진실_공급원)
