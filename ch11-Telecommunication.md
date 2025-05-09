# 📘 Chapter 11: 통신 - 설계 검토를 통한 다차원 모델 개선

## 🔍 1. 장 개요 및 학습 목표

- **이 장은 모델링 설계의 오류를 진단하고 개선하는 데 중점을 둔다.**
- 새로운 모델링을 ‘처음부터’ 하지 않으며, **기존 스키마의 검토와 향상이 핵심**이다.
- **통신 업계의 고객 청구 프로세스**를 중심으로 사례가 진행된다.
- 마지막에는 **지리적 위치 데이터**의 모델링 방안도 간단히 다룬다.

---

## 🎯 2. 학습 포인트 요약

| 항목 | 설명 |
| --- | --- |
| ✅ 버스 매트릭스 일부 설계 | 통신사의 전체 비즈니스 프로세스를 행/열 구조로 표현 |
| ✅ 설계 검토 실습 | 기존 다차원 모델의 설계 오류 파악 및 개선 제안 |
| ✅ 자주 발생하는 설계 실수 | 설계자들이 흔히 저지르는 문제 체크리스트 제공 |
| ✅ 설계 개선 전략 | 설계를 강화하는 구체적인 방법 제시 |
| ✅ 지리적 위치 데이터 | 주소, 도시, 국가 등 추상적 지리 데이터 모델링 팁 |

---

## 🧪 3. 사례 시작: 통신사 청구 시스템

### 💼 프로젝트 배경

- 독자가 통신사 DW/BI팀의 다차원 모델러로 **승진한 가상의 시나리오**로 시작됨.
- 현업과 IT로 구성된 **후원위원회**가 강력하게 프로젝트를 뒷받침 중.
- 프로젝트 초기 단계에서 작성된 **버스 매트릭스**와 **팩트/디멘션 스키마 초안**이 주어짐.

### 🧾 비즈니스 요구사항

- 분석 대상: **고객 청구 데이터**
- **청구 메트릭**: 월 사용량(통화 분수, 문자 수, 데이터 사용량), 월 요금, 청구 금액 등
- **분석 축**: 고객, 요금제, 영업 조직 및 채널

### 🧠 설계 방향 결정

- **후원위원회**: 청구 데이터를 우선시 (업무적 가치와 실현 가능성 때문)
- **일부 IT 의견**: 통화 상세 데이터(CDR)를 먼저 모델링하자는 주장 (복잡성과 비용 우려로 기각)

---

## 📐 4. 통신 청구 모델 개요

### 🔗 관계 요약

- 하나의 고객 → 여러 회선(Line)
- 하나의 청구서(Bill) → 여러 회선 포함
- 회선별 → 요금제, 영업조직, 사용량 메트릭 존재

### 📊 팩트 테이블: 월 청구서 요약 (청구서별 1 레코드)

- 팩트: 총 통화 시간, 문자 수, 데이터 사용량, 청구 금액
- 디멘션:
    - 고객(Customer)
    - 요금제(Rate Plan)
    - 영업조직(Sales Org)
    - 시간(Time)
    - 지리 정보(Location)
    - 회선(Line) → 가능하면 디멘션으로, 아니면 팩트의 속성

---

## 🧩 5. 여러분의 역할: 설계 검토자

> 팀은 현재 설계 초안이 "잘 되어 있다"고 생각하며, 여러분의 리뷰를 요청합니다.
> 

**검토 포인트 예시 (스스로 생각해볼 수 있는 것들)**:

- ✔️ 팩트 테이블의 **그레인(Granularity)** 적절한가?
- ✔️ **역할을 구분해야 할 디멘션**이 있는가? (예: 시작 vs 종료 지점의 지역 등)
- ✔️ 디멘션이 충분히 **정규화되어 있는가?**
- ✔️ 팩트 속에 존재하는 **디멘션 후보 속성**이 있는가?
- ✔️ 분석에 유용한 **충분한 파생 컬럼/지표**가 포함되어 있는가?

# 📘 설계 검토 시 일반적 고려 사항

---

## 1️⃣ 업무요건과 원천 데이터 사이의 균형 잡기

- **핵심 메시지**: 다차원 모델은 *업무 요건*과 *원천 데이터*를 동시에 고려하여 설계해야 함.
- **실행 팁**:
    - 요구사항을 수집하면서 동시에 원천 데이터를 프로파일링할 것.
    - 요구사항만 보면 데이터가 없을 수도 있고, 데이터만 보면 중요한 비즈니스 속성이 누락될 수 있음.

---

## 2️⃣ 업무 프로세스에 집중할 것

- **핵심 메시지**: 모델은 *보고서 기반*이 아니라 *업무 프로세스 기반*으로 설계해야 한다.
- **구체 전략**:
    - 특정 보고서나 질문에 맞춘 모델은 시간이 지나면 무용지물이 될 수 있음.
    - 중심이 되는 프로세스 기반의 모델은 다양한 질문에 유연하게 대응 가능.
- **보완 모델**:
    - 요약 집계 테이블
    - 점진적 스냅샷
    - 통합 팩트 테이블 (여러 프로세스의 동일한 그레인 결합)
    - 부분집합 팩트 테이블 (보안 또는 특정 부서 용도)

---

## 3️⃣ 팩트 테이블의 그래뉼래러티(Grain)

- **핵심 메시지**: 팩트 테이블의 "그레인(grain)"을 명확히 정의하고 합의할 것.
- **중요성**:
    - 명확한 그레인이 없으면 설계가 계속 표류함.
    - 가능한 가장 낮은 수준의 그래뉼래러티로 설계해야 사용자 쿼리에 유연하게 대응 가능.
- **주의사항**:
    - 그레인은 “전사 데이터 중 가장 낮은 수준”이 아니라 “해당 프로세스 내에서 가장 낮은 수준”이어야 함.

---

## 4️⃣ 팩트들의 그래뉼래러티 일관성 검토

- **핵심 메시지**: 팩트 테이블에 있는 모든 팩트는 동일한 그레인을 공유해야 한다.
- **잘못된 예**:
    - 일자 단위 데이터에 "당년 누적 합계"가 같이 들어 있는 경우: 날짜 필터 시 중복 집계 위험 있음.
- **정리 포인트**:
    - 텍스트 식별자나 플래그는 팩트 테이블에 넣지 말고 디멘션으로 분리할 것.
    - 팩트 테이블은 숫자형 값만, 그 외의 해석 가능한 속성은 디멘션으로!

---

## 5️⃣ 디멘션 그래뉼래러티와 계층

- **핵심 메시지**: 디멘션 테이블의 각 속성은 하나의 값만 가져야 하며, 계층은 디멘션 안에서 명확하게 표현할 것.
- **피해야 할 설계**:
    - 정규화된 디멘션(스노우플레이크): 질의 성능과 사용성 모두 저하.
    - 팩트 테이블에 계층의 외래키를 따로따로 넣는 것: 관리 불가능한 구조로 변함.
    - 한 디멘션 테이블에서 하위/중간 레벨 데이터를 구분해 로우로 관리하는 것: 중복 집계의 위험.
- **권장 설계**:
    - 디멘션은 비정규화 형태로 구성하되, 계층 구조는 한 테이블 안에서 다룰 것.
    - 아웃리거는 예외적으로 허용되지만 남용 금지 (가짓수가 적을 때만 활용).

---

## ✅ 요약: 설계 검토 체크리스트

| 항목 | 체크포인트 |
| --- | --- |
| ✔️ 업무/데이터 균형 | 요건 + 데이터 프로파일링 동시에 수행 |
| ✔️ 프로세스 기반 설계 | 보고서 기반 X, 중심 프로세스 중심으로 |
| ✔️ 팩트 그레인 명확화 | 프로젝트 팀과 공통 이해 형성 필수 |
| ✔️ 집계값 주의 | 합산 가능한 팩트만 팩트 테이블에 포함 |
| ✔️ 디멘션 설계 | 계층은 하나의 디멘션 테이블 내에서 표현, 정규화 지양 |

---

## ✅ 1. 설계 검토 가이드라인

설계 검토를 효과적으로 수행하기 위해 다음과 같은 **사전 준비 사항**과 **검토 중/후 전략**을 따라야 한다.

### 🔹 사전 준비 체크리스트

| 항목 | 설명 |
| --- | --- |
| **참석자 선정** | 모델링팀, 개발팀 대표, 실제 업무요건을 잘 아는 사람 필수. 너무 많은 인원은 비효율적. |
| **검토 리더 지정** | 중립적이거나 이해관계자 중에서 선정. 역할: 논의가 목표에서 벗어나지 않게 조율. |
| **검토 범위 명확화** | 사전 합의를 통해 부수적 주제로 논의가 새는 것 방지. |
| **일정 확보** | 보통 이틀간 집중 세션, 참석자는 이틀 내내 전원 참여해야 함. |
| **장소 예약** | 큰 칠판, 차트, 마커 등 시각적 도구 필요. 다과 제공도 추천. |
| **사전 숙제 부여** | 각자 5가지 개선점 작성 및 사전 제출 → 집중도 향상, 즉흥적 논의 최소화. |

---

### 🔹 검토 중 전략

| 항목 | 설명 |
| --- | --- |
| **열린 자세 유지** | 기존 설계를 맹신하지 말고 개선 가능성에 마음을 열 것. |
| **디지털 기기 제한** | PC/스마트폰 사용 금지 → 집중력 유지. |
| **리더의 강한 조율력** | 주제 이탈 방지, 열린 분위기 유도. |
| **공통 이해 확보** | 설계 목적과 현재 모델에 대해 참가자들이 같은 이해를 갖도록 시작 시 설명. |
| **필기자 지정** | 주요 논의와 결정 사항을 기록할 사람 필요. |
| **큰 그림부터 시작** | Bus Matrix → 우선순위 높은 업무 → 그래뉼래러티 → 관련 디멘션 순으로 점차 상세화. |
| **업무요건 중심 설계** | 모델의 성공 여부는 실제 사용자의 경험 향상 여부로 판단. |
| **샘플 데이터 제공** | 데이터를 보며 논의 시, 모두가 개선사항을 동일하게 이해할 수 있음. |
| **종료 시 요약 필수** | 각자의 과제와 마감일을 명확히 하고 종료. |

---

### 🔹 검토 후 전략

| 항목 | 설명 |
| --- | --- |
| **미해결 이슈 담당자 지정** | 리뷰 후에도 해결이 필요한 이슈는 책임자를 지정하여 추후 처리. |
| **구현 계획 수립** | 개선안의 실현 가능성과 우선순위에 따라 구체적 실행 계획 수립. |
| **재검토 주기 설정** | 모델은 12~24개월마다 정기적으로 재검토 필요. 변경이 많아도 실패가 아니라 자연스러운 진화로 간주해야 함. |

---

## ✅ 2. 설계 초안에 대한 논의

**그림 11-2의 설계 초안**을 기반으로 일반적으로 나타나는 모델링 오류와 개선 방법을 분석한다.

### 🟦 문제 1: 팩트 테이블의 그레인(grain)

- **문제**: ‘월별 청구서 1건당 1로우’가 그레인이라고 했지만, 실제 최소 단위는 **회선별 1로우**.
- **개선**:
    - 팩트 테이블의 그레인을 **회선 단위**로 재정의.
    - 회선 키를 외래 키로 추가.
    - 청구 디멘션에서 회선 키 제거 → **퇴화 디멘션(degenerate dimension)**으로 청구 번호를 처리.
    - 청구일자도 팩트 테이블로 이동 → 일자 디멘션과 조인하도록 수정.

---

### 🟦 문제 2: 스노우플레이크 구조

- **문제**: 영업 채널 디멘션이 계층적으로 나뉘어 **스노우플레이크 형태**로 표현되어 있음.
- **개선**:
    - 영업 채널 식별자 및 속성을 **영업조직 디멘션의 속성**으로 통합.
    - 팩트 테이블에서 불필요한 외래 키 제거.

---

### 🟦 문제 3: 요금제 유형 코드 관리 방식

- **문제**: **요금제 유형 코드가 텍스트 형태로 팩트 테이블에 존재**.
- **개선**:
    - 해당 정보는 **요금제 디멘션의 롤업 속성**으로 관리해야 한다.
    - 코드와 설명은 디멘션 테이블 내 속성으로 분리해 관리할 것.

---

## ✨ 마무리 요약

| 핵심 포인트 |
| --- |
| 모델 설계 검토는 단순 리뷰가 아닌 **조직 전체의 이해관계 조율 과정**이다. |
| **사전 준비**, **집중력 유지**, **업무요건 중심의 논의**, **구현계획 수립**이 필수. |
| 잘못된 그레인 설정, 디멘션의 과도한 복잡화, 비정규화되지 않은 속성은 흔한 실수. |
| **사례 기반의 피드백**과 **실제 샘플 데이터**를 통해 현실적인 개선안 도출 가능. |