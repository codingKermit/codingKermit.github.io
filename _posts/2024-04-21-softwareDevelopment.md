---
title : 소프트웨어 개발
date : 2024-04-12 19:30
categories : [자격증, 정보처리기사]
tags : [정보처리기사]
---

안녕하세요 개발구리입니다🐸.

저는 소프트웨어 개발 과목에서는 단순 암기를 하려하면 했갈리는게 많더군요
생명주기 모델 종류도 있고 개발 방법론 종류도 있고 종류별로 원리, 가치 등도 종류가 너무 많구...
그나저나 저도 마크다운 문법이 있다는게 기억나서 이번엔 마크다운 문법으로 써보려구요

## 소프트웨어 생명주기(SDLC; Software Development Life Cycle) 모델
시스템의 요구분석부터 유지보수까지 전 공정을 체계화한 절차

## 소프트웨어 생명주기 모델 프로세스 - 요설구테유
1. **요**구사항 분석 : 개발할 S/W의 조건, 목표를 정의
2. **설**계 : 수행 방법을 논리적으로 결정
3. **구**현 : 실제 프로그램 작성
4. **테**스트 : 요구 사항을 만족하는지 검사하고 평가
5. **유**지보수 : 시스템이 인수된 후의 모든 활동

## 소프트웨어 생명주기 모델 종류 - 폭프나반    
- [**폭**포수 모델 (Waterfall Model)](#폭포수-모델)
- [**프**로토타이핑 모델 (Prototyping Model)](#프로토타이핑-모델)
- [**나**선형 모델 (Spiral Model)](#나선형-모델)
- [**반**복적 모델 (Iteration Model)](#반복적-모델)

### 폭포수 모델
소프트웨어 개발 시 각 단계를 확실히 마무리 지은 후에 다음 단계로 너머가는 모델
- 폭포수 모델의 절차
  1. 타당성 검토
  2. 계획
  3. 요구사항 분석
  4. 설계
  5. 구현
  6. 테스트
  7. 유지보수

### 프로토타이핑 모델
고객이 요구한 주요 기능을 프로토타입으로 구현하여, 고객의 피드백을 반영하여 소프트웨어를 만들어가는 모델

### 나선형 모델
시스템 개발 시 위험을 최소화하기 위해 점진적으로 개발하는 모델
- 나선형 모델 절차
  1. 계획 및 정의
  2. 위험 분석
  3. 개발
  4. 고객 평가

### 반복적 모델
구축 대상을 나누어 병렬적으로 개발하고 통합하거나, 반복적으로 개발하여 완성시키는 모델

## 소프트웨어 개발 방법론 종류 - 구정객컴애제
- [소프트웨어 생명주기(SDLC; Software Development Life Cycle) 모델](#소프트웨어-생명주기sdlc-software-development-life-cycle-모델)
- [소프트웨어 생명주기 모델 프로세스 - 요설구테유](#소프트웨어-생명주기-모델-프로세스---요설구테유)
- [소프트웨어 생명주기 모델 종류 - 폭프나반](#소프트웨어-생명주기-모델-종류---폭프나반)
  - [폭포수 모델](#폭포수-모델)
  - [프로토타이핑 모델](#프로토타이핑-모델)
  - [나선형 모델](#나선형-모델)
  - [반복적 모델](#반복적-모델)
- [소프트웨어 개발 방법론 종류 - 구정객컴애제](#소프트웨어-개발-방법론-종류---구정객컴애제)
  - [구조적 방법론(Structured Development)](#구조적-방법론structured-development)
  - [정보공학 방법론(Information Engineering Development)](#정보공학-방법론information-engineering-development)
  - [객체 지향 방법론(OOD; Object-Oriented Development)](#객체-지향-방법론ood-object-oriented-development)
    - [객체 지향 분석 (OOA; Object Oriented Analysis)](#객체-지향-분석-ooa-object-oriented-analysis)
      - [자료흐름도(DFD; Data Flow Diagram)](#자료흐름도dfd-data-flow-diagram)
  - [컴포넌트 기반 방법론 (CBD; Component-Based Development)](#컴포넌트-기반-방법론-cbd-component-based-development)
  - [애자일 방법론(Agile Development)](#애자일-방법론agile-development)
    - [XP(eXteam Programming)](#xpexteam-programming)
    - [SCRUM](#scrum)
    - [린(Lean) 방법론](#린lean-방법론)
- [키워드 정리](#키워드-정리)

### 구조적 방법론(Structured Development)
전체 시스템을 기느엥 따라 나누어 개발하고, 이를 통합하는 분할과 정복 접근 방식 방법론

### 정보공학 방법론(Information Engineering Development)
정보시스템 개발에 필요한 관리 절차와 작업 기법을 체계화한 방법론

### 객체 지향 방법론(OOD; Object-Oriented Development)
실세계의 개체를 속성과 메서드가 결합한 형태의 객체로 표현하는 기법
- 객체 지향 기법 - 캡상다추정관
  1. **캡**슐화
  2. **상**속성
  3. **다**형성
  4. **추**상화
  5. **정**보은닉
  6. **관**계성
- 객체 지향 설계 원칙 - SOLID
  1. 단일 책임의 원칙(SRP; Single Responsibility Principle)
  2. 개방 폐쇠 원칙(OCP; Open Close Principle)
  3. 리스코프 치환의 원칙 (LSP; Liskov Subsitution Principle)
  4. 인터페이스 분리의 원칙 (ISP; Interface Segregation Principle)
  5. 의존성 역전의 원칙 (DIP; Dependency Inversion Principle)

#### 객체 지향 분석 (OOA; Object Oriented Analysis)
사용자의 요구사항을 분석하여 요구된 문제와 관련된 모든 객체, 속성, 연산, 관계를 정의하여 모델링하는 기법
- 객체 지향 분석 방법론 종류
  1. 야콥슨(Jacobson) 방법론 : Usecase를 강조
  2. 럼바우(Rumbaugh) 방법론 - 객동기
     1. 객체 모델링(Object Modeling) = 정보 모델링(Information Modeling) : E-R 다이어그램 산출
     2. 동적 모델링(Dynamic Modeling) : 상태 다이어그램을 활용하여 표현
     3. 기능 모델링(Functional Modeling) : 자료흐름도(DFD; Data Flow Diagram) 산출
  3. 부치(Booch) 방법론 : 미시적 개발, 거시적 개발 프로세스 사용
  4. Coad와 Yourdon 방법론 : E-R 다이어그램을 사용
  5. Wirfs-Brock 방법론 : 분석과 설계 간의 구분이 없음

##### 자료흐름도(DFD; Data Flow Diagram)
- 자료흐름도(DFD; Data Flow Diagram)의 구성요소
  1. Data flow : 데이터 흐름 표시
  2. Process : 동작 표현
  3. Data Store : 저장될 데이터 표현
  4. Terminator
- 자료흐름도(DFD; Data Flow Diagram) 작성 이후에 작성하는 요소
  - 데이터 사전(DD; Data Dictionary) : DFD에서의 데이터 상세를 명시
  - 소단위명세서(Mini Spec) : DFD에서의 Process 상세를 명시

### 컴포넌트 기반 방법론 (CBD; Component-Based Development)
소프트웨어를 구성하는 컴포넌트를 조립하여 새로운 응용 프로그램을 만드는 방법론.  
- 개발 기간 단축
- 쉬운 확장성
- 재사용 용이

### 애자일 방법론(Agile Development)
절차보다는 사람 중심이 되어 변화에 유연하고 신속하게 적응하면서 효율적으로 개발하는 방법론.

#### XP(eXteam Programming)
의사소통 개선과 즉각적 피드백으로 소프트웨어 품질을 높이기 위한 방법론
- XP의 5가지 가치 - 용단의 피존
  1. **용**기(Courage)
  2. **단**순성(Simplicity)
  3. **의**사소통(Communication)
  4. **피**드백(Feedback)
  5. **존**중(Respect)
- XP의 12가지 기본원리
  1. 짝 프로그래밍(Pair Programming) : 개발자 둘이서 짝으로 코딩
  2. 공동 코드 소유(Collective Ownership) : 시스템 내 코드는 누구나 수정 가능
  3. 지속적인 통합(CI; Continuous Integration)
  4. 계획 세우기(Planning Process)
  5. 작은 릴리즈(Small Release)
  6. 메타포어(Metaphor) : 공통의 이름 쳬게를 통해 개발자와 고객간 의사소통 원활화
  7. 간단한 디자인(Simple Design)
  8. 테스트 기반 개발(TDD; Test-Driven Development)
  9. 리팩토링(Refactoring) : 프로그램의 기능을 바꾸지 않으면서 시스템을 재구성
  10. 40시간 작업(40-Hour Work)
  11. 고객 상주(On Site Customer)
  12. 코드 표준(Coding Standard)

#### SCRUM
매일 정해진 시간, 장소에서 짧은 시간의 개발을 하는 팀을 위한 프로젝트 관리 중심 방법론

#### 린(Lean) 방법론
도요타의 린 시스템 품질기법을 소프트웨어 개발 프로세스에 적용한 방법론

## 키워드 정리
- 소프트웨어 생명주기 모델 프로세스 - 요설구테유
  1. 요구사항 분석 
  2. 설계 : 수행 방법 논리적 결정
  3. 구현
  4. 테스트
  5. 유지보수
- 폭포수 모델 : 각 단계가 마무리 되어야 다음 단계로 진행
- 프로토타이핑 모델 : 요구사항을 프로토타입으로 만들어 피드백 반영하며 지속적 개발
- 나선형 모델 : 위험 최소화를 위해 점진적 개발
- 반복적 모델 : 병렬적 개발 후 통합
- 나선형 모델 절차 - 계위개고
  1. 계획
  2. 위험분석
  3. 개발
  4. 고객평가
- 구조적 방법론 : 시스템을 기능에 따라 나누어 개발, 분할과 정복 접근 방식
- 정보공학 방법론 : 정보시스템 개발에 필요한 관리 절차를 쳬게화
- 객체 지향 방법론 : 객체라는 단위로 시스템을 분석
- 컴포넌트 기반 방법론 : 컴포넌트를 조립
- 애자일 방법론 : 절차보다는 사람에 집중하고 유연
- 제품 계열 방법론 : 특정 제품에 적용하고 싶은 공통 기능을 정의
- XP의 5가지 가치 - 용단의 피존
  1. 용기(Courage)
  2. 단순성(Simplicity)
  3. 의사소통(Communication)
  4. 피드백(Feedback)
  5. 존중(Respect)
- XP의 12가지 기본원리
  1. 짝 프로그래밍
  2. 공동 코드 소유
  3. 지속적인 통합
  4. 계획 세우기
  5. 작은 릴리즈
  6. 메타포어
  7. 간단한 디자인
  8. 테스트 기반 개발
  9. 리팩토링
  10. 40시간 작업
  11. 고객 상주
  12. 코드 표준
- SCRUM : 매일 정해진 시간, 장소에서 짧은 시간 개발
- 린 방법론 : 도요타의 린 시스템을 소프트웨어 개발에 적용
- 객체 지향 기법 - 캡상다추정관
  1. 캡슐화
  2. 상속성
  3. 다형성
  4. 추상화
  5. 정보은닉
  6. 관계성
- 객체 지향 설계 원칙 - SOLID
  1. 단일 책임 원칙 (SRP)
  2. 개방 폐쇠 원칙 (OCP)
  3. 리스코프 치환의 원칙 (LSP)
  4. 인터페이스 분리의 원칙 (ISP)
  5. 의존성 역전의 원칙 (DIP)
- 럼바우 모델링 - 객동기
  1. 객체 모델링(정보 모델링)
  2. 동적 모델링
  3. 기능 모델링

공부하면서 문득 생각난 것은 많은 자료에서 애자일 방법론과 폭포수 모델을 대비하여 설명해주는데, 폭포수 모델은 생명주기 모델의 분류에 속하고 애자일 방법론은 개발 방법론 분류에 속하니까 엄연히 분류 자체가 다른데 이를 서로 비교하는게 자칫 오히려 했갈리게 하기도 하더군요.  
특성을 보면 확실히 둘이 대비되는 개념이라서 비교해서 설명해주기 딱 좋긴 한데 말이죠 흠🤔  
원래는 소프트웨어 생명주기 모델과 개발 방법론을 따로 작성하려 했으나 이 둘 때문에 같이 쓰다보니 글이 길어졌네요 
소프트웨어 너무 어려워요😢  
시스템의 동작 원리 같은게 아니라 방법론이니까 암기하는 수 밖에 없잖아요
![](/assets/img/kermit/crying%20kermit.jpg)