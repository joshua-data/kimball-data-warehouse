# 📚 다차원 모델링 프로세스와 태스크 - 학습 노트

## 🧭 1. 개요: 왜 다차원 모델링 프로세스가 중요한가?

Kimball의 다차원 모델링은 단순한 테이블 설계를 넘어서, **비즈니스 요건을 효과적으로 반영**하고, **확장 가능하고 분석 친화적인 데이터 구조**를 만드는 것이 핵심입니다.

이 장에서는 다음을 다룹니다:

- 다차원 모델링 프로세스의 전체 흐름
- 모델링 단계별 전략
- 참여자(특히 현업 대표)의 중요성
- 실제 설계 산출물과 고려할 사항들

---

## 🧩 2. 다차원 모델링 프로세스 개요

### ✅ 준비 단계

| 단계 | 설명 |
| --- | --- |
| 현업 대표 및 데이터 담당자 확보 | 실무를 잘 아는 사람들과 협력해야 정확한 모델 생성 가능 |
| 예비 버스 매트릭스, 상세 비즈니스 요건 확보 | 설계 방향성을 결정할 핵심 입력값 |
| 초기 모델 초안 도출 | 팩트 테이블 그레인, 연관 디멘션, 설계 범위 식별 |
| 상위 설계 → 상세 설계 | 디멘션 속성, 도메인 값, 데이터 원천, 품질 이슈 등 세분화 |
| 팩트 테이블 설계 | 디멘션이 먼저 정리된 후 팩트 테이블 설계 |
| 리뷰 및 검토 | 현업과 함께 비즈니스 요건 충족 여부 재검토 |

> 💡 반복적이고 점진적인 프로세스라는 점을 명심하세요. 설계 → 검토 → 수정이 여러 번 반복됩니다.
> 

### ⏳ 소요 기간

- 보통 **3~4주 소요**
- 팀 숙련도, 데이터 복잡도, 현업 참여도, 표준 디멘션 재사용 여부에 따라 달라짐

---

## 🏗️ 3. 조직 구성 및 참여자 식별

### 핵심 원칙: "혼자 하지 말고, 함께 설계하라"

| 역할 | 책임 및 중요성 |
| --- | --- |
| 데이터 모델러 | 설계 리더, 구조화 및 문서화 담당 |
| 현업 대표 (SME) | 실제 업무 흐름, KPI, 비즈니스 규칙 설명 |
| 원천 시스템 관리자 | 소스 시스템 구조와 데이터 특성 설명 |
| ETL/DBA 담당자 | 실현 가능한 ETL 전략 및 기술 검토 |
| 데이터 오너 | 데이터 정의, 품질 관리, 조직 내 합의 유도 |

> 🧠 3차 정규화 관점의 유혹, ETL 단순화를 위한 타협은 주의해야 할 함정입니다.
> 

### 조직 전략: 데이터 담당 오너십

- **표준 디멘션 전략**과 연계되어야 함
- DW/BI가 일관성을 가지려면 **공통된 정의와 속성명**에 대한 합의 필요

> ⚠️ 많은 조직에서 가장 어려운 부분은 기술이 아니라 조직 내 합의와 커뮤니케이션입니다.
> 

---

## 📋 4. 모델링 산출물

### 1️⃣ 예비 입력

- 예비 버스 매트릭스
- 상세한 비즈니스 요건 정의서

### 2️⃣ 핵심 산출물

| 산출물 | 설명 |
| --- | --- |
| 상위 단계 다차원 모델 | 초기 설계 방향 결정 |
| 상세 디멘션 설계 | 속성, 도메인, 관계, 데이터 품질 고려 |
| 상세 팩트 테이블 설계 | 측정값, 외래키, 그레인 정리 |
| 이슈 로그 | 설계 중 발견된 질문, 결정해야 할 사항 기록 |

---

## 🧠 5. 전략적 팁 요약

- **현업과 협업** 없이는 의미 있는 모델 설계는 불가능하다.
- **표준 디멘션 정의**는 어렵지만 필수다.
- 모델링은 반복적인 과정이며, 초기 설계만으로 충분하지 않다.
- ETL 편의를 위해 BI 분석이 복잡해지지 않도록 균형을 맞춰야 한다.
- 외부 전문가가 참여하더라도 설계팀이 **직접 설계 과정에 참여하며 학습**해야 다음 단계도 주도 가능하다.

---

# 📘 Kimball 다차원 모델링 학습 노트

> 출처: 『The Data Warehouse Toolkit』 Chapter 18 요약
> 

---

## ✅ 다차원 모델 설계의 4가지 핵심 결정사항

1. **비즈니스 프로세스 식별**
2. **비즈니스 프로세스의 그레인(grain) 선언**
3. **디멘션(dimension) 식별**
4. **팩트(fact) 식별**

> 이 네 가지는 설계 프로세스의 기준이 되며, 설계는 반복적이며 점진적으로 정제됨.
> 

---

## 1️⃣ 상위 수준 다차원 모델 설계 (버블 차트)

### 🎯 목적

- 초기 요구사항을 바탕으로 비즈니스 프로세스를 시각화
- 팩트 테이블과 디멘션 테이블 간의 관계 구조를 개략적으로 도식화

### 📌 절차

- **버스 매트릭스**로부터 시작
- **버블 차트 (Bubble Chart)** 생성
    - 중심: 팩트 테이블
    - 주변: 디멘션 테이블
- 설계팀 전원이 참여하여 의견 수렴

### 🧠 주요 고려사항

- **팩트 테이블의 그레인 선언**이 디멘션 식별에 직접적인 영향을 미침
- 디멘션이 그레인과 맞지 않을 경우:
    - 디멘션 제외
    - 팩트 그레인 조정
    - 다중값 설계 고려

### 📎 장점

- 전체 설계팀과 관련자 간의 공통 이해 형성
- 범위 조율, 설계 방향 정렬 가능
- 향후 문서화/발표 자료로도 활용 가능

---

## 2️⃣ 상세 다차원 모델 설계

### 🧩 설계 순서

1. 디멘션 테이블 설계
2. 팩트 테이블 설계

### 📌 추천 접근

- 단순한 디멘션부터 시작 (ex: 날짜 디멘션)
- 점진적 학습과 팀 협업을 위한 성공 경험 확보

---

## 📂 디멘션 설계

### 💡 목표

- 디멘션 속성 및 의미 정의
- 기업 표준 디멘션으로 정의

### 📋 고려 사항

- 명명 규칙(Naming Conventions) 정의 시도
- 최종 명명은 **현업 사용자와 합의** 필요
- 경우에 따라 **정크 디멘션** 또는 **미니 디멘션** 고려

> ⚠️ 현업의 용어와 정의에 대한 동의를 얻는 데 시간이 걸릴 수 있음.
> 
> 
> → 데이터 거버넌스를 위한 위원회 구성도 검토 가능
> 

---

## 📊 팩트 설계

### 💡 목표

- 그레인에 맞는 수치 정보 식별

### 📋 고려 사항

- **기초 팩트 (measurable events)**: 횟수, 금액 등
- **파생 팩트**: 기초 팩트로부터 계산된 수치 (예: 평균, 비율)

> 데이터 프로파일링을 통해 원천 시스템의 이벤트 구조 분석 필요
> 

---

## 🕰️ 디멘션 이력 관리 (Slowly Changing Dimension)

### 📌 절차

- 디멘션 속성별 변경 발생 시 처리 방식 결정
    - Type 0~7 중 적절한 기법 선택
- 원천 시스템 데이터 변경 패턴 분석

> 💡 원천 데이터 변경이 보정인지, 실질 변화인지 판단 → 원천 시스템 담당자와 협업 필수
> 

---

## 📄 설계 산출물: 상세 설계 문서

### 📑 설계 문서 구성

- 각 테이블의 컬럼명, 설명, 정의
- 출처 시스템 및 ETL 로직 정보 포함
- 향후 ETL 개발 및 QA 기준이 됨

### 🔗 자료 위치

- [KimballGroup.com](https://www.kimballgroup.com/)
    - 『The Data Warehouse Lifecycle Toolkit (2nd Ed.)』 > Tools and Utilities 탭

---

## 🔁 설계는 반복적인 프로세스

- 요구사항 변경, 원천 시스템 분석 결과에 따라 지속적으로 **수정·보완**
- 설계팀은 설계와 동시에 현업과 **계속 협의**하고, **원천 시스템 프로파일링**을 병행

---

## ✍️ 정리: 다차원 모델 설계 흐름 요약

```
[비즈니스 요구사항 수집]
    ↓
[비즈니스 프로세스 식별 및 그레인 선언]
    ↓
[버스 매트릭스 및 버블 차트 설계]
    ↓
[디멘션 상세 설계 → 팩트 상세 설계]
    ↓
[이력 관리 기법 적용]
    ↓
[설계 문서 작성 및 리뷰]
    ↓
[지속적인 개선과 반복]
```

---

## 🌱 실무 적용 팁

- 설계 초기에는 지나치게 복잡하게 시작하지 말 것 → 단순한 모델부터 점진적으로 확장
- 설계자 혼자 결정하지 말고 항상 **현업 사용자와 협업**
- 명명 규칙, 정의, 수치 정보 등은 **문서화 필수**
- 반복 설계에 유연하게 대응할 것