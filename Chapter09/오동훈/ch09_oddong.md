
# 9장 단위 테스트
    

## 1. TDD 법칙 세 가지
- 첫째 법칙: 실패하는 단위 테스트를 작성할 때까지 실제 코드를 작성하지 않는다.
- 둘째 법칙: 컴파일은 실패하지 않으면서 실행이 실패하는 정도로만 단위 테스트를 작성한다.
- 셋째 법칙: 현재 실패하는 테스트를 통과할 정도로만 실제 코드를 작성한다.

위 세 가지 규칙을 따르면 개발과 테스트가 대략 30초 주기로 묶인다. (?? 어케 가능하지)


## 2. 깨끗한 테스트 코드 유지하기
### 1. 테스트는 유연성, 유지보수성, 재사용성을 제공한다
- 실제 코드를 점검하는 자동화된 단위 테스트 슈트는 설계와 아키텍쳐를 최대한 개끗하게 보존하는 열쇠다. 테스트는 유연성, 유지 보수성, 재사용성을 제공하는데, 그 이유는 테스트 케이스가 있으면 변경이 쉬워지기 때문이다.

## 3.깨끗한 테스트 코드
- 깨끗한 코드를 만드려면 필수적인게 가독성, 가독성, 가독성, 가독성, 가독성이다~!!!
- 테스트 코드는 최소의 표현으로 많은 것을 나타내야 한다.  

```java
public void testGetPageHierarchyAsXml() throws Exception {
  makePages("PageOne", "PageOne.ChildOne", "PageTwo");

  submitRequest("root", "type:pages");

  assertResponseIsXML();
  assertResponseContains(
    "<name>PageOne</name>", "<name>PageTwo</name>", "<name>ChildOne</name>");
}

public void testSymbolicLinksAreNotInXmlPageHierarchy() throws Exception {
  WikiPage page = makePage("PageOne");
  makePages("PageOne.ChildOne", "PageTwo");

  addLinkTo(page, "PageTwo", "SymPage");

  submitRequest("root", "type:pages");

  assertResponseIsXML();
  assertResponseContains(
    "<name>PageOne</name>", "<name>PageTwo</name>", "<name>ChildOne</name>");
  assertResponseDoesNotContain("SymPage");
}

public void testGetDataAsXml() throws Exception {
  makePageWithContent("TestPageOne", "test page");

  submitRequest("TestPageOne", "type:data");

  assertResponseIsXML();
  assertResponseContains("test page", "<Test");
}
```

BUILD-OPERATE-CHECK 패턴이 위와 같은 테스트 구조에 적합하다.

1. 첫 부분 : 테스트 자료 만들기
2. 두 번째 : 테스트 자료 조작
3. 세 번째 : 조작한 결과가 올바른지 확인

### 1. 도메인에 특화된 테스트 언어
### 2. 이중 표준
## 4. 테스트 당 assert 하나
- JUnit으로 테스트 코드를 짤 때는 함수마다 assert문 하나만 사용해야한다.
### 1. 테스트 당 개념 하나
- 테스트 함수마다 한 개념만 테스트해라. 독자적인 개념 세 개를 테스트할거면 독자적인 테스트 세 개로 쪼개자.

## 5. F.I.R.S.T.

**F(Fast)**
- 테스트는 빨라야 한다. 빨리 돌아야 하고 느릴수록 자주 돌릴 엄두를 내지 못한다. 자주 돌리지 못하면 초반에 문제를 찾기 힘들다.

**I(Independent)**
- 테스트는 서로 의존하면 안된다. 한 테스트가 다음 테스트가 실행될 환경을 준비해서는 안된다. 각 테스트는 독립적이여야 한다. 

**R(Repeatable)**
- 테스트는 어떤 환경에서도 반복 가능해야한다.

**S(Self-Validating)**
- 테스트는 부울 값으로 결과를 내야한다. 수작업으로 비교해서 안된다.

**T(Timely)**
테스트는 적시에 작성해야 한다. 실제 코드보다 먼저 테스트 코드를 작성해야 실제 코드가 테스트하기 어렵다는 사실을 알게될 수 있다.
