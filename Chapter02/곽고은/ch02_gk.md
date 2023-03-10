# Chapter 2. 의미 있는 이름

#### 의도를 분명히 밝혀라
- 변수/함수/클래스의 존재 이유는? 수행 기능은? 사용 방법은?
- 주석이 필요하다면 의도를 분명히 드러내지 않았을 확률이 높다.

#### 그릇된 정보를 피하라
- 일반적으로 통용되는 의미가 있는 단어를 다른 의미로 사용하면 안 된다.
- 서로 다른 모듈에서 흡사한 이름을 사용하지 않도록 주의한다.
- 유사한 개념은 유사한 표기법을 사용한다(일관성).

#### 의미 있게 구분하라
- 연속된 숫자를 붙이거나 불용어를 추가하는 방식은 적절하지 못하다. 
</br>* 불용어: 검색 용어로 사용하지 않는 단어. ex) 관사, 전치사, 조사, 접속사 등
  - char a1, char a2
  - class ProdcutInfo, ProductData
- 읽는 사람이 차이를 알도록 이름을 지어라. 

#### 발음하기 쉬운 이름을 사용하라

#### 검색하기 쉬운 이름을 사용하라
- 이름을 의미있게 지으면 길이가 길어지지만 그래도 검색하기 쉬운 이름이 좋다.

#### 인코딩을 피하라
- 유형이나 범위 정보까지 인코딩에 넣으면 이름을 해독하기 어려워진다.

##### __ 헝가리식 표기법
- 변수 이름에 타입을 인코딩 하지 말라. 이제는 IDE가 오류를 감지해주는 시대!

##### __ 멤버 변수 접두어
- 멤버 변수에 'm_'이라는 접두어를 붙이지 말라.
- 접두어는 old-style 코드이다.

##### __ 인터페이스 클래스와 구현 클래스
- 이 경우에는 인코딩이 필요하다. 인터페이스 클래스 이름과 구현 클래스 이름 중 하나를 인코딩 해야 한다면 구현 클래스 이름을 인코딩 하는 것이 좋다. </br>

|                | BAD           | GOOD                            |
| -------------  |:-------------:| :-----:                         |
| Interface Class| IShapeFactory | ShapeFactory                    |
| Concrete Class | ShapeFactory  | ShapreFactoryImp, CShapeFactory |

#### 자신의 기억력을 자랑하지 마라
- 문자 하나만 사용하는 변수 이름은 문제가 있다. (Loop에서 반복 횟수 세는 변수 제외)

#### 클래스 이름
- 클래스 이름과 객체 이름은 명사/명사구가 적합하다.
- 범용적인 단어(Data, Info 등)는 피하고, 동사는 사용하지 않는다. 

#### 메서드 이름
- 동사/동사구가 적합하다.
- Accessor(접근자), Mutator(변경자), Predicate(조건자)는 앞에 Get, Set, Is를 붙인다.
- Constructor를 Overloading 할 때는 Static Factory Method를 사용한다.
</br> * Static Factory Method: 객체 생성 역할을 하는 클래스 메서드

#### 기발한 이름은 피하라
- 누구나 알아볼 수 있는 이름을 선택하라.
- 명료하고 의도를 분명히 표현하라.

#### 한 개념에 한 단어를 사용하라
- 개념 하나에 단어 하나를 선택해 이를 고수한다. 메서드 이름은 일관적이어야 한다.
</br> * ex) 똑같은 메서드를 클래스마다 fetch, retrieve, get으로 각각 부르지 않기!

#### 말장난을 하지 마라
- 한 단어를 두 가지 목적으로 사용하지 말라.

#### 해법 영역에서 가져온 이름을 사용하라
- 기술 개념에는 기술 이름이 가장 적합한 선택이다.

#### 문제 영역에서 가져온 이름을 사용하라
- 적절한 프로그래밍 언어가 없다면 Domain 영역에서 이름을 지어라.

#### 의미 있는 맥락을 추가하라
- 클래스/함수의 이름에 의미를 넣을 수 없다면 마지막 수단으로 접두어를 붙인다.
  - addrFirstName, addrLastName 

#### 불필요한 맥락을 없애라
- 이름에 불필요한 부분을 넣지 말라.
</br> * ex) 모든 클래스의 이름 앞에 프로젝트 이름을 붙이지 말라.
