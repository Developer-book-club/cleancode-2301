
# 17장 -

---

## 1. 주석
### C1: 부적절한 정보
- 변경 이력, 이슈 추적 등 다른 시스템에 저장할 정보는 주석으로 적절하지 못하다. 따라서 작성자, 최종 수정일, SRP 번호 같은 정보만 주석으로 넣는다.

- 주석은 코드와 설계에 기술적인 설명을 부연하는 수단이기 때문에 필수적인 정보만 넣어준다.

### C2: 쓸모 없는 주석
- 쓸모 없어진 주석은 빨리 삭제하자 (오래된, 엉뚱한, 잘못된 주석)

### C3: 중복된 주석
- 코드만으로 충분한데 구구절절 설명하는 주석은 삭제하자.

### C4: 성의 없는 주석
- 작성할 가치가 있는 주석은 잘 작성할 가치가 있다. 간단하고 명료하게 적자.

### C5: 주석 처리된 코드
- 주석으로 처리된 코드는 얼마나 오래됐는지, 중요한 코드인지 아무도 모른다. 헷갈리게 하지 말고 지워버리자.


## 2. 환경

### E1: 여러 단계로 빌드해야 한다
- 한 명령으로 전체를 체크아웃해서 한 명령으로 빌드할 수 있어야 한다.


### E2: 여러 단계로 테스트해야 한다
- 모든 단위 테스트는 한 명령으로 돌려야 한다.


## 3. 함수

### F1: 너무 많은 인수
- 함수에서 인수 개수는 작을수록 좋다. 아예 없으면 더 좋다.

### F2: 출력 인수
- 조회 : 입력인수 (ex) fileExists("Myfile"))
- 변환 : 출력인수 (ex) appendFooter(StringBuffer report))

### F3: 플래그 인수
- boolean 인수는 되도록 피하자

### F4: 죽은 함수
- 아무도 호출하지 않는 함수는 삭제하자

## 4. 일반
### G1: 한 소스 파일에 여러 언어를 사용한다
- 요즘는 다양한 언어를 지원해 섞이는 경우가 많은데, 되도록이면 소스 파일 내에는 언어 하나만 쓰도록 하자

### G2: 당연한 동작을 구현하지 않는다
- 라이브러리가 있으면 가져다 쓰고 직접 구현해서 낭비하지 말자

### G3: 경계를 올바로 처리하지 않는다
- 모든 경계와 구석진 곳에서 코드를 증명하려 애쓰지 않는데, 스스로의 직관에 의존하지 말고 모든 경계 조건을 찾아내 테스트 케이스를 작성하자

### G4: 안전 절차 무시
- 테스트 케이스를 무시하지 말자 절대!

### G5: 중복
- 코드에서 중복을 발견할 때마다 추상화할 기회로 간주하라

### G6: 추상화 수준이 올바르지 못하다
- 모든 저차원 개념은 파생 클래스에 넣고, 모든 고차원 개념은 기초 클래스에 넣자

### G7: 기초 클래스가 파생 클래스에 의존한다


### G8: 과도한 정보
- 잘 정의된 모듈이나 함수는 작고 결합도가 낮다.

### G9: 죽은 코드
- 실행되지 않는 코드는 장례식 치뤄주자

### G10: 수직 분리
- 변수와 함수는 사용되는 위치에 가깝게 정의한다. 지역 변수는 처음으로 사용하기 직전에 선언하며 수직으로 가까운 곳에 위치해야 한다.

### G11: 일관성 부족
- 일관성있게 착실하게 적용해 나간다면 코드를 읽고 수정하기가 굉장히 편해진다.

### G12: 잡동사니
- 아무도 사용하지 않는 변수, 함수, 주석 등 필요 없는 것들은 지우자

### G13: 인위적 결합
- 서로 무관한 개념을 인위적으로 결합하지 말자

### G14: 기능 욕심
### G15: 선택자 인수

- 인수를 넘겨 추가 동작을 하게 만들거라면 함수를 따로 만들어 분리시키자

### G16: 모호한 의도
- 코드를 짤 때는 의도를 최대한 분명히 밝히자

### G17: 잘못 지운 책임
- 

### G18: 부적절한 static 함수
### G19: 서술적 변수
### G20: 이름과 기능이 일치하는 함수
- 이름과 기능이 일치할 수 있도록 구현하자

### G21: 알고리즘을 이해하라
- 알고리즘을 이해해야 코드도 간결하게 짤 수 있다

### G22: 논리적 의존성은 물리적으로 드러내라
### G23: If/Else 혹은 Switch/Case 문보다 다형성을 사용하라
### G24: 표준 표기법을 따르라
### G25: 매직 숫자는 명명된 상수로 교체하라
### G26: 정확하라
- 결정을 내리는 이유와 예외를 처리할 방법을 분명히 알아야 한다.

### G27: 관례보다 구조를 사용하라
- 설계 결정을 강제할 때는 규칙보다 관례를 사용한다. 

### G28: 조건을 캡슐화하라
- 조건이 2개 이상이면 캡슐화 해라

### G29: 부정 조건은 피하라
- 부정 조건은 긍정 조건보다 이해하기 어렵다. 되도록 피하자

### G30: 함수는 한 가지만 해야 한다
- 함수는 하나의 일만 하도록 하자

### G31: 숨겨진 시간적인 결합

### G32: 일관성을 유지하라

### G33: 경계 조건을 캡슐화하라
### G34: 함수는 추상화 수준을 한 단계만 내려가야 한다
- 함수 내 모든 문장은 추상화 수준이 동일해야 한다. 그리고 그 추상화 수준은 함수 이름이 의미하는 작업보다 한 단계만 낮아야 한다.

### G35: 설정 정보는 최상위 단계에 둬라

### G36: 추이적 탐색을 피하라

## 5. 자바
### J1: 긴 import 목록을 피하고 와일드카드를 사용하라
- 패키지에서 클래스를 둘 이상 사용한다면 와일드카드를 사용해 패키지 전체를 가져와라

### J2: 상수는 상속하지 않는다
### J3: 상수 대 Enum

## 6. 이름

### N1: 서술적인 이름을 사용하라
- 이름 신중하게 지어라
### N2: 적절한 추상화 수준에서 이름을 선택하라
- 작업 대상 클래스나 함수가 위치하는 추상화 수준을 반영하는 이름을 선택해라

### N3: 가능하다면 표준 명명법을 사용하라
- 기존 명명법을 사용하는 이름은 이해하기가 쉽다

### N4: 명확한 이름
- 함수나 변수의 목적을 명확히 밝히는 이름을 선택한다.

### N5: 긴 범위는 긴 이름을 사용하라
- 

### N6: 인코딩을 피하라

### N7: 이름으로 부수 효과를 설명하라
- 함수, 변수, 클래스가 하는 일을 모두 기술하는 이름을 사용해라


## 7. 테스트
### T1: 불충분한 테스트
### T2: 커버리지 도구를 사용하라!
### T3: 사소한 테스트를 건너뛰지 마라
### T4: 무시한 테스트는 모호함을 뜻한다
### T5: 경계 조건을 테스트하라
### T6: 버그 주변은 철저히 테스트하라
### T7: 실패 패턴을 살펴라
### T8: 테스트 커버리지 패턴을 살펴라
### T9: 테스트는 빨라야 한다

결론