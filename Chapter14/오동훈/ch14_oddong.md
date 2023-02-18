
# 14장 - 점진적인 개선
---

## 1. Args 구현

```java
public static void main(String[] args) {
    try {
        Args arg = new Args("l, p$, d*", args);
        boolean logging = arg.getBoolean('l');
        int port = arg.getInt('p');
        String directory = arg.getString('d');
        ececuteApplication(logging, port, directory);
    } catch (ArgsException e) {
        System.out.println("Argument error: %s\n", e.errorMessage());
    }
}
```

- Args는 사용법이 간단하다. Args 생성자에 (입력으로 들어온) 인수 문자열과 형식 문자열을 넘겨 Args 인스턴스를 생성한 후 Args 인스턴스에다 인수 값을 질의한다.

- 매개변수 두 개로 Args 클래스의 클래스의 인스턴스를 만든다. 첫째 매개변수는 형식 또는 스키마를 지정한다. "l,p#,d*"은 명령행 인수 세 개를 정의한다. 첫 번째 -l은 부울 인수다. 두 번째 -p는 정수 인수다. 세 번째 -d는 문자열 인수다. 두 번째 매개변수는 main으로 넘어온 명령행 인수 배열 자체다.

- ArgsException이 발생하지 않는다면 명령행 인수의 구문을 성공적으로 분석했으며 Args 인스턴스에 질의를 던져도 좋다는 말이다.

- 형식 문자열이나 명령행 인수 자체에 문제가 있으면 ArgsException이 발생한다.
- 구체적인 오류를 알아아내려면 예외가 제공하는 errorMessage 메서드를 사용한다.

### 1. 어떻게 짰느냐고?

- 깨끗한 코드를 짜려면 먼저 지저분한 코드를 짠 뒤에 정리해야 한다.

---
## 2. Args: 1차 초안
- 맨 처음 짰던 Args 클래스다. 코드는 돌아가지만 엉망이다.

```java
import java.text.ParseException; 
import java.util.*;
 
public class Args {
  private String schema;
  private String[] args;
  private boolean valid = true;
  private Set<Character> unexpectedArguments = new TreeSet<Character>(); 
  private Map<Character, Boolean> booleanArgs = new HashMap<Character, Boolean>();
  private Map<Character, String> stringArgs = new HashMap<Character, String>(); 
  private Map<Character, Integer> intArgs = new HashMap<Character, Integer>(); 
  private Set<Character> argsFound = new HashSet<Character>();
  private int currentArgument;
  private char errorArgumentId = '\0';
  private String errorParameter = "TILT";
  private ErrorCode errorCode = ErrorCode.OK;
 
  private enum ErrorCode {
    OK, MISSING_STRING, MISSING_INTEGER, INVALID_INTEGER, UNEXPECTED_ARGUMENT}
 
  public Args(String schema, String[] args) throws ParseException { 
    this.schema = schema;
    this.args = args;
    valid = parse();
  }
 
  private boolean parse() throws ParseException { 
    if (schema.length() == 0 && args.length == 0)
      return true; 
    parseSchema(); 
    try {
      parseArguments();
    } catch (ArgsException e) {
    }
    return valid;
  }
 
  private boolean parseSchema() throws ParseException { 
    for (String element : schema.split(",")) {
      if (element.length() > 0) {
        String trimmedElement = element.trim(); 
        parseSchemaElement(trimmedElement);
      } 
    }
    return true; 
  }
 
  private void parseSchemaElement(String element) throws ParseException { 
    char elementId = element.charAt(0);
    String elementTail = element.substring(1); 
    validateSchemaElementId(elementId);
    if (isBooleanSchemaElement(elementTail)) 
      parseBooleanSchemaElement(elementId);
    else if (isStringSchemaElement(elementTail)) 
      parseStringSchemaElement(elementId);
    else if (isIntegerSchemaElement(elementTail)) 
      parseIntegerSchemaElement(elementId);
    else
      throw new ParseException(String.format("Argument: %c has invalid format: %s.", 
        elementId, elementTail), 0);
    } 
  }
 
  private void validateSchemaElementId(char elementId) throws ParseException { 
    if (!Character.isLetter(elementId)) {
      throw new ParseException("Bad character:" + elementId + "in Args format: " + schema, 0);
    }
  }
 
  private void parseBooleanSchemaElement(char elementId) { 
    booleanArgs.put(elementId, false);
  }
 
  private void parseIntegerSchemaElement(char elementId) { 
    intArgs.put(elementId, 0);
  }
 
  private void parseStringSchemaElement(char elementId) { 
    stringArgs.put(elementId, "");
  }
 
  private boolean isStringSchemaElement(String elementTail) { 
    return elementTail.equals("*");
  }
 
  private boolean isBooleanSchemaElement(String elementTail) { 
    return elementTail.length() == 0;
  }
 
  private boolean isIntegerSchemaElement(String elementTail) { 
    return elementTail.equals("#");
  }
 
  private boolean parseArguments() throws ArgsException {
    for (currentArgument = 0; currentArgument < args.length; currentArgument++) {
      String arg = args[currentArgument];
      parseArgument(arg); 
    }
    return true; 
  }
 
  private void parseArgument(String arg) throws ArgsException { 
    if (arg.startsWith("-"))
      parseElements(arg); 
  }
 
  private void parseElements(String arg) throws ArgsException { 
    for (int i = 1; i < arg.length(); i++)
      parseElement(arg.charAt(i)); 
  }
 
  private void parseElement(char argChar) throws ArgsException { 
    if (setArgument(argChar))
      argsFound.add(argChar); 
    else 
      unexpectedArguments.add(argChar); 
      errorCode = ErrorCode.UNEXPECTED_ARGUMENT; 
      valid = false;
  }
 
  private boolean setArgument(char argChar) throws ArgsException { 
    if (isBooleanArg(argChar))
      setBooleanArg(argChar, true); 
    else if (isStringArg(argChar))
      setStringArg(argChar); 
    else if (isIntArg(argChar))
      setIntArg(argChar); 
    else
      return false;
 
    return true; 
  }
 
  private boolean isIntArg(char argChar) {
    return intArgs.containsKey(argChar);
  }
 
  private void setIntArg(char argChar) throws ArgsException { 
    currentArgument++;
    String parameter = null;
    try {
      parameter = args[currentArgument];
      intArgs.put(argChar, new Integer(parameter)); 
    } catch (ArrayIndexOutOfBoundsException e) {
      valid = false;
      errorArgumentId = argChar;
      errorCode = ErrorCode.MISSING_INTEGER;
      throw new ArgsException();
    } catch (NumberFormatException e) {
      valid = false;
      errorArgumentId = argChar; 
      errorParameter = parameter;
      errorCode = ErrorCode.INVALID_INTEGER; 
      throw new ArgsException();
    } 
  }
 
  private void setStringArg(char argChar) throws ArgsException { 
    currentArgument++;
    try {
      stringArgs.put(argChar, args[currentArgument]); 
    } catch (ArrayIndexOutOfBoundsException e) {
      valid = false;
      errorArgumentId = argChar;
      errorCode = ErrorCode.MISSING_STRING; 
      throw new ArgsException();
    } 
  }
 
  private boolean isStringArg(char argChar) { 
    return stringArgs.containsKey(argChar);
  }
 
  private void setBooleanArg(char argChar, boolean value) { 
    booleanArgs.put(argChar, value);
  }
 
  private boolean isBooleanArg(char argChar) { 
    return booleanArgs.containsKey(argChar);
  }
 
  public int cardinality() { 
    return argsFound.size();
  }
 
  public String usage() { 
    if (schema.length() > 0)
      return "-[" + schema + "]"; 
    else
      return ""; 
  }
 
  public String errorMessage() throws Exception { 
    switch (errorCode) {
      case OK:
        throw new Exception("TILT: Should not get here.");
      case UNEXPECTED_ARGUMENT:
        return unexpectedArgumentMessage();
      case MISSING_STRING:
        return String.format("Could not find string parameter for -%c.", errorArgumentId);
      case INVALID_INTEGER:
        return String.format("Argument -%c expects an integer but was '%s'.", errorArgumentId, errorParameter);
      case MISSING_INTEGER:
        return String.format("Could not find integer parameter for -%c.", errorArgumentId);
    }
    return ""; 
  }
 
  private String unexpectedArgumentMessage() {
    StringBuffer message = new StringBuffer("Argument(s) -"); 
    for (char c : unexpectedArguments) {
      message.append(c); 
    }
    message.append(" unexpected.");
 
    return message.toString(); 
  }
 
  private boolean falseIfNull(Boolean b) { 
    return b != null && b;
  }
 
  private int zeroIfNull(Integer i) { 
    return i == null ? 0 : i;
  }
 
  private String blankIfNull(String s) { 
    return s == null ? "" : s;
  }
 
  public String getString(char arg) { 
    return blankIfNull(stringArgs.get(arg));
  }
 
  public int getInt(char arg) {
    return zeroIfNull(intArgs.get(arg));
  }
 
  public boolean getBoolean(char arg) { 
    return falseIfNull(booleanArgs.get(arg));
  }
 
  public boolean has(char arg) { 
    return argsFound.contains(arg);
  }
 
  public boolean isValid() { 
    return valid;
  }
 
  private class ArgsException extends Exception {
  } 
}
```


### 1. 그래서 멈췄다

- 위의 코드에 추가하면 원하던 프로그램은 완성할 수 있었겠지만,그랬다가는 너무 커서 손대기 어려운 골칫거리가 생겨날 참이었다. 그래서 기능을 더 이상 추가하지 않기로 결정하고 리팩터링을 시작했다. String 인수 유형과 Integer 인수 유형을 추가한 경험에서 새 유형을 추가하려면 주요 지점 세 곳에다 코드를 추가해야 한다는 사실을 이미 깨달았다.

> 1. 인수 유형에 해당하는 HashMap을 선택하기 위해 스키마 요소의 구문을 분석
> 2. 명령행 인수에서 인수 유형을 분석해 진짜 유형으로 변환
> 3. getXXX 메서드를 구현해 호출자에게 진짜 유형을 반환 

### 2. 점진적으로 개선하다
- 프로그램을 망치는 가장 좋은 방법 중 하나는 개선이라는 이름 아래 구조를 크게 뒤집는 행위다.

- 따라서 TDD가 필요하다. TDD는 언제 어디서라도 시스템이 돌아가야 한다는 원칙을 따른다. 다시 말해 TDD는 시스템을 망가뜨리는 변경을 허용하지 않는다. 변경을 가한 후에도 시스템이 변경 전과 똑같이 돌아가야 한다.
