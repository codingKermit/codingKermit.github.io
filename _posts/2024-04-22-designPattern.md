---
title : 디자인 패턴
date : 2024-04-12 19:30
categories : [자격증, 정보처리기사]
tags : [정보처리기사]
---

## 디자인 패턴
소프트웨어 공학의 소프트웨어 설계에서 공통으로 발생하는 문제에 대해 자주 쓰이는 설계 방법

## 디자인 패턴의 구성 요소 - 패문솔 사결샘
- 패턴의 이름
- 문제 및 배경
- 솔루션
- 사례
- 결과
- 샘플 코드

## 디자인 패턴의 종류 - 생구행
1. 생성 패턴
2. 구조 패턴
3. 행위 패턴

### 생성 패턴의 종류 - 생빌 프로 팩앱싱
1. 빌더 (Builder) : 인스턴스를 조립하여 만드는 구조
2. 프로토타입 (Prototype) : 원형을 만들고 그것을 복사한 후 필요한 부분만 수정하여 사용
3. 팩토리 메서드 (Factory Method) : 상위 클래스에서 인터페이스 정의, 하위 클래스에서 인스턴스 생성
4. 추상 팩토리 (Abstract Factory) : 서로 연관된 객체들의 조합으로 인터페이스를 제공
5. 싱글톤 (Singleton) : 객체를 하나만 생성하고 어디서든 참조

### 구조 패턴의 종류 - 구 브데 퍼플 프록 컴 어
1. 브릿지(Bridge) : 기능 계층과 구현 계층을 연결하고, 구현부에서 추상 계층을 분리하여 추상화된 부분과 실제 구현 부분을 독립적으로 확장할 수 있는 패턴
2. 데코레이터(Decorator) : 기존에 구현되어 있는 클래스에 필요한 기능을 추가해가는 패턴
3. 퍼사드(Facad) : 
4. 플라이웨이트(Flyweight)
5. 프록시(Proxy)
6. 컴포지트(Composite)
7. 어댑터(Adapter)

### 행위 패턴의 종류
1. 옵저버 (Observer) 
2. 중재자 (Mediator) 
3. 방문자 (Visitor) 
4. 전략 (Strategy)