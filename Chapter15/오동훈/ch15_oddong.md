
# 15장 - Junit

---

- JUnit : 자바 프로그래밍 언어용 유닛 테스트 프레임워크


### 보이스카우트 규칙
- 체크 아웃때보다 더 좋은 코드를 체크인 한다. 즉, 코드 정리를 거듭할수록 더 좋은 코드가 되어야 한다는 의미

#### 1. 인코딩을 피하라
- 이름에 유형 정보나 범위 정보를 나타내는 접두어를 붙이는 것은 불필요하다. 따라서 멤버 변수 앞에 붙인 접두어 f를 제거한다.

```java
private int contextLength;
private String expected;
private String actual;
private int prefix;
private int suffix;
```

#### 2. 조건을 캡슐화 하라.
아래 compact 함수 시작부에 캡슐화되지 않은 조건문이 보인다.

```java
public String compact(String message) {
    if (expected == null || actual == null || areStringsEqual()) { // 이부분
        return Assert.format(message, expected, actual);
    }

    findCommonPrefix();
    findCommonSuffix();
    String expected = compactString(this.expected);
    String actual = compactString(this.actual);
    return Assert.format(message, expected, actual);
}
```

의도를 명확하게 표현하기 위해 조건문을 캡슐화하는 과정을 거친다. 즉, 조건문을 메서드로 뽑아내 이름을 붙여 아래와 같이 변경한다.

```java
public String compact(String message) {
    if (shouldNotCompact()) {
        return Assert.format(message, expected, actual);
    }

    findCommonPrefix();
    findCommonSuffix();
    String expected = compactString(this.expected);
    String actual = compactString(this.actual);
    return Assert.format(message, expected, actual);
}

private boolean shouldNotCompact() { // 조건문을 함수로 뽑아낸다.
    return expected == null || actual == null || areStringsEqual();
}
```


#### 3. 가능하다면 표준 명명법을 사용하라
#### 4. 부정 조건은 피하라
- 부정문은 긍정문보다 이해하기 약간 더 어렵다. 그러므로 첫 문장 if를 긍정으로 만들어 조건문을 반전한다.

#### 5. 이름으로 부수 효과를 설명하라
- compact 함수는 문자열 압축을 위한 함수이지만, 실제로 canBeCompacted가 false이면 압축하지 않는다. 이렇게 부가 단계가 숨겨지는 이름은 좋지 못하다. 이름은 함수, 변수, 클래스가 하는 일을 모두 담고 있는 것으로 사용해야 한다.

- 그리고 단순한 압축 문자열이 아닌 형식이 갖춰진 문자열을 반환하기 때문에 실제로는 formatCompactedComparison이라는 이름이 적합하다.

#### 6. 함수는 한 가지만 해야 한다
- `formatCompactedComparison` 내의 if문 안에서는 예상 문자열과 실제 문자열을 압축한다. 이 부분을 따로 빼내서 `compactExpectedAndActual`이라는 메서드로 만들어 압축을 수행하도록하고, `formatCompactedComparison` 함수는 형식을 맞추는 작업만 할 수 있도록 한다.
그렇게 되면 각 함수는 하나의 기능만 수행하게 된다.

```java
private String compactExpected; 
private String compactActual;

...

public String formatCompactedComparison(String message) {
    if (canBeCompacted()) {
        compactExpectedAndActual();
        return Assert.format(message, compactExpected, compactActual);
    } else {
        return Assert.format(message, expected, actual);
    }       
}

private compactExpectedAndActual() {
    findCommonPrefix();
    findCommonSuffix();
    compactExpected = compactString(expected);
    compactActual = compactString(actual);
}
```

#### 7. 일관성 부족

#### 8. 서술적인 이름을 사용하라
- 멤버 변수가 색인 위치를 나타내기 때문에 더 명확한 의미를 주기 위해 prefix, suffix로 선언되어있던 것을 prefixIndex, suffixIndex로 수정한다.

```java
private int prefixIndex;
private int suffixIndex;
```

#### 9. 숨겨진 시각적인 결합
#### 10. 일관성을 유지하라
- 코드를 구조를 짤때는 이유를 고민하고, 그 이유를 코드 구조가 명백히 표현할 수 있도록 해야한다. 그래야 일관성있는 구조를 만들 수 있다.



#### 11. 경계 조건을 캡슐화하라
- 경계 조건은 한 곳에서 별도로 처리한다.

#### 12. 죽은 코드
- 죽은 코드란 실행되지 않는 코드를 말한다. 예를 들어 throw문이 없는 try catch 문이나 방금 본 불필요한 if문 같은 것들이 있다.

- 이들은 시스템에서 제거해주는것이 좋다. 따라서 if문을 제거하고 구조를 다듬자.
