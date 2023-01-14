# Chapter 3. 함수
    
#### 작게 만들어라!
##### __ 블록과 들여쓰기
- if문/else문/while문 등에 들어가는 블록은 **한 줄**이어야 한다. 대개 이 한 줄이 함수를 호출한다.
</br> 그러면 enclosing function이 작아지고, 블록 안에서 호출하는 함수 이름을 적절히 지으면 코드 이해도 쉬워진다.
```java
if (isTestPage(pageData))
{
  includeSetupPages(pageData, isSuite);
}
```
- 함수에서 들여쓰기 수준은 1단이나 2단을 넘어서면 안 된다.

#### 한 가지만 해라!
- 함수는 한 가지만 해야 한다.
##### __ 함수 내 섹션
- 함수 내에서 섹션이 여러 개로 나뉜다는 것은 여러 작업을 한다는 증거다. 한 가지 작업만 하면 섹션을 나누기 어렵다.

#### 함수 당 추상화 수준은 하나로!

##### __ 위에서 아래로 코드 읽기: 내려가기 규칙

#### Switch 문
- Switch문은 다형적 객체를 생성하는 코드에서만 사용하자.

#### 서술적인 이름을 사용하라!
-길고 서술적인 이름이 짧고 어려운 이름보다 좋다.

#### 함수 인수
- 파라미터가 0개인 것이 가장 이상적이며, 파라미터 개수가 적을수록 좋다.
- 함수 파라미터로 넘기는 방법보다는 인스턴스 변수로 선언해서 사용하는 방법을 추천한다.
- 테스트 케이스를 작성할 때도 파라미터가 없는 게 테스트하기 용이하다.

##### __ 많이 쓰는 단항 형식
- 파라미터가 1개인 함수
  - 파라미터에 질문을 던지는 경우. ex) `boolean fileExists("MyFile")`
  - 파라미터를 변환해 결과를 반환하는 경우. ex) `InputStream fileOpen("MyFile")`
 - 변환 함수에서 출력 파라미터를 사용하기보다는 입력 파라미터를 변환하여 변환 결과는 return값으로 돌려주는 것이 좋다.

##### __ 플래그 인수
- 파라미터로 bool 변수 사용하는 것은 함수가 한꺼번에 여러 작업을 한다는 의미이므로 좋지 않다.

##### __ 이항 함수
- 파라미터가 2개인 함수

##### __ 삼항 함수
- 파라미터가 3개인 함수

##### __ 인수 객체
- 파라미터가 2-3개 필요하다면 일부를 독자적인 **클래스 변수로 선언**할 가능성을 생각해보자.
```java
Circle makeCircle(double x, double y, double radius);
Circle makeCircle(Point center, double radius);
```

##### __ 인수 목록
- 때로는 인수 개수가 가변적인 함수도 필요하다. 
</br> ex) `String.format("", name, hours);` </br> `public String format(String format, Object... args)`

##### __ 동사와 키워드
- 단항 함수는 함수가 동사, 파라미터가 명사로 쌍을 이루어야 한다.
- 함수 이름에 키워드로 파라미터 이름을 넣는다.
</br> `assertEquals()` </br> `assertExpectedEqualsActual(expected, actual)`

#### 부수 효과를 일으키지 마라!
- 부수 효과는 temporal coupling이나 order dependency를 일으킨다.

##### __ 출력 인수
- 일반적으로 출력 파라미터는 피해야 한다.
- 함수에서 상태를 변경해야 하다며 함수가 속한 객체 상태를 변경하는 방식을 선택해라.

#### 명령과 조회를 분리하라!
- 뭔가를 수행(객체 상태 변경)하거나 뭔가를 답하거나(객체 정보 반환) 둘 중 하나만 해야 한다.
- 
#### 오류 코드보다 예외를 사용하라!

##### __ Try/Catch 블록 뽑아내기
- 정상 동작과 오류 처리 동작을 분리하라.
```java
public void delete(Page page)
{
  try
  {
    deletePageAndAllReferences(page);
  }
  catch(Exception e)
  {
    logError(e);
  }
}

private void deletePageAndAllReferences(Page page) throws Exception
{
  deletePage(page);
  registry.deleteReference(page.name);
  configKeys.deleteKey(page.name.makeKey());
}

private void logError(Exception e)
{
  logger.log(e.getMessage());
}
```

##### __ 오류 처리도 한 가지 작업이다.
- 오류 처리하는 함수는 오류만 처리해야 한다.
</br> 함수에 키워드 try가 있다면 함수는 try문으로 시작해 catch/finally 문으로 끝나야 한다.

##### __ Error.java 의존성 자석

#### 반복하지 마라!
- 관계형 데이터베이스에서는 정규 형식을 만들어 중복을 제거한다.
- 객체 지향 프로그래밍은 코드를 부모 클래스로 몰아 중복을 제거한다.

#### 구조적 프로그래밍
- 함수는 return문이 하나여야 한다. loop 안에서 break, continue를 사용해선 안 되며 goto는 절대로 안 된다. (단, 함수가 작다면 OK)

#### 함수를 어떻게 짜죠?
- 처음에는 완벽하지 않게 코드를 작성하고, 그 뒤로 리팩토링을 여러 번 해라.
- 처음 코드를 작성할 때부터 단위 테스트를 만들고, 리팩토링 하면서도 단위 테스트를 통과시킨다.
